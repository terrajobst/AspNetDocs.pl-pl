---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Wprowadzenie z formularzami sieci Web ASP.NET 4,7 i Visual Studio 2017 | Microsoft Docs
author: Erikre
description: W tej serii samouczków krok po kroku przedstawiono podstawowe informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,7 i Microsoft Visual Studio
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 52d5eb7abe4520ebdf6667d299d055fc7619a635
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74615452"
---
# <a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a>Wprowadzenie z formularzami sieci Web ASP.NET 4,5 i programem Visual Studio 2017

[Pobierz program Wingtip zabawki (C#)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

W tej serii samouczków pokazano, jak utworzyć aplikację ASP.NET Web Forms z ASP.NET 4,5 i Microsoft Visual Studio 2017. 

## <a name="introduction"></a>Wprowadzenie

Ta seria samouczków prowadzi użytkownika przez proces tworzenia aplikacji ASP.NET Web Forms przy użyciu programu Visual Studio 2017 i ASP.NET 4,5. Utworzysz aplikację o nazwie **zabawki Wingtip** — uproszczona witryna sieci Web w sklepie do sprzedaży elementów sprzedawanych w trybie online. W ramach serii są wyróżnione nowe funkcje ASP.NET 4,5.

### <a name="target-audience"></a>Odbiorcy docelowi

Deweloperzy, którzy korzystają z formularzy sieci Web ASP.NET, są docelowymi odbiorcami tej serii samouczków.

Należy mieć pewną wiedzę w następujących obszarach:

- Programowanie zorientowane obiektowo (OOP) i Języki
- Programowanie dla sieci Web (HTML, CSS, JavaScript)
- Relacyjne bazy danych
- Architektura N-warstwowa

Aby przejrzeć te obszary, Rozważ zbadanie następującej zawartości:

- [Wprowadzenie z wizualizacjąC#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Programowanie dla sieci Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, php, jQuery](http://w3schools.com/)
- [Relacyjna baza danych](http://en.wikipedia.org/wiki/Relational_database)
- [Architektura wielowarstwowa](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Funkcje aplikacji

Funkcje formularza sieci Web ASP.NET przedstawione w tej serii obejmują:

- Projekt aplikacji sieci Web (nie projekt witryny sieci Web)
- Formularze sieci Web
- Strony wzorcowe, konfiguracja
- Bootstrap
- Entity Framework Code First, LocalDB
- Żądaj weryfikacji
- Kontrolki danych o jednoznacznie określonym typie
- Powiązanie modelu
- Adnotacje danych
- Dostawcy wartości
- SSL i OAuth
- ASP.NET Identity, konfiguracja i autoryzacja
- Niezauważalna weryfikacja
- Routing
- Obsługa błędów platformy ASP.NET

### <a name="application-scenarios-and-tasks"></a>Scenariusze i zadania aplikacji

Zadania serii samouczków obejmują:

- Tworzenie, przeglądanie i uruchamianie nowego projektu
- Tworzenie struktury bazy danych
- Inicjowanie i wypełnianie bazy danych
- Dostosowywanie interfejsu użytkownika za pomocą stylów, grafiki i strony wzorcowej
- Dodawanie stron i nawigowania
- Wyświetlanie szczegółów menu i danych produktu
- Tworzenie koszyka
- Dodawanie obsługi protokołów SSL i OAuth
- Dodawanie metody płatności
- Dołączenie roli administratora i użytkownika do aplikacji
- Ograniczanie dostępu do określonych stron i folderów
- Przekazywanie pliku do aplikacji sieci Web
- Implementowanie walidacji danych wejściowych
- Rejestrowanie tras dla aplikacji sieci Web
- Implementowanie obsługi błędów i rejestrowania błędów

## <a name="overview"></a>Omówienie

Ta seria samouczków jest przeznaczona dla kogoś, kto zna koncepcje programowania, ale nowy ASP.NET formularzy sieci Web. Jeśli masz już doświadczenie w korzystaniu z formularzy sieci Web ASP.NET, ta seria może pomóc Ci w Poznaniu nowych funkcji ASP.NET 4,5. W przypadku czytelników nieznających pojęć związanych z programowaniem i formularzy sieci Web ASP.NET zapoznaj się z samouczkami dotyczącymi dodatkowych formularzy sieci Web znajdujących się w sekcji [wprowadzenie](../../../index.md) w witrynie sieci Web ASP.NET.

ASP.NET 4,5 podany w tej serii samouczków obejmuje następujące funkcje:

- Prosty interfejs użytkownika do tworzenia projektów, które oferują [obsługę wielu platform ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC i Web API).
- Struktura projektu [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), układu, tworzenia i reagowania.
- [ASP.NET Identity](../../../../identity/index.md)nowy system członkostwa ASP.NET, który działa tak samo we wszystkich strukturach ASP.NET i współpracuje z oprogramowaniem hostingu w sieci Web innym niż IIS.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)

  Aktualizacja Entity Framework umożliwiająca:
  - Pobieranie i manipulowanie danymi jako obiektami o jednoznacznie określonym typie
  - Dostęp do danych asynchronicznie
  - Obsługa błędów przejściowych połączeń
  - Rejestruj instrukcje SQL

Aby zapoznać się z pełną listą funkcji ASP.NET 4,5, zobacz [ASP.NET and Web Tools do Visual Studio 2013 informacji o wersji](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>Przykładowa aplikacja Wingtip zabawki

Poniższe zrzuty ekranu pochodzą z aplikacji ASP.NET Web Forms utworzonej w tej serii samouczków. Po uruchomieniu aplikacji w programie Visual Studio zostanie wyświetlona następująca Strona główna sieci Web.

![Zabawki Wingtip — strona domyślna](introduction-and-overview/_static/image1.png)

Możesz zarejestrować się jako nowy użytkownik lub zalogować się jako istniejący użytkownik. Górna Nawigacja zawiera linki do kategorii produktów i ich produktów z bazy danych.

W przypadku wybrania opcji **produkty**zostaną wyświetlone wszystkie dostępne produkty. 

![Zabawki Wingtip — produkty](introduction-and-overview/_static/image2.png)

W przypadku wybrania określonego produktu zostaną wyświetlone szczegóły produktu.

![Zabawki Wingtip — szczegóły produktu](introduction-and-overview/_static/image3.png)

Jako użytkownik możesz zarejestrować i zalogować się przy użyciu domyślnych funkcji szablonu formularzy sieci Web. W tym samouczku wyjaśniono również, jak zalogować się przy użyciu istniejącego konta usługi Gmail. Ponadto możesz zalogować się jako administrator, aby dodawać i usuwać produkty z bazy danych.

![Zabawki Wingtip — Zaloguj się](introduction-and-overview/_static/image4.png)

Po zalogowaniu się jako użytkownik możesz dodać produkty do koszyka zakupów i wyewidencjonować je w systemie PayPal. Przykładowa aplikacja została zaprojektowana tak, aby działała w piaskownicy deweloperów w systemie PayPal. Nie ma rzeczywistej transakcji pieniężnej.

![Zabawki Wingtip — koszyk](introduction-and-overview/_static/image5.png)

System PayPal potwierdza Twoje konto, zamówienie i informacje o płatności.

![Zabawki Wingtip — system PayPal](introduction-and-overview/_static/image6.png)

Po powrocie z systemu PayPal możesz przejrzeć i zakończyć zamówienie.

![Zabawki Wingtip — przegląd kolejności](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że na komputerze zainstalowano następujące oprogramowanie:

- [Microsoft Visual Studio 2017 lub Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).

.NET Framework jest instalowana automatycznie.

Ta seria samouczków używa Microsoft Visual Studio Community 2017. Aby ukończyć tę serię samouczków, możesz użyć opcji lub Microsoft Visual Studio 2017.

Zwróć uwagę na następujące informacje o programie Visual Studio:

* W tej serii samouczków Microsoft Visual Studio 2017 i Microsoft Visual Studio Community 2017 są określane jako *Visual Studio* .

* Program Visual Studio 2017 jest instalowany obok wszystkich starszych wersji, które są już zainstalowane. Lokacje utworzone we wcześniejszych wersjach mogą być otwierane w programie Visual Studio 2017 i nadal otwierane w poprzednich wersjach.

* Przy pierwszym uruchomieniu programu Visual Studio przyjęto założenie, że wybrano ustawienia *deweloperskie sieci Web* . Aby uzyskać więcej informacji, zobacz [How to: SELECT Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).

Po zainstalowaniu wymagań wstępnych możesz zacząć tworzyć projekt sieci Web przedstawiony w tej serii samouczków.

## <a name="download-the-sample-application"></a>Pobieranie przykładowej aplikacji

 Ukończoną przykładową aplikację można pobrać w dowolnym momencie z witryny przykładów MSDN:

[Wprowadzenie z formularzami sieci Web ASP.NET 4,5 i zabawkami Visual Studio 2013-Wingtip](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

 Ten pobieranie zawiera następujące elementy:

- Przykładowa aplikacja w folderze *WingtipToys* .
- Zasoby używane do tworzenia przykładowej aplikacji w folderze *WingtipToys-Assets* w folderze *WingtipToys* .

Pobranie to plik *. zip* . Aby wyświetlić ukończony Projekt tworzony przez tę serię samouczków, Znajdź i wybierz *C#* folder w pliku zip. Zapisz C# folder w folderze używanym do pracy z projektami programu Visual Studio. Domyślnie folder projektów programu Visual Studio 2017 to:

<strong>C:\Users&#92;</strong>  <strong><em>&lt;username&gt;</em></strong> <strong>\source\repos</strong>

Zmień nazwę ***C#*** folderu na ***WingtipToys***.

> [!NOTE]
> Jeśli masz już folder o nazwie *WingtipToys* w folderze projekty, tymczasowo Zmień nazwę istniejącego folderu przed zmianą nazwy *C#* folderu na *WingtipToys*.

Aby uruchomić ukończony projekt, Otwórz folder *WingtipToys* i kliknij dwukrotnie plik *WingtipToys. sln* . Program Visual Studio 2017 otwiera projekt. Następnie kliknij prawym przyciskiem myszy plik *default. aspx* w **Eksplorator rozwiązań** i wybierz pozycję **Widok w przeglądarce**.

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a>Skorzystaj z quizu formularzy sieci Web ASP.NET, aby przejrzeć zawartość

Po zakończeniu serii samouczków zapoznaj się z quizem, aby przetestować swoją wiedzę i wzmocnić kluczowe pojęcia. Każde pytanie zawiera wyjaśnienie i linki do dodatkowych wskazówek.

* [Quiz formularzy sieci Web ASP.NET](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a>Obsługa samouczków i komentarzy

Pytania i Komentarze można znaleźć w sekcji pytań i odpowiedzi, które znajdują się na [wprowadzenie za pomocą formularzy sieci Web ASP.NET 4,5 i Visual Studio 2013-Wingtip zabawki](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#).

Komentarze do tej serii samouczków są powitane. Gdy ta seria samouczków zostanie zaktualizowana, należy wziąć pod uwagę poprawki lub sugestie dotyczące ulepszeń.

Jeśli wystąpi błąd, odpowiednie komunikaty o błędach mogą być mylące i nie są dobrym wyjaśnieniem, jak to naprawić. Aby uzyskać pomoc, możesz sprawdzić [fora ASP.NET](https://forums.asp.net/). Innym dobrym źródłem jest sekcja pytań i odpowiedzi w [wprowadzenie za pomocą formularzy sieci Web ASP.NET 4,5 i Visual Studio 2013-Wingtip zabawki](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#). 

> [!div class="step-by-step"]
> [Next](create-the-project.md)

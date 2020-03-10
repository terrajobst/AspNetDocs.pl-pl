---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Tworzenie nowego projektu ASP.NET MVC | Microsoft Docs
author: microsoft
description: Krok 1 pokazuje, jak umieścić podstawową strukturę aplikacji NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 189ddc187fc83db14106b2da199ba12a70a32b45
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580935"
---
# <a name="create-a-new-aspnet-mvc-project"></a>Tworzenie nowego projektu ASP.NET MVC

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 1 bezpłatnego [samouczka dotyczącego aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , który zawiera instrukcje tworzenia niewielkiej, ale kompletnej aplikacji sieci Web przy użyciu ASP.NET MVC 1.
> 
> Krok 1 pokazuje, jak umieścić podstawową strukturę aplikacji NerdDinner.
> 
> Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner krok 1: plik&gt;nowy projekt

Zaczniemy korzystać z naszej aplikacji NerdDinner, wybierając pozycję **plik-&gt;nowy projekt** w programie visual Studio 2008 lub bezpłatnie Visual Web Developer 2008 Express.

Spowoduje to wyświetlenie okna dialogowego "nowy projekt". Aby utworzyć nową aplikację ASP.NET MVC, wybierz węzeł "Web" po lewej stronie okna dialogowego, a następnie wybierz szablon projektu "aplikacja sieci Web ASP.NET MVC" z prawej strony:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Ważne: Upewnij się, że pobrano i zainstalowano ASP.NET MVC — w przeciwnym razie nie będzie wyświetlany w oknie dialogowym Nowy projekt. Możesz użyć v2 [Instalator platformy Microsoft Web](https://www.microsoft.com/web/downloads/platform.aspx) , jeśli jeszcze go nie zainstalowano (ASP.NET MVC jest dostępny w sekcji "platformy sieci Web —&gt;struktury i środowiska uruchomieniowe").*

Zmienimy nazwę nowego projektu, aby utworzyć "NerdDinner", a następnie kliknij przycisk "OK", aby go utworzyć.

Po kliknięciu przycisku "OK" program Visual Studio wyświetli dodatkowe okno dialogowe z prośbą o opcjonalne utworzenie projektu testów jednostkowych dla nowej aplikacji. Ten projekt testu jednostkowego umożliwia nam Tworzenie zautomatyzowanych testów, które weryfikują funkcjonalność i zachowanie naszej aplikacji (co omówiono w dalszej części tego samouczka).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

Lista rozwijana "środowisko testowe" w powyższym oknie dialogowym zawiera wszystkie dostępne szablony projektów testów jednostkowych ASP.NET MVC zainstalowanych na komputerze. Wersje można pobrać dla NUnit, MBUnit i XUnit. Obsługiwana jest również wbudowana struktura testów jednostkowych programu Visual Studio.

*Uwaga: Struktura testów jednostkowych programu Visual Studio jest dostępna tylko w programie Visual Studio 2008 Professional i nowszych wersjach. Jeśli używasz programu VS 2008 Standard Edition lub Visual Web Developer 2008 Express, konieczne będzie pobranie i zainstalowanie rozszerzeń NUnit, MBUnit lub XUnit dla ASP.NET MVC, aby można było wyświetlić to okno dialogowe. Okno dialogowe nie zostanie wyświetlone, jeśli nie są zainstalowane żadne platformy testowe.*

Użyjemy domyślnej nazwy "NerdDinner. Tests" dla tworzonego projektu testowego i użyj opcji "Visual Studio Unit Test". Po kliknięciu przycisku "OK" program Visual Studio utworzy rozwiązanie dla nas z dwoma projektami w nim — jeden dla naszej aplikacji sieci Web i jeden dla naszych testów jednostkowych:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Badanie struktury katalogów NerdDinner

Podczas tworzenia nowej aplikacji ASP.NET MVC przy użyciu programu Visual Studio program automatycznie dodaje do projektu kilka plików i katalogów:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

Projekty ASP.NET MVC domyślnie mają sześć katalogów najwyższego poziomu:

| **Katalogi** | **Przeznaczenie** |
| --- | --- |
| **/Controllers** | Miejsce umieszczenia klas kontrolera, które obsługują żądania adresów URL |
| **/Models** | Miejsce umieszczenia klas, które reprezentują dane i manipulowanie nimi |
| **/Views** | Miejsce umieszczenia plików szablonów interfejsu użytkownika, które są odpowiedzialne za renderowanie danych wyjściowych |
| **/Scripts** | Miejsce umieszczenia plików biblioteki JavaScript i skryptów (. js) |
| **/Content** | Miejsce umieszczenia plików CSS i obrazów oraz innych zawartości niedynamicznej/niezawierającej języka JavaScript |
| **/App\_dane** | Miejsce przechowywania plików danych, które mają być odczytywane i zapisywane. |

ASP.NET MVC nie wymaga tej struktury. W rzeczywistości deweloperzy pracujący nad dużymi aplikacjami zwykle dzielą aplikację na wiele projektów w celu łatwiejszego zarządzania nimi (na przykład: klasy modelu danych często przechodzą do oddzielnego projektu biblioteki klas z aplikacji sieci Web). Domyślną strukturą projektu jest jednak zapewnienie domyślnej Konwencji katalogów, która może być używana do przechowywania aplikacji.

Po rozszerzeniu katalogu/controllers okaże się, że program Visual Studio dodał dwie klasy kontrolera — HomeController i elementu AccountController — domyślnie do projektu:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Po rozszerzeniu katalogu/views znajdziesz trzy podkatalogi —/Home,/Account i/Shared — a także kilka plików szablonów w ramach nich również zostały dodane do projektu domyślnie:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Po rozszerzeniu katalogów/Content i/scripts zostanie znaleziony plik site. CSS, który służy do nadawania stylu całej HTML w witrynie, a także bibliotek języka JavaScript, które umożliwiają obsługę ASP.NET technologii AJAX i jQuery w aplikacji:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Po rozszerzeniu projektu NerdDinner. Tests znajdziesz dwie klasy, które zawierają testy jednostkowe dla naszych klas kontrolera:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Te domyślne pliki dodane przez program Visual Studio udostępniają nam podstawową strukturę dla działającej aplikacji — kompletnej ze stroną główną, informacje na temat strony, logowania/wylogowywania/stron rejestracji oraz nieobsłużoną stronę błędu (wszystkie aplikacje przewodowe i wychodzące z usługi Box).

### <a name="running-the-nerddinner-application"></a>Uruchamianie aplikacji NerdDinner

Można uruchomić projekt, wybierając pozycję **Debuguj-&gt;Rozpocznij debugowanie** lub **&gt;Rozpocznij debugowanie bez debugowania** :

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Spowoduje to uruchomienie wbudowanego serwera sieci Web ASP.NET, który jest dostarczany z programem Visual Studio, i uruchomienie naszej aplikacji:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Poniżej znajduje się Strona główna nowego projektu (URL: "/") po uruchomieniu:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Kliknięcie karty "informacje" spowoduje wyświetlenie strony o stronie informacje (URL: "/Home/About"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Kliknięcie linku "Zaloguj się" w prawym górnym rogu spowoduje przejście do strony logowania (URL: "/Account/LogOn")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Jeśli nie masz konta logowania, możemy kliknąć link Register (URL: "/Account/Register"), aby go utworzyć:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Kod umożliwiający zaimplementowanie funkcji powyższego poziomu w domu, o i wylogowania/rejestrowania został domyślnie dodany podczas tworzenia nowego projektu. Będziemy używać go jako punktu początkowego naszej aplikacji.

### <a name="testing-the-nerddinner-application"></a>Testowanie aplikacji NerdDinner

Jeśli korzystamy z wersji Professional lub nowszej programu Visual Studio 2008, możemy użyć wbudowanej obsługi IDE testów jednostkowych w programie Visual Studio w celu przetestowania projektu:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Wybranie jednej z powyższych opcji spowoduje otwarcie okienka "Wyniki testów" w środowisku IDE i przekazanie nam stanu powodzenia/niepowodzenia na 27 testach jednostkowych zawartych w naszym nowym projekcie, które obejmują wbudowaną funkcję:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

W dalszej części tego samouczka dowiesz się więcej o automatycznym testowaniu i dodawaniu dodatkowych testów jednostkowych obejmujących wdrożoną funkcję aplikacji.

### <a name="next-step"></a>Następny krok

Teraz mamy podstawową strukturę aplikacji. [Utwórzmy teraz bazę danych do przechowywania danych aplikacji](create-a-database.md).

> [!div class="step-by-step"]
> [Poprzednie](introducing-the-nerddinner-tutorial.md)
> [dalej](create-a-database.md)

---
uid: web-forms/what-is-web-forms
title: Co to są formularze sieci Web | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: 19be419c499759713971a6c77674c924867d1bbc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636578"
---
# <a name="what-is-web-forms"></a>Co to są formularze sieci Web

ASP.NET Web Forms jest częścią struktury aplikacji sieci Web ASP.NET i jest dołączona do [programu Visual Studio](https://www.asp.net/downloads). Jest to jeden z czterech modeli programowania, których można użyć do tworzenia aplikacji sieci Web ASP.NET. inne są ASP.NET MVC, ASP.NET strony sieci Web i ASP.NET aplikacji jednostronicowych.

Formularze sieci Web to strony, do których użytkownicy żądają przy użyciu przeglądarki. Te strony można napisać przy użyciu kombinacji kodu HTML, klienta-skryptu, kontrolek serwera i serwera. Gdy użytkownicy żądają strony, są kompilowane i wykonywane na serwerze przez platformę, a następnie struktura generuje znacznik HTML, który może być renderowany przez przeglądarkę. Strona formularzy sieci Web ASP.NET przedstawia informacje dla użytkownika w dowolnej przeglądarce lub na urządzeniu klienckim.

Za pomocą programu Visual Studio można tworzyć formularze sieci Web ASP.NET. Zintegrowane środowisko programistyczne (IDE) programu Visual Studio umożliwia przeciąganie i upuszczanie formantów serwera w celu układania strony formularzy sieci Web. Następnie można łatwo ustawić właściwości, metody i zdarzenia dla kontrolek na stronie lub dla samej strony. Te właściwości, metody i zdarzenia są używane do definiowania zachowania strony sieci Web, wyglądu i działania oraz tak dalej. Aby napisać kod serwera do obsługi logiki strony, można użyć języka platformy .NET, takiego jak Visual Basic lub C#.

> [!NOTE] 
> 
> Dokumentacja ASP.NET i programu Visual Studio obejmuje kilka wersji. Tematy, które wyróżnią funkcje z poprzednich wersji, mogą być przydatne w przypadku bieżących zadań i scenariuszy przy użyciu najnowszych wersji.

**ASP.NET Web Forms:** 

- W oparciu o technologię Microsoft ASP.NET, w której kod uruchomiony na serwerze dynamicznie generuje dane wyjściowe stron sieci Web do przeglądarki lub urządzenia klienckiego.
- Zgodne z dowolnymi przeglądarkami lub urządzeniami przenośnymi. Strona sieci Web ASP.NET automatycznie renderuje prawidłowy kod HTML zgodny z przeglądarką dla funkcji, takich jak style, układ i tak dalej.
- Zgodne z dowolnym językiem obsługiwanym przez środowisko uruchomieniowe języka wspólnego .NET, takich jak Microsoft Visual Basic C#i Microsoft Visual.
- Utworzone na platformie Microsoft .NET. Zapewnia to wszystkie zalety platformy, w tym środowisko zarządzane, bezpieczeństwo typów i dziedziczenie.
- Elastyczność, ponieważ można dodać do nich kontrolki utworzone przez użytkownika i innych firm.

**Oferta formularzy sieci Web ASP.NET:** 

- Rozdzielenie kodu HTML i innego interfejsu użytkownika z logiki aplikacji.
- Bogaty zestaw kontrolek serwera dla typowych zadań, w tym dostęp do danych.
- Zaawansowane tworzenie powiązań danych dzięki doskonałej obsłudze narzędzi.
- Obsługa skryptów po stronie klienta wykonywanych w przeglądarce.
- Obsługa różnych możliwości, w tym routingu, zabezpieczeń, wydajności, funkcji wielojęzycznych, testowania, debugowania, obsługi błędów i zarządzania stanem.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>ASP.NET Web Forms pomaga w pokonaniu wyzwań

Programowanie aplikacji sieci Web przedstawia wyzwania, które zazwyczaj nie powstają podczas programowania tradycyjnych aplikacji opartych na kliencie. Wśród wyzwań są:

- **Implementacja zaawansowanego interfejsu użytkownika sieci Web** — może być trudne i żmudnym do projektowania i implementowania interfejsu użytkownika przy użyciu podstawowych obiektów HTML, zwłaszcza jeśli strona ma złożony układ, dużą ilość zawartości dynamicznej i w pełni funkcjonalne obiekty interaktywne.
- **Rozdzielenie klienta i serwera** — w aplikacji sieci Web, klienta (przeglądarki) i serwera są różne programy, które często działają na różnych komputerach (i nawet w różnych systemach operacyjnych). W związku z tym dwie połowy aplikacji mają bardzo mało informacji; mogą komunikować się, ale zazwyczaj wymienia tylko małe fragmenty prostych informacji.
- **Bezstanowe wykonywanie** — gdy serwer sieci Web odbiera żądanie dotyczące strony, odnajdzie stronę, przetwarza ją, wysyła do przeglądarki, a następnie odrzuca wszystkie informacje o stronie. Jeśli użytkownik zażąda tej samej strony ponownie, serwer powtarza całą sekwencję, przetwarzanie strony od podstaw. W innym przypadku serwer nie ma pamięci stron, które zostały przetworzone — strona nie jest bezstanowa. W związku z tym, jeśli aplikacja musi obsługiwać informacje o stronie, jej stan bez stanu może stać się problemem.
- **Nieznane możliwości klienta** — w wielu przypadkach aplikacje sieci Web są dostępne dla wielu użytkowników korzystających z różnych przeglądarek. Przeglądarki mają różne możliwości, dzięki czemu trudno jest utworzyć aplikację, która będzie działała równie dobrze na wszystkich z nich.
- **Komplikacje związane z dostępem do danych** — odczytywanie i zapisywanie do źródła danych w tradycyjnych aplikacjach sieci Web może być skomplikowane i intensywnie obciążające zasoby.
- **Komplikacje z skalowalnością** — w wielu przypadkach aplikacje sieci Web, które mają istniejące metody, nie spełniają wymagań skalowalności ze względu na brak zgodności między różnymi składnikami aplikacji. Jest to często typowy punkt awarii dla aplikacji w ramach dużego cyklu wzrostu.

Spełnienie tych wyzwań dla aplikacji sieci Web może wymagać znacznego czasu i wysiłku. ASP.NET Web Forms i ASP.NET Framework dotyczą tych wyzwań w następujący sposób:

- **Intuicyjny, spójny model obiektów** — struktura strony ASP.NET przedstawia model obiektów, który pozwala na myśleć o formularzach jako jednostki, nie jako osobne elementy klienta i serwera. W tym modelu można programować stronę w bardziej intuicyjny sposób niż w tradycyjnych aplikacjach sieci Web, w tym możliwość ustawiania właściwości elementów strony i reagowania na zdarzenia. Ponadto formanty serwera ASP.NET są abstrakcją z fizycznej zawartości strony HTML oraz od bezpośredniej interakcji między przeglądarką a serwerem. Ogólnie rzecz biorąc, można użyć kontrolek serwerowych w sposób umożliwiający współdziałanie z kontrolkami w aplikacji klienckiej i nie trzeba myśleć o sposobie tworzenia kodu HTML, aby przedstawić i przetworzyć kontrolki i ich zawartość.
- **Model programowania oparty na zdarzeniach** — ASP.NET Web Forms dołączenie do aplikacji sieci Web znany model obsługi zdarzeń pisania dla zdarzeń występujących na kliencie lub serwerze. Struktura stron ASP.NETa jest abstrakcyjna dla tego modelu w taki sposób, że podstawowy mechanizm przechwytywania zdarzenia na kliencie, transmitowania go do serwera i wywoływania odpowiedniej metody jest wszystkie automatyczne i niewidoczne. Wynikiem jest jasno, łatwą do zapisania strukturę kodu, która obsługuje programowanie oparte na zdarzeniach.
- **Intuicyjne zarządzanie Stanami** — struktura strony ASP.NET automatycznie obsługuje zadanie utrzymania stanu strony i jego kontrolek, a także zapewnia jawne sposoby utrzymania stanu informacji specyficznych dla aplikacji. Jest to realizowane bez dużego użycia zasobów serwera i można je zaimplementować z lub bez wysyłania plików cookie do przeglądarki.
- **Aplikacje niezależne od przeglądarki** — struktura strony ASP.NET umożliwia tworzenie wszystkich logiki aplikacji na serwerze, eliminując konieczność jawnego kodu dla różnic w przeglądarkach. Jednak nadal można korzystać z funkcji specyficznych dla przeglądarki, pisząc kod po stronie klienta w celu zapewnienia lepszej wydajności i bogatszego środowiska klienta.
- **.NET Framework obsługa środowiska uruchomieniowego** CLR — struktura strony ASP.NET jest oparta na .NET Framework, dzięki czemu cała platforma jest dostępna dla dowolnej aplikacji ASP.NET. Aplikacje można napisać w dowolnym języku, który jest zgodny z środowiskiem uruchomieniowym. Ponadto dostęp do danych jest uproszczony przy użyciu infrastruktury dostępu do danych dostarczonej przez .NET Framework, w tym ADO.NET.
- **.NET Framework skalowalnej wydajności serwera** — struktura stron ASP.NET umożliwia skalowanie aplikacji sieci Web z jednego komputera z pojedynczym procesorem do kolektywu serwerów sieci Web z obsługą wielu komputerów i bez skomplikowanych zmian w logice aplikacji.

## <a name="features-of-aspnet-web-forms"></a>Funkcje formularzy sieci Web ASP.NET

- **Formanty serwera**— formanty serwera sieci Web ASP.NET są obiektami na stronach sieci Web ASP.NET, które są uruchamiane po zażądaniu strony i są renderowane do przeglądarki. Wiele kontrolek serwera sieci Web jest podobnych do znanych elementów HTML, takich jak przyciski i pola tekstowe. Inne kontrolki obejmują złożone zachowanie, takie jak kontrolki kalendarza, oraz kontrolki, których można użyć do nawiązywania połączeń ze źródłami danych i wyświetlania danych.
- **Strony wzorcowe**— ASP.NET strony główne umożliwiają tworzenie spójnego układu stron w aplikacji. Pojedyncza strona wzorcowa definiuje wygląd i działanie oraz standardowe zachowanie dla wszystkich stron (lub grupy stron) w aplikacji. Następnie można utworzyć poszczególne strony zawartości zawierające zawartość, która ma zostać wyświetlona. Gdy użytkownicy zażądają stron zawartości, scalają się ze stroną wzorcową w celu utworzenia danych wyjściowych, które łączą układ strony wzorcowej z zawartością ze strony zawartość.
- **Praca z danymi**— ASP.NET udostępnia wiele opcji przechowywania, pobierania i wyświetlania danych. W aplikacji ASP.NET Web Forms używasz formantów powiązanych z danymi, aby zautomatyzować prezentację lub dane wejściowe danych w elementach interfejsu użytkownika strony sieci Web, takich jak tabele i pola tekstowe i listy rozwijane.
- **Członkostwo**— ASP.NET Identity przechowuje poświadczenia użytkowników w bazie danych utworzonej przez aplikację. Gdy użytkownicy logują się, aplikacja sprawdza poprawność swoich poświadczeń, odczytując bazę danych. Folder *konta* projektu zawiera pliki implementujące różne części członkostwa: rejestrowanie, logowanie, zmiana hasła i Autoryzowanie dostępu. Ponadto ASP.NET Web Forms obsługuje uwierzytelnianie OAuth i OpenID Connect. Te ulepszenia uwierzytelniania umożliwiają użytkownikom logowanie się do witryny przy użyciu istniejących poświadczeń, takich jak Facebook, Twitter, Windows Live i Google. Domyślnie szablon tworzy bazę danych członkostwa przy użyciu domyślnej nazwy bazy danych w wystąpieniu SQL Server Express LocalDB, serwer bazy danych programistycznej, który jest dostarczany z Visual Studio Express 2013 dla sieci Web.
- **Struktury skryptów i klientów klienta**— można rozszerzyć funkcje programu ASP.NET oparte na serwerze, dołączając funkcje skryptu klienta na stronach ASP.NET formularzy sieci Web. Możesz użyć skryptu klienta, aby zapewnić bogatszy, bardziej reagujący interfejs użytkownika dla użytkowników. Za pomocą skryptu klienta można również wykonywać wywołania asynchroniczne do serwera sieci Web, gdy strona jest uruchomiona w przeglądarce.
- **Routing-adresy**URL umożliwiają skonfigurowanie aplikacji do akceptowania adresów URL żądań, które nie są mapowane na pliki fizyczne. Adres URL żądania jest po prostu adresem URL, który użytkownik wprowadza do przeglądarki, aby znaleźć stronę w witrynie sieci Web. Funkcja routingu służy do definiowania adresów URL, które są semantycznie zrozumiałe dla użytkowników i które mogą pomóc w optymalizacji aparatu wyszukiwania (wyszukiwarka).
- **Zarządzanie stanem**— formularze sieci Web ASP.NET zawierają kilka opcji, które pomagają zachować dane zarówno dla poszczególnych stron, jak i całej aplikacji.
- **Zabezpieczenia**— ważnym elementem tworzenia bezpieczniejszej aplikacji jest zrozumienie dla nich zagrożeń. Firma Microsoft opracowała sposób kategoryzowania zagrożeń: fałszowanie, manipulowanie, wyparcie, ujawnienie informacji, odmowa usługi i podniesienie uprawnień (krok). W formularzach sieci Web ASP.NET można dodać punkty rozszerzalności i opcje konfiguracji, które umożliwiają dostosowanie różnych zachowań zabezpieczeń w formularzach sieci Web ASP.NET.
- **Wydajność**— wydajność może być kluczowym czynnikiem w przypadku pomyślnej witryny lub projektu sieci Web. ASP.NET Web Forms umożliwia modyfikowanie wydajności związanej z przetwarzaniem stron i formantami serwera, zarządzaniem stanem, dostępem do danych, konfiguracją i ładowaniem aplikacji oraz wydajnymi praktykami kodowania.
- Funkcja ASP.NET Web Forms umożliwia tworzenie stron sieci Web, które mogą uzyskać zawartość i inne dane na podstawie preferowanych ustawień języka dla przeglądarki lub w oparciu o jawny wybór języka przez użytkownika. Zawartość i inne dane są określane jako zasoby, a takie dane mogą być przechowywane w plikach zasobów lub w innych źródłach. Na stronie formularzy sieci Web ASP.NET można skonfigurować kontrolki do pobierania ich wartości właściwości z zasobów. W czasie wykonywania wyrażenia zasobów są zastępowane przez zasoby z odpowiedniego zlokalizowanego pliku zasobów.
- **Debugowanie i obsługa błędów**— ASP.NET zawiera funkcje, które ułatwiają diagnozowanie problemów, które mogą wystąpić w aplikacji formularzy sieci Web. Debugowanie i obsługa błędów są również obsługiwane w ramach formularzy sieci Web ASP.NET, dzięki czemu aplikacje są kompilowane i działać efektywnie.
- **Wdrażanie i hosting**— Visual Studio, ASP.NET, Azure i IIS udostępniają narzędzia ułatwiające proces wdrażania i obsługiwania aplikacji formularzy sieci Web.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Podejmowanie decyzji o tym, kiedy należy utworzyć aplikację formularzy sieci Web

Należy uważnie rozważyć, czy zaimplementować aplikację sieci Web za pomocą modelu ASP.NET Web Forms lub innego modelu, takiego jak ASP.NET MVC Framework. Struktura MVC nie zastępuje modelu formularzy internetowych; w aplikacjach internetowych można korzystać z obu struktur. Przed podjęciem decyzji o użyciu modelu formularzy sieci Web lub platformy MVC dla określonej witryny sieci Web, należy zważyć zalety poszczególnych rozwiązań.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Zalety aplikacji internetowych opartych na formularzach internetowych

Struktura oparta na formularzach sieci Web ma następujące zalety:

- Obsługuje model zdarzeń zachowujący stan przez HTTP, co przydaje się przy projektowaniu branżowych aplikacji internetowych. Aplikacja oparta na formularzach sieci Web udostępnia dziesiątki zdarzeń, które są obsługiwane przez setki kontrolek serwerowych.
- Korzysta z wzorca kontrolera strony, który dodaje funkcje do pojedynczych stron. Aby uzyskać więcej informacji, zobacz [kontroler strony](https://go.microsoft.com/fwlink/?LinkId=106359 "Kontroler strony") w witrynie MSDN w sieci Web.
- Korzysta ona z widoku stanu lub formularzy opartych na serwerze, które ułatwiają zarządzanie informacjami o stanie.
- Sprawdza się w przypadku niewielkich zespołów projektantów witryn sieci Web oraz w przypadku projektantów, którzy chcą czerpać korzyści z dużej liczby składników dostępnych do szybkiego opracowywania aplikacji.
- Ogólnie rzecz biorąc, jest mniej skomplikowane do tworzenia aplikacji, ponieważ składniki (Klasa **strony** , kontrolki itd.) są ściśle zintegrowane i zwykle wymagają mniej kodu niż model MVC.

### <a name="advantages-of-an-mvc-based-web-application"></a>Zalety aplikacji internetowych opartych na strukturze MVC

Struktura ASP.NET MVC ma następujące zalety:

- Ułatwia zarządzanie złożonością, dzieląc aplikację na model, widok i kontroler.
- Nie używa stanu widoku ani formularzy opartych na serwerach. To sprawia, że struktura MVC jest idealna dla deweloperów, którzy chcą w pełni kontrolować działanie aplikacji.
- Używa wzorca kontrolera frontowego, który przetwarza żądania aplikacji internetowej za pośrednictwem jednego kontrolera. To pozwala projektować aplikacje obsługujące złożoną infrastrukturę routingu. Aby uzyskać więcej informacji, zobacz [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Kontroler frontonu") w witrynie MSDN w sieci Web.
- Oferuje lepszą obsługę projektowania opartego na testach (test-driven development, TDD).
- Dobrze sprawdza się w przypadku aplikacji sieci Web, które są obsługiwane przez duże zespoły deweloperów i projektantów sieci Web, którzy potrzebują wysokiego poziomu kontroli nad zachowaniem aplikacji.

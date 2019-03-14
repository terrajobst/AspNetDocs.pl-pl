---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: Omówienie uwierzytelniania formularzy (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku będziemy zmieni się z zaledwie dyskusji do wdrożenia; w szczególności omówimy Wdrażanie uwierzytelniania formularzy. W aplikacji sieci web...
ms.author: riande
ms.date: 01/14/2008
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: ca290c6b7b6b4f8da92b9658519d716651d46341
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57072842"
---
<a name="an-overview-of-forms-authentication-vb"></a>Omówienie uwierzytelniania formularzy (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> W tym samouczku będziemy zmieni się z zaledwie dyskusji do wdrożenia; w szczególności omówimy Wdrażanie uwierzytelniania formularzy. Zaczniemy tworzenie w ramach tego samouczka aplikacji sieci web będzie być wbudowane w system w kolejnych samouczkach podczas przenoszenia z uwierzytelniania formularzy proste do członkostwa i ról.
> 
> Zobacz ten film, aby uzyskać więcej informacji na ten temat: [Za pomocą uwierzytelniania podstawowego formularzy na platformie ASP.NET](# "using-basic-forms-authentication-in-aspnet").


## <a name="introduction"></a>Wprowadzenie

W [poprzedni Samouczek](security-basics-and-asp-net-support-vb.md) Omówiliśmy różne uwierzytelniania, autoryzacji i użytkownika konta opcje dostarczanego przez platformę ASP.NET. W tym samouczku będziemy zmieni się z zaledwie dyskusji do wdrożenia; w szczególności omówimy Wdrażanie uwierzytelniania formularzy. Zaczniemy tworzenie w ramach tego samouczka aplikacji sieci web będzie być wbudowane w system w kolejnych samouczkach podczas przenoszenia z uwierzytelniania formularzy proste do członkostwa i ról.

Ten samouczek rozpoczyna się od przyjrzeć się przepływu pracy uwierzytelniania formularzy, temat, który firma Microsoft korzysta z chwilą w poprzednim samouczku. Poniżej utworzymy w witrynie sieci Web ASP.NET za pomocą którego na targach pojęć dotyczących uwierzytelniania formularzy. Następnie zostaną skonfigurowane lokacji korzystanie z uwierzytelniania formularzy, Utwórz stronę logowania proste i zobacz, jak określić w kodzie, czy użytkownik jest uwierzytelniany, a jeśli tak, nazwy użytkownika, są rejestrowane przy użyciu.

Omówienie formularzy, przepływu pracy uwierzytelniania, włączając je w aplikacji sieci web i tworzenie stron logowania i wylogowywania się wszystkie istotne kroków w tworzeniu aplikacji ASP.NET, która obsługuje konta użytkowników i uwierzytelnia użytkowników za pomocą strony sieci web. W związku z tym — i ponieważ tych samouczków zależą od siebie nawzajem — czy zachęcam Cię do pracy za pomocą tego samouczka w całości przed przejściem do następnego, nawet jeśli masz już mieć środowisko konfigurowania uwierzytelniania formularzy w ciągu ostatnich projektów.

## <a name="understanding-the-forms-authentication-workflow"></a>Informacje o przepływie pracy uwierzytelniania formularzy

Gdy środowisko uruchomieniowe ASP.NET przetwarza żądanie dla zasobu platformy ASP.NET, takich jak strony ASP.NET lub usługi sieci Web platformy ASP.NET, żądanie podnosi liczbę zdarzeń podczas ich cyklu życia. Brak zdarzeń wywołanych z końcem bardzo zaczynające się i bardzo żądań, takich, które jest wywoływane, gdy żądanie jest uwierzytelniane i autoryzacji, zdarzenie zgłaszane w przypadku nieobsługiwanego wyjątku i tak dalej. Aby wyświetlić pełną listę zdarzeń, zobacz [zdarzenia obiektu aplikacji HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*Moduły HTTP* są klasami zarządzanymi, którego kod jest wykonywany w odpowiedzi na konkretne zdarzenie w cyklu życia żądania. ASP.NET jest dostarczany z liczbą moduły HTTP, który wykonywania podstawowych zadań w tle. Są dwa wbudowane moduły HTTP, które są szczególnie istotne w naszym dyskusji:

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)**  — uwierzytelnia użytkownika, sprawdzając biletu uwierzytelniania formularzy, które najczęściej przygotowywanych do uwzględnienia w kolekcji plików cookie przez użytkownika. Jeśli ma nie biletu uwierzytelniania formularzy, użytkownik jest anonimowy.
- **[UrlAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)**  -Określa, czy bieżący użytkownik jest autoryzowany do dostępu do żądanego adresu URL. Ten moduł określa uprawnienia przez zapoznanie się z reguł autoryzacji, określone w plikach konfiguracji aplikacji. Program ASP.NET zawiera również [FileAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) określający urząd, przeglądając pliki żądanej listy ACL.

FormsAuthenticationModule podejmuje próbę uwierzytelnienia użytkownika przed wykonaniem UrlAuthorizationModule (i FileAuthorizationModule). Jeżeli użytkownika zgłaszającego żądanie nie ma autoryzacji dostępu do żądanego zasobu, moduł autoryzacji kończy żądanie, a następnie zwraca [HTTP 401 nieautoryzowane](http://www.checkupdown.com/status/E401.html) stanu. W scenariuszach uwierzytelniania Windows stan HTTP 401, jest zwracana do przeglądarki. Ten kod stanu powoduje, że w przeglądarce, monit o podanie swoich poświadczeń za pośrednictwem modalne okno dialogowe. Za pomocą uwierzytelniania formularzy, stan HTTP 401 nieautoryzowane nigdy nie są wysyłane do przeglądarki, ponieważ FormsAuthenticationModule wykrywa ten stan i modyfikuje je, aby przekierować użytkownika do strony logowania, zamiast tego (za pośrednictwem [HTTP 302przekierowania.](http://www.checkupdown.com/status/E302.html) stanu).

Odpowiedzialność strony logowania jest ustalić, czy poświadczenia użytkownika są prawidłowe, i jeśli tak, utworzyć bilet uwierzytelniania formularzy i przekieruje użytkownika z powrotem do strony one próba odwiedź stronę. Bilet uwierzytelniania znajduje się w kolejnych żądań do stron w witrynie sieci Web, która FormsAuthenticationModule używa do identyfikacji użytkownika.


[![Przepływ pracy uwierzytelniania formularzy](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**Rysunek 01**: Przepływ pracy uwierzytelniania formularzy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image3.png))


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Uzupełnij biletu uwierzytelniania między odwiedzin strony

Po zalogowaniu się w biletu uwierzytelniania formularzy musi zostać odesłana do serwera sieci web na każde żądanie tak, aby użytkownik pozostaje zalogowany, ponieważ przeglądania witryny. Zazwyczaj jest to osiągane przez umieszczenie biletu uwierzytelniania w kolekcji plików cookie przez użytkownika. [Pliki cookie](http://en.wikipedia.org/wiki/HTTP_cookie) to małe pliki tekstowe, które znajdują się na komputerze użytkownika i są przekazywane w nagłówkach HTTP na każde żądanie do witryny sieci Web, która utworzyła plik cookie. W związku z tym po utworzeniu biletu uwierzytelniania formularzy i przechowywane w plikach cookie w przeglądarce każdej kolejnej wizyty do tej lokacji wysyła bilet uwierzytelniania wraz z żądaniem w ten sposób identyfikowania użytkowników.

> [!NOTE]
> Demonstracyjnej aplikacji internetowej używany w każdym samouczku, jest dostępna do pobrania. Ta aplikacja do pobrania został utworzony za pomocą programu Visual Web Developer 2008 przeznaczone dla .NET Framework w wersji 3.5. Ponieważ aplikacja jest przeznaczona dla .NET 3.5, jego plik Web.config zawiera elementy konfiguracji dodatkowych, specyficzne dla 3.5. Długi tekst krótki, jeśli masz jeszcze do zainstalowania programu .NET 3.5 na komputerze, następnie do pobrania aplikacji sieci web programu nie będzie działać bez usuwania znacznika specyficzne dla 3.5 z pliku Web.config.


Jednym z aspektów plików cookie jest jego wygaśnięcia, czyli Data i godzina, jaką odrzuca wszystkie pliki cookie przeglądarki. Po wygaśnięciu pliku cookie uwierzytelniania formularzy, użytkownik nie jest już może uwierzytelniony i w związku z tym stają się anonimowe. Gdy użytkownik odwiedził z poziomu terminalu publicznych, jest szansa, że chce ich biletu uwierzytelniania wygaśnie przy zamykaniu przeglądarki. Podczas przeglądania w domu, jednak ten sam użytkownik może być biletu uwierzytelniania, należy pamiętać w przeglądarce zostanie ponownie uruchomiony, tak, aby nie są dostępne do ponownego logowania się za każdym razem, mogą odwiedzić witrynę. Ta decyzja często jest wykonywane przez użytkownika w postaci checkbox Pamiętaj mnie na stronie logowania. W kroku 3 zostanie omówiony sposób implementacji wyboru Pamiętaj mnie na stronie logowania. Następującego samouczka adresów ustawienia limitu czasu biletu uwierzytelniania szczegółowo.

> [!NOTE]
> Istnieje możliwość, że agenta użytkownika używane do logowania się do witryny sieci Web mogą nie obsługiwać pliki cookie. W takim przypadku platformy ASP.NET można użyć biletów uwierzytelniania formularzy cookieless. W tym trybie biletu uwierzytelniania jest zakodowany w adresie URL. Przyjrzymy się gdy są używane bilety cookieless uwierzytelniania i jak są tworzone i zarządzane w następnym samouczku.


### <a name="the-scope-of-forms-authentication"></a>Zakres uwierzytelniania formularzy

Odbywa się FormsAuthenticationModule kodu oznacza to część środowiska uruchomieniowego programu ASP.NET. Przed firmy Microsoft w wersji 7 [Internet Information Services (IIS)](https://www.iis.net/) serwera sieci web wystąpił distinct barierę między potoku HTTP usług IIS i środowiska uruchomieniowego ASP.NET potoku. Krotko mówiąc, w usługach IIS 6 i starszych FormsAuthenticationModule wykonywana tylko wtedy, gdy żądanie jest delegowane za pomocą programu IIS do środowiska uruchomieniowego programu ASP.NET. Domyślnie usługi IIS przetwarzają zawartość statyczną sam — jak strony HTML i CSS i pliki obrazów — i jedynie zdejmowania rąk żądania do środowiska uruchomieniowego programu ASP.NET, czy żądaniu strony z rozszerzeniem .aspx, .asmx i ashx.

Usługi IIS 7, jednak umożliwia zintegrowanych usług IIS i platformy ASP.NET potoków. Przy użyciu kilku ustawień konfiguracji, można skonfigurować usługi IIS 7 do wywołania FormsAuthenticationModule dla *wszystkich* żądań. Ponadto za pomocą usług IIS 7 można zdefiniować reguły autoryzacji adresów URL dla plików dowolnego typu. Aby uzyskać więcej informacji, zobacz [zmiany między usług IIS 6 i zabezpieczenia usług IIS7](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [Your zabezpieczenia platformy sieci Web](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements), i [Autoryzacja adresów URL usług IIS7 opis](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Długi tekst krótki, w wersjach wcześniejszych niż IIS 7, tylko umożliwia uwierzytelnianie formularzy chronić zasoby obsługiwane przez środowisko uruchomieniowe programu ASP.NET. Podobnie reguły autoryzacji adresów URL są stosowane tylko do zasobów, obsługiwane przez środowisko uruchomieniowe programu ASP.NET. Ale za pomocą usług IIS 7 można go zintegrować FormsAuthenticationModule i UrlAuthorizationModule do potoku HTTP usług IIS, rozszerzając w ten sposób działania tej funkcji na wszystkich żądań.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Krok 1. Tworzenie witryny sieci Web platformy ASP.NET dla tej serii samouczków

Aby osiągnąć najszerszych możliwych odbiorców, witryny sieci Web platformy ASP.NET, będziemy budować w całej serii tego zostanie utworzone z firmy Microsoft w bezpłatnej wersji usługi Visual Studio 2008 [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Firma Microsoft będzie implementować magazyn użytkownika SqlMembershipProvider w [programu Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) bazy danych. Jeśli używasz programu Visual Studio 2005 lub innej wersji programu Visual Studio 2008 lub SQL Server, nie martw się — kroki będą niemal identyczne, i będzie wskazano różnice nietrywialnymi.

Zanim można skonfigurować uwierzytelnianie formularzy, najpierw należy w witrynie sieci Web platformy ASP.NET. Rozpocznij od utworzenia nowego pliku w oparciu o system ASP.NET witryny sieci Web. Aby to zrobić, uruchom Visual Web Developer, a następnie przejdź do menu Plik i wybierz nową witrynę sieci Web w celu wyświetlania okna dialogowego nową witrynę sieci Web. Wybierz szablon witryny sieci Web platformy ASP.NET, zmień wartość na liście rozwijanej lokalizacji w systemie plików, wybierz folder do umieszczenia w witrynie sieci web i ustaw języka VB. Spowoduje to utworzenie nowej witryny sieci web za pomocą strony Default.aspx ASP.NET, aplikację\_folderu danych i pliku Web.config.

> [!NOTE]
> Program Visual Studio obsługuje dwa tryby zarządzania projektem: Projektów witryny sieci Web i projektów aplikacji sieci Web. Projektów witryny sieci Web brakuje pliku projektu, a Web Application Projects naśladować architektury projektu w Visual Studio .NET 2002/2003 — Dołącz plik projektu i skompilować kod źródłowy projektu do jednego zestawu, który jest umieszczony w folderze/bin. Visual Studio 2005 projektów początkowo tylko obsługiwane witryny sieci Web, mimo że modelu projektu aplikacji sieci Web została przywrócona z dodatkiem Service Pack 1; Program Visual Studio 2008 oferuje oba modele projektu. Visual Web Developer 2005 i wersji 2008, ale obsługują tylko projektów witryny sieci Web. Czy mogę używać modelu projektu witryny sieci Web. Jeśli używasz wersji non-Express i chcesz użyć [modelu projektu aplikacji sieci Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) zamiast tego możesz to zrobić, ale należy pamiętać, że może istnieć pewne rozbieżności między wyświetlanych na ekranie oraz czynności należy wykonać w porównaniu z zrzuty ekranu pokazano, jak i instrukcje podane w tych samouczkach.


[![Tworzenie nowego pliku w oparciu o System witryny sieci Web](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**Rysunek 02**: Tworzenie witryny sieci Web New File System-Based ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image6.png))


### <a name="adding-a-master-page"></a>Dodawanie strony wzorcowej

Następnie dodaj nową stronę wzorcową do lokacji w katalogu głównym o nazwie Site.master. [Strony wzorcowe](https://msdn.microsoft.com/library/wtxbf3hh.aspx) włączyć projektanta strony zdefiniować szablon całej lokacji, które mogą być stosowane do strony ASP.NET. Główną zaletą stron wzorcowych to, że ogólnego wyglądu witryny można zdefiniować w obrębie jednej lokalizacji, co ułatwia aktualizacji lub dostosować układ strony.


[![Dodaj stronę wzorcową o nazwie Site.master do witryny sieci Web](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**Rysunek 03**: Dodaj Site.master o nazwie Master strony do witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image9.png))


Na stronie głównej, należy zdefiniować tutaj układu strony całej lokacji. Możesz użyć widoku projektu i dodawanie kontrolek niezależnie od układu lub sieci Web, należy, lub można ręcznie dodawać znaczniki ręcznie w widoku źródła. Strukturalnych I układ strony wzorcowej do naśladowania układ używane w mojej *[Praca z danymi w programie ASP.NET 2.0](../../data-access/index.md)* serii samouczków (zobacz rysunek 4). Strona główna używa [kaskadowych arkuszy stylów](http://www.w3schools.com/css/default.asp) pozycjonowanie i ustawieniami CSS zdefiniowanych w pliku Style.css (która znajduje się w tym samouczku pobieranie skojarzone). Gdy nie można odróżnić od znaczników, pokazano poniżej, reguły CSS są zdefiniowane tak, aby nawigacji &lt;div&gt;firmy zawartości są pozycjonowane absolutnie, który pojawia się po lewej stronie i ma stałą szerokość 200 pikseli.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

Strona wzorcowa definiuje układ stron statycznych i regiony, które mogą być edytowane przez strony ASP.NET, które używają strony wzorcowej. Te obszary można edytować zawartości są wskazywane przez kontrolki ContentPlaceHolder, które są widoczne w zawartości &lt;div&gt;. Nasze strony wzorcowej ma jednej ContentPlaceHolder (MainContent), ale strona wzorcowa może mieć wiele kontrolek ContentPlaceHolder.

Ze znacznikami podanymi powyżej przełączanie do widoku projektu zawiera układ strony wzorcowej. Wszystkie strony ASP.NET, które używają tej strony wzorcowej mają ten jednolity układ i możliwość określenia znaczników dla regionu MainContent.


[![Strona wzorcowa, podczas wyświetlania za pośrednictwem widoku projektu](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**Rysunek 04**: strona wzorcowa, podczas wyświetlania za pośrednictwem widoku projektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image12.png))


### <a name="creating-content-pages"></a>Tworzenie stron zawartości

W tym momencie mamy strony Default.aspx w naszej witryny sieci Web, ale nie używa stronę wzorcową, którą właśnie utworzyliśmy. Chociaż możliwe do manipulowania oznaczeniu deklaracyjnym strony sieci web do użycia na stronę wzorcową, jeśli strona nie zawiera żadnej zawartości jest jeszcze łatwiej jest po prostu usunąć stronę i dodaj go ponownie do projektu, określanie strony wzorcowej do użycia. W związku z tym start, usuwając Default.aspx z projektu.

Następnie kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierz dodać nowy formularz sieci Web o nazwie Default.aspx. Ten czas, zaznacz pole wyboru, wybierz stronę wzorcową i wybierz stronę wzorcową Site.master z listy.


[![Dodawanie nowej strony Default.aspx, wybierając pozycję Wybierz stronę wzorcową](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**Rysunek 05**: Dodaj nowe Default.aspx strona wybór opcji wybierz stronę wzorcową ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image15.png))


[![Użyj strony wzorcowej Site.master](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**Rysunek 06**: Użyj strony wzorcowej Site.master ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image18.png))


> [!NOTE]
> Jeśli używasz modelu projektu aplikacji sieci Web w oknie dialogowym Dodaj nowy element nie zawiera pola wyboru Wybierz stronę wzorcową. Zamiast tego należy dodać element typu formularz zawartości sieci Web. Po opcji formularz zawartości sieci Web, a następnie klikając przycisk Dodaj, Visual Studio wyświetli wybierz ten sam główny okno dialogowe pokazano na rysunku 6.


Nowa strona Default.aspx oznaczeniu deklaracyjnym obejmuje tylko @Page dyrektywy, określając ścieżkę do poziomu głównego dla MainContent ContentPlaceHolder strony wzorcowej strony plików i kontrolki zawartości.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

Na razie pozostaw puste Default.aspx. Firma Microsoft będzie zwrócić w dalszej części tego samouczka, aby dodać zawartość.

> [!NOTE]
> Strony głównej zawiera sekcja menu lub innych interfejs nawigacyjny. W przyszłości samouczku utworzymy taki interfejs.


## <a name="step-2-enabling-forms-authentication"></a>Krok 2. Włączanie uwierzytelniania formularzy

Z witryną internetową programu ASP.NET utworzona naszym kolejnym krokiem jest aby włączyć uwierzytelnianie formularzy. Konfiguracja uwierzytelniania aplikacji jest określony za pomocą [ &lt;uwierzytelniania&gt; elementu](https://msdn.microsoft.com/library/532aee0e.aspx) w pliku Web.config. &lt;Uwierzytelniania&gt; element zawiera jeden atrybut o nazwie tryb określający model uwierzytelniania używany przez aplikację. Ten atrybut może mieć jedną z następujących czterech wartości:

- **Windows** — zgodnie z opisem w poprzednim samouczku, gdy aplikacja używa uwierzytelniania Windows odpowiada serwera sieci web do uwierzytelniania obiektu odwiedzającego i zwykle jest to realizowane podstawowe, szyfrowane lub zintegrowane Windows uwierzytelnianie.
- **Formularze**— użytkownicy są uwierzytelniani za pomocą formularza na stronie sieci web.
- **Usługa Passport**— użytkownicy są uwierzytelniani za pomocą sieci Passport firmy Microsoft.
- **Brak**— bez modelu uwierzytelniania jest używany; wszystkie osoby odwiedzające są anonimowe.

Domyślnie aplikacje ASP.NET uwierzytelnianie Windows. Aby zmienić typ uwierzytelniania do uwierzytelniania formularzy, następnie należy zmodyfikować &lt;uwierzytelniania&gt; atrybut tryb elementu do formularzy.

Jeśli projekt nie zawiera jeszcze pliku Web.config, należy dodać jeden teraz przez kliknięcie prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wybierając Dodaj nowy element, a następnie dodanie pliku konfiguracji sieci Web.


[![Jeśli projekt nie zawiera jeszcze pliku Web.config, dodaj je teraz](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**Rysunek 07**: Jeśli Twój projekt jest nie jeszcze dołączyć plik Web.config, Dodaj teraz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image21.png))


Następnie znajdź &lt;uwierzytelniania&gt; elementu i aktualizacji, aby używał uwierzytelniania formularzy. Po tej zmianie kodu znaczników pliku Web.config powinien wyglądać podobnie do poniższej:

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> Ponieważ plik Web.config jest plikiem XML, ważne jest wielkość liter w wyrazie. Upewnij się, czy ten atrybut zostanie ustawiony tryb formularzy, z wielkiej litery F. Jeśli używasz innej wielkości znaków, takich jak formularze, otrzymasz błąd konfiguracji podczas odwiedzania witryn za pośrednictwem przeglądarki.


&lt;Uwierzytelniania&gt; elementu może opcjonalnie obejmować &lt;formularzy&gt; elementu podrzędnego, który zawiera ustawienia specyficzne dla uwierzytelniania formularzy. Na razie Przyjrzyjmy wystarczy użyć domyślne ustawienia uwierzytelniania formularzy. Przeanalizujemy &lt;formularzy&gt; element podrzędny bardziej szczegółowo w następnym samouczku.

## <a name="step-3-building-the-login-page"></a>Krok 3. Tworzenie strony logowania

Aby obsługiwać uwierzytelnianie formularzy naszej witryny sieci Web musi strony logowania. Zgodnie z opisem w interpretacji sekcji przepływu pracy uwierzytelniania formularzy, FormsAuthenticationModule spowoduje automatyczne przekierowanie użytkownika do strony logowania, gdy próbują uzyskać dostęp do strony, które nie są one uprawnienia do wyświetlenia. Dostępne są także formantów sieci Web platformy ASP.NET, które zostanie wyświetlone łącze do strony logowania dla użytkowników anonimowych. Wymaga to pytanie, w jaki jest adres URL strony logowania?

Domyślnie system uwierzytelniania formularzy oczekuje strony logowania, aby mieć nazwę Login.aspx i umieszczane w katalogu głównym aplikacji sieci web. Jeśli chcesz użyć innej nazwy logowania adresu URL strony, możesz to zrobić, określając je w pliku Web.config. Zobaczymy jak to zrobić w kolejnym samouczku.

Strona logowania ma trzy obowiązki:

1. Podaj interfejsu, który umożliwia obiektu odwiedzającego wprowadzić swoje poświadczenia.
2. Ustal, czy przesłane poświadczenia są prawidłowe.
3. Zaloguj użytkownika, tworząc bilet uwierzytelniania formularzy.

### <a name="creating-the-login-pages-user-interface"></a>Tworzenie interfejsu użytkownika strony logowania

Zacznijmy od pierwszego zadania. Dodawanie nowej strony programu ASP.NET do katalogu głównego witryny o nazwie Login.aspx i skojarz go ze stroną wzorcową Site.master.


[![Dodawanie nowej strony programu ASP.NET o nazwie Login.aspx](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**Rysunek 08**: Dodaj nowy ASP.NET strony o nazwie Login.aspx ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image24.png))


Interfejs strony logowania typowe składa się z dwóch pól tekstowych — jeden dla nazwy użytkownika, po jednym dla hasła — i przycisk, który można przesłać formularza. Witryny sieci Web często obejmują Pamiętaj mnie pole wyboru, które, jeśli opcja jest zaznaczona, są utrwalane wynikowy biletu uwierzytelniania w ponownego uruchomienia przeglądarki.

Dodawanie dwóch pól tekstowych do strony Login.aspx i ustawiać ich właściwości Identyfikatora nazwy użytkownika i hasła, odpowiednio. Właściwość trybie tekstowym hasła należy również ustawić hasło. Następnie należy dodać kontrolkę pola wyboru, ustawiając jego właściwość ID RememberMe i jego właściwość Text na Pamiętaj mnie. Poniżej Dodaj przycisk o nazwie LoginButton, którego właściwość tekst jest ustawiona na nazwę logowania. Na koniec Dodaj formant etykiety w sieci Web i ustaw jej właściwość ID InvalidCredentialsMessage, jego właściwość Text na nazwę użytkownika lub hasło jest nieprawidłowe. Spróbuj ponownie., jego właściwości ForeColor czerwony, a jego właściwość Visible wartość false.

W tym momencie ekran powinien wyglądać podobnie do ekranu zrzut na rysunku 9, a następnie na stronie składni deklaratywnej zasady powinny budzić zaufanie następujące czynności:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]


[![Na stronie logowania zawiera dwa pola tekstowe, pola wyboru, przycisk i etykietę](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**Rysunek 09**: Logowania strona zawiera dwa pola tekstowe, pola wyboru, przycisk i etykietę ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image27.png))


Na koniec Utwórz procedurę obsługi zdarzeń Click LoginButton zdarzeń. Przy użyciu projektanta po prostu kliknij dwukrotnie formant przycisku aby utworzyć ten program obsługi zdarzeń.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Określanie, czy podane poświadczenia są prawidłowe

Teraz musisz zaimplementować zadanie 2 w przypadku kliknięcia przycisku program obsługi zdarzeń - określenie, czy podane poświadczenia są prawidłowe. Aby to zrobić, to musi istnieć w magazynie użytkownika, który przechowuje wszystkie poświadczenia użytkowników, dzięki czemu można określić, jeśli podane poświadczenia zgodny z żadnych znanych poświadczeń.

Przed ASP.NET 2.0 deweloperzy były odpowiada za zaimplementowanie własnej magazynami użytkowników i pisanie kodu w celu zweryfikowania podanych poświadczeń względem magazynu. Większość programistów będzie implementować magazyn użytkownika w bazie danych, tworzenie tabeli o nazwie Użytkownicy z kolumny, takie jak nazwa użytkownika, hasło, adres E-mail, LastLoginDate i tak dalej. Wówczas, ta tabela będzie miała jeden rekord dla konta użytkownika. Weryfikowanie podanych poświadczeń użytkownika obejmowałaby zapytanie do bazy danych dla dopasowanej nazwy użytkownika i zapewnienie, że podane hasło odpowiadał hasła w bazie danych.

Za pomocą programu ASP.NET 2.0 deweloperzy należy używać jednego z dostawców członkostwa do zarządzania magazynie użytkownika. W tej serii samouczków firma Microsoft będzie używać SqlMembershipProvider, która korzysta z bazy danych programu SQL Server dla magazynu użytkowników. Korzystając z SqlMembershipProvider musimy zaimplementować schematu konkretnej bazy danych, który zawiera tabele, widoki i składowane oczekiwany przez dostawcę. Zostanie omówiony sposób implementacji tego schematu w *[tworzenie schematu członkostwa w programie SQL Server](../membership/creating-the-membership-schema-in-sql-server-vb.md)* samouczka. Za pomocą dostawcy członkostwa w miejscu, sprawdzanie poprawności poświadczeń użytkownika jest równie proste co wywołanie metody [klasa członkowska](https://msdn.microsoft.com/library/system.web.security.membership.aspx)firmy [ValidateUser (*username*, *hasło*) Metoda](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx), która zwraca wartość logiczną wskazującą czy ważności *username* i *hasło* kombinacji. Obserwowanie, jak możemy jeszcze nie zaimplementowano SqlMembershipProvider magazyn użytkownika, firma Microsoft nie można użyć metody ValidateUser klasy członkostwa w tej chwili.

Zamiast Poświęć chwilę aby tworzyć własne niestandardowe użytkowników tabeli bazy danych (która jest przestarzałe po wdrożyliśmy SqlMembershipProvider), umożliwia zamiast trwale kodować prawidłowe poświadczenia w ramach logowania stronie. Program obsługi zdarzeń kliknięcia, Dodaj następujący kod w LoginButton:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

Jak widać, istnieją trzy prawidłowe konta użytkownika — Scott, Jisun i Sam — a wszystkie trzy mają tego samego hasła (hasło). Kod w pętli tablic użytkowników i haseł, szuka dopasowania prawidłową nazwę użytkownika i hasło. Jeśli nazwy użytkownika i hasło są prawidłowe, należy zalogować użytkownika i przekierowywać je do odpowiedniej strony. Jeśli poświadczenia są nieprawidłowe, możemy wyświetlić etykiety InvalidCredentialsMessage.

Gdy użytkownik wprowadzi prawidłowe poświadczenia, wspomniałem, następnie nastąpi przekierowanie do odpowiedniej strony. Co to jest odpowiedniej strony, mimo że? Pamiętaj, że gdy użytkownik odwiedzi strony, które nie mają uprawnień do wyświetlania, FormsAuthenticationModule automatycznie przekierowuje go do strony logowania. W ten sposób obejmuje żądanego adresu URL w zmiennej querystring za pomocą parametru ReturnUrl. Oznacza to jeśli użytkownik próbował odwiedź ProtectedPage.aspx, a nie miało uprawnień w tym celu, FormsAuthenticationModule będzie przekierowywać je do:

Login.aspx?ReturnUrl=ProtectedPage.aspx

Po pomyślnym zalogowaniu użytkownika powinien być przekierowany z powrotem do ProtectedPage.aspx. Alternatywnie użytkownicy mogą odwiedzić strony logowania na ich własnych volition. W takim przypadku po zalogowaniu się użytkownika powinny być wysłane do strony Default.aspx folderu głównego.

### <a name="logging-in-the-user"></a>Logowanie użytkownika

Przy założeniu, że podane poświadczenia są prawidłowe, należy utworzyć bilet uwierzytelniania formularzy, a tym samym logowania użytkownika do witryny. [Klasy FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) w [przestrzeń nazw System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) udostępnia są formatowane przy metody do rejestrowania, a następnie wylogowywania użytkowników za pośrednictwem formularzy systemu uwierzytelniania. Istnieje kilka metod w klasie uwierzytelniania formularzy, 3, w którego przypadku interesuje NAS w tym momencie są:

- [GetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) — tworzy bilet uwierzytelniania formularzy dla podanej nazwy *username*. Następnie ta metoda tworzy i zwraca obiekt HttpCookie, który znajduje się zawartość biletu uwierzytelniania. Jeśli *persistCookie* ma wartość PRAWDA, trwały plik cookie jest tworzony.
- [SetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) -wywołuje GetAuthCookie (*username*, *persistCookie*) metodę w celu wygenerowania pliku cookie uwierzytelniania formularzy. Ta metoda dodaje plik cookie zwrócony przez GetAuthCookie kolekcję plików cookie (przy założeniu formularzy opartych na pliki cookie uwierzytelniania jest używany; w przeciwnym razie, ta metoda wywołuje Klasa wewnętrzna, która obsługuje logiki cookieless biletu).
- [RedirectFromLoginPage (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) — ta metoda wywołuje SetAuthCookie (*username*, *persistCookie*), a następnie przekierowuje użytkownika do odpowiedniej strony.

GetAuthCookie jest przydatna, gdy trzeba zmodyfikować biletu uwierzytelniania przed zapisaniem pliku cookie zbioru plików cookie. SetAuthCookie jest przydatne, jeśli chcesz utworzyć bilet uwierzytelniania formularzy i dodać go do kolekcji plików cookie, ale nie chcesz przekierować użytkownika do odpowiedniej strony. Być może chcesz zachować je na stronie logowania lub wysyłać je do niektórych alternatywnej strony.

Ponieważ chcemy zalogować użytkownika i Przekierowanie do odpowiedniej strony, użyjemy RedirectFromLoginPage. Aktualizuj LoginButton kliknij obsługi zdarzeń, zastępując dwa wiersze komentarze TODO następującego kodu:

FormsAuthentication.RedirectFromLoginPage(UserName.Text, RememberMe.Checked)

Podczas tworzenia biletu uwierzytelniania formularzy używamy właściwości tekstu w polu tekstowym Nazwa użytkownika dla biletu uwierzytelniania formularzy *username* parametr i stan zaznaczenia pola wyboru RememberMe dla  *persistCookie* parametru.

Aby przetestować stronę logowania, odwiedź stronę w przeglądarce. Rozpocznij, wprowadzając nieprawidłowe poświadczenia, takie jak nazwa Nope i hasło problem. Po kliknięciu przycisku logowania. nastąpi odświeżenie strony i będzie wyświetlana etykieta InvalidCredentialsMessage.


[![Etykieta InvalidCredentialsMessage jest wyświetlany po wprowadzeniu nieprawidłowe poświadczenia](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**Na rysunku nr 10**: Etykieta InvalidCredentialsMessage jest wyświetlany po wprowadzeniu nieprawidłowych poświadczeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image30.png))


Następnie wprowadź prawidłowe poświadczenia i kliknij przycisk logowania. Tym razem, gdy zwrotu wystąpi biletu uwierzytelniania formularzy jest tworzony i automatycznie nastąpi przekierowanie do Default.aspx. W tym momencie użytkownik zalogował się do witryny sieci Web, mimo że nie ma żadnych podpowiedzi wizualne, aby wskazać, że użytkownik jest aktualnie zalogowany. W kroku 4, należy sprawdzić, jak programowo określić, czy użytkownik jest zalogowany nie, a także sposób identyfikacji użytkownika, odwiedzając stronę.

Krok 5 sprawdza technik do logowania użytkownika z witryny sieci Web.

### <a name="securing-the-login-page"></a>Zabezpieczanie strony logowania

Gdy użytkownik wprowadzi swoje poświadczenia i przesyła formularz strony logowania, poświadczenia — w tym jej hasła — są przesyłane przez Internet do serwera sieci web w *zwykły tekst*. Oznacza to, że wszelkie haker wykrywanie ruchu sieciowego można zobaczyć, nazwę użytkownika i hasło. Aby temu zapobiec, jest niezbędne do szyfrowania ruchu sieciowego przy użyciu [gniazda warstwy SSL (Secure)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Pozwoli to zagwarantować, są zaszyfrowane poświadczenia (a także kod znaczników HTML całą stronę) od momentu jego opuszczają przeglądarki otrzymanie przez serwer sieci web.

Chyba że witryny sieci Web zawiera poufne informacje, wystarczy do używania protokołu SSL na stronie logowania i na innych stronach których hasło użytkownika, w przeciwnym razie będą przesyłane przez sieć w postaci zwykłego tekstu. Nie trzeba się martwić o zabezpieczaniu biletu uwierzytelniania formularzy, ponieważ, domyślnie jest on zarówno zaszyfrowany i podpisany cyfrowo (w celu uniknięcia niepowołanych manipulacji). Dokładniejsze omówienie na zabezpieczenia biletów uwierzytelniania formularzy jest przedstawiona w następującego samouczka.

> [!NOTE]
> Wiele witryn sieci Web finansowych i medycznych są skonfigurowane do używania protokołu SSL na *wszystkich* stron dostępne dla uwierzytelnionych użytkowników. Jeśli tworzysz takie witryny sieci Web systemu uwierzytelniania formularzy można skonfigurować tak, aby biletu uwierzytelniania formularzy są jedynie przesyłane za pośrednictwem bezpiecznego połączenia. Przyjrzymy się różne opcje konfiguracji uwierzytelniania formularzy w następnym samouczku  *[Konfiguracja uwierzytelniania formularzy i Tematy zaawansowane](../membership/creating-the-membership-schema-in-sql-server-vb.md)*.


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Krok 4. Wykrywanie uwierzytelnionego odwiedzających i ich identyfikacji

W tym momencie możemy włączono uwierzytelniania formularzy oraz utworzone strony logowania podstawowe, ale musimy jeszcze sprawdzić, jak możemy określić, czy użytkownik ma uwierzytelnieni lub anonimowi. W niektórych scenariuszach firma Microsoft chcą wyświetlić inne dane lub informacje w zależności od tego, czy uwierzytelnieni lub anonimowi odwiedzania strony. Ponadto często potrzebujemy muszą znać tożsamość uwierzytelnionego użytkownika.

Możemy rozszerzyć istniejącą stronę Default.aspx w celu przedstawienia tych metod. W Default.aspx Dodaj dwa formanty panelu jeden AuthenticatedMessagePanel o nazwie i inny AnonymousMessagePanel nazwanych. Dodaj kontrolkę typu etykieta o nazwie WelcomeBackMessage w pierwszym panelu. W drugim panelu Dodawanie kontrolki Hiperlinku, ustaw jego właściwość Text na logowanie i jego właściwość NavigateUrl ~ / Login.aspx. W tym momencie oznaczeniu deklaracyjnym dla Default.aspx powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

Jak ma prawdopodobnie złamać, przeprowadzona, w tym miejscu chodzi o to mają być wyświetlane tylko AuthenticatedMessagePanel uwierzytelnionego odwiedzających i po prostu AnonymousMessagePanel anonimowych gościom. W tym celu należy ustawić tych paneli właściwości widocznych w zależności od tego, czy użytkownik jest zalogowany lub nie.

[Właściwość Request.IsAuthenticated](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) zwraca wartość logiczną wskazującą, czy żądanie zostało uwierzytelnione. Wprowadź następujący kod do strony\_obciążenia kod procedury obsługi zdarzeń:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

Przy użyciu tego kodu w miejscu odwiedź stronę Default.aspx za pośrednictwem przeglądarki. Przy założeniu, że masz jeszcze się zalogować, zostanie wyświetlony link do strony logowania (zobacz rysunek 11). Kliknij ten link i zaloguj się do witryny. Jak widzieliśmy w kroku 3, po wprowadzeniu poświadczeń nastąpi powrót do Default.aspx, ale tym razem na stronie znajdują się Witamy z powrotem! komunikat (zobacz rysunek 12).


[![Gdy, odwiedzając anonimowo, w dzienniku łącza zostanie wyświetlona](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**Rysunek 11**: Podczas odwiedzania anonimowo, jest wyświetlany w dzienniku Link ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image33.png))


[![Uwierzytelnieni użytkownicy są wyświetlane Witamy z powrotem! Komunikat](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**Rysunek 12**: Uwierzytelnieni użytkownicy są wyświetlane Witamy z powrotem! Komunikat ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image36.png))


Można określić aktualnie zalogowanego użytkownika tożsamości za pośrednictwem [obiektu HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)firmy [właściwości użytkownika](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx). Obiektu HttpContext reprezentuje informacje o bieżącym żądaniu i jest główną dla tych wspólnych obiektów ASP.NET jako odpowiedzi, żądania i sesji, między innymi. Właściwości użytkownika reprezentuje w kontekście zabezpieczeń bieżącego żądania HTTP i implementuje [interfejsu IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

Właściwość użytkownika jest ustawiana przez FormsAuthenticationModule. W szczególności jeśli FormsAuthenticationModule znajdzie biletu uwierzytelniania formularzy w żądaniu przychodzącym, jego tworzy nowy obiekt obiektów GenericPrincipal i przypisuje go do właściwości użytkownika.

Obiekty główne (na przykład obiektów GenericPrincipal) zawierają informacje o tożsamości użytkowników i ról, do których należą. Interfejsu IPrincipal definiuje dwa elementy członkowskie:

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) — metodę, która zwraca wartość logiczną wskazującą, czy podmiot zabezpieczeń należy do określonej roli.
- [Tożsamość](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) — właściwość, która zwraca obiekt, który implementuje [interfejsu IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). Interfejs IIdentity definiuje trzy właściwości: [Element AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [właściwości](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx), i [nazwa](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Można określić nazwę bieżącego obiektu odwiedzającego, używając następującego kodu:

Dim currentUsersName As String = User.Identity.Name

Podczas uwierzytelniania, przy użyciu formularzy [obiektu FormsIdentity](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) jest tworzona dla obiektów GenericPrincipal właściwości tożsamości. Klasa FormsIdentity zawsze zwraca formularzy ciągu dla jego właściwości AuthenticationType i wartość True dla właściwości jego właściwości. Właściwość Name zwraca nazwę użytkownika określone podczas tworzenia biletu uwierzytelniania formularzy. Oprócz tych trzech właściwości FormsIdentity obejmuje dostęp do podstawowych biletu uwierzytelniania za pośrednictwem jego [biletu właściwość](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). Właściwości Ticket zwraca obiekt typu [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), który ma właściwości, takie jak wygaśnięcia, IsPersistent, IssueDate, nazwy i tak dalej.

Należy koniecznie zwrócić na wynos, w tym miejscu jest to, że *username* parametr określony w FormsAuthentication.GetAuthCookie (*username*, *persistCookie*), FormsAuthentication.SetAuthCookie (*username*, *persistCookie*), a FormsAuthentication.RedirectFromLoginPage (*username*, *persistCookie*) metod jest taka sama wartość zwrócona przez User.Identity.Name. Ponadto biletu uwierzytelniania utworzone przy użyciu tej metody jest dostępna przez rzutowanie User.Identity obiektowi FormsIdentity i następnie uzyskiwanie dostępu do właściwości Ticket:

Dim ident jako FormsIdentity =, CType (User.Identity, FormsIdentity)

Dim authTicket jako FormsAuthenticationTicket = ident. Bilet

Teraz zawierają bardziej spersonalizowane w Default.aspx. Aktualizuj stronę\_obciążenia programu obsługi zdarzeń, aby właściwość tekst etykiety WelcomeBackMessage przypisano ciąg Witaj, *username*!

WelcomeBackMessage.Text = "Witaj ponownie," &amp; User.Identity.Name &amp; "!"

Rysunek 13 przedstawiono wpływ tej zmiany (po zalogowaniu się jako użytkownik Scott).


[![Wiadomość powitalna obejmuje aktualnie zarejestrowane nazwy użytkownika](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**Rysunek 13**: Wiadomość powitalna zawiera nazwę obecnie rejestrowane w użytkownika ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image39.png))


### <a name="using-the-loginview-and-loginname-controls"></a>Przy użyciu widoku logowania i kontroli nazwa logowania

Wyświetlanie różnych zawartości do uwierzytelnionego i anonimowych użytkowników jest typowym wymogiem; Dlatego jest wyświetlana nazwa aktualnie zalogowanego użytkownika. Z tego powodu program ASP.NET zawiera dwie kontrolki sieci Web, które zapewniają taką samą funkcjonalność, jak pokazano na rysunku 13, ale bez konieczności pisania nawet wiersza kodu.

[Kontrolki widoku logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) jest oparty na szablonie formant sieci Web, która ułatwia wyświetlanie różnych danych do użytkowników uwierzytelnionych i anonimowych. Widoku logowania zawiera dwa wstępnie zdefiniowanych szablonów:

- AnonymousTemplate - marżę dodane do tego szablonu jest wyświetlana tylko anonimowych gościom.
- LoggedInTemplate - znaczników ten szablon jest wyświetlany tylko do uwierzytelnionych użytkowników.

Dodajmy kontrolki widoku logowania do strony głównej w naszej witrynie, Site.master. Zamiast dodawać tylko kontrolki widoku logowania, jednak dodamy zarówno nowy formant ContentPlaceHolder, a następnie umieść kontrolki widoku logowania w ramach tej nowej ContentPlaceHolder. Uzasadnienie dla tej decyzji staną się jasne wkrótce.

> [!NOTE]
> Oprócz AnonymousTemplate i LoggedInTemplate kontrolki widoku logowania mogą obejmować szablony specyficzne dla ról. Szablony specyficzne dla ról Pokaż adiustację tylko do użytkowników należących do określonej roli. W przyszłości samouczka będziemy sprawdzać funkcje kontrolki widoku logowania opartej na rolach.


Rozpocznij od dodania ContentPlaceHolder, o nazwie LoginContent do strony wzorcowej w nawigacji &lt;div&gt; elementu. Można po prostu przeciągnij formant ContentPlaceHolder z przybornika do widoku źródła umieszczenia wynikowego znaczniki nad zadań do wykonania: Menu znajdzie się tutaj tekst.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

Następnie dodaj kontrolki widoku logowania LoginContent ContentPlaceHolder. Zawartość umieszczane kontrolek ContentPlaceHolder strony wzorcowej są traktowane jako *domyślnej zawartości* dla ContentPlaceHolder. Oznacza to stron ASP.NET, które korzystają z tej strony wzorcowej można określić własne zawartości dla każdego ContentPlaceHolder lub użyć zawartości domyślnej strony wzorcowej.

Widoku logowania i inne kontrolki skojarzone z logowaniem znajdują się w karcie logowania przybornika.


[![Kontrolki widoku logowania w przyborniku](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**Rysunek 14**: Kontrolki widoku logowania w przyborniku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image42.png))


Następnie dodaj dwa &lt;br /&gt; elementy natychmiast po kontrolki widoku logowania, ale nadal mieszczą się w ContentPlaceHolder. W tym momencie nawigacji &lt;div&gt; znaczników elementu powinien wyglądać podobnie do poniższego:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

Szablony widoku logowania można zdefiniować z projektanta lub oznaczeniu deklaracyjnym. Przy użyciu projektanta programu Visual Studio rozwiń tagu inteligentnego widoku logowania, która wyświetla skonfigurowane szablony na liście rozwijanej. Wpisz tekst Witaj, obcej osoby do AnonymousTemplate; następnie dodaj kontrolkę Hiperlinku i ustaw jego właściwości tekstu i NavigateUrl logowanie i ~ / Login.aspx, odpowiednio.

Po skonfigurowaniu AnonymousTemplate, przełącz się do LoggedInTemplate i wprowadzania tekstu, "Witamy z powrotem,". Następnie przeciągnij kontrolki nazwy logowania z przybornika do LoggedInTemplate, umieszczając go bezpośrednio po "Witaj," tekstu. [Kontrolki nazwy logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), jak jego nazwa wskazuje, wyświetla nazwę aktualnie zalogowanego użytkownika. Wewnętrznie kontrolki nazwy logowania po prostu Wyświetla właściwości User.Identity.Name

Po wprowadzeniu te dodatki do szablonów widoku logowania, znaczników powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

Z tym dodatkiem strony wzorcowej Site.master każda strona w naszej witryny sieci Web zostanie wyświetlony komunikat różne w zależności od tego, czy użytkownik jest uwierzytelniony. Rysunek 15 pokazuje strony Default.aspx po odwiedzeniu za pośrednictwem przeglądarki przez użytkownika Jisun. Witamy ponownie, Jisun wiadomości jest powtórzony dwa razy: jeden raz w sekcji nawigacji strony wzorcowej po lewej stronie (za pomocą kontrolki widoku logowania właśnie dodaliśmy) i jeden raz w Default.aspx zawartość obszaru (za pomocą kontrolki Panel i logiki programowej).


[![Zapraszamy Wyświetla kontrolki widoku logowania ponownie Jisun.](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**Rysunek 15**: Zapraszamy Wyświetla kontrolki widoku logowania ponownie Jisun. ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image45.png))


Ponieważ dodaliśmy widoku logowania na stronie wzorcowej może znajdować się w każdej strony w naszej witrynie. Jednak mogą istnieć stron sieci web gdy nie chcemy pokazuj tego komunikatu. Jedna takie strona jest strony logowania, ponieważ link do strony logowania jest prawdopodobnie poza ma miejsce. Ponieważ firma Microsoft umieszczone kontrolki widoku logowania w ContentPlaceHolder na stronie głównej, firma Microsoft można zastąpić ten kod znaczników domyślne na naszej stronie zawartości. Otwórz Login.aspx i przejdź do projektanta. Ponieważ firma Microsoft nie został jawnie zdefiniowany kontrolkę zawartości w Login.aspx dla LoginContent ContentPlaceHolder na stronie głównej, na stronie logowania będzie wyświetlana znaczników domyślnej strony wzorcowej dla tego elementu ContentPlaceHolder. Widać to za pomocą projektanta - LoginContent ContentPlaceHolder Pokazuje znaczniki domyślne (kontrolki widoku logowania).


[![Na stronie logowania przedstawiono domyślne zawartości dla strony wzorcowej LoginContent ContentPlaceHolder](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**Rysunek 16**: Na stronie logowania znajdują się domyślne zawartości dla strony wzorcowej LoginContent ContentPlaceHolder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image48.png))


Aby zastąpić domyślne znaczniki dla LoginContent ContentPlaceHolder, kliknij prawym przyciskiem myszy na region w Projektancie i wybierz opcję Utwórz zawartość niestandardowe z menu kontekstowego. (Gdy za pomocą programu Visual Studio 2008 ContentPlaceHolder zawiera tag inteligentny, po wybraniu oferuje tę samą opcję.) Spowoduje to dodanie nowego formantu zawartości strony znaczników i tym samym pozwala nam do definiowania niestandardowej zawartości dla tej strony. Można dodać niestandardowy komunikat, np. Zaloguj się, ale możemy po prostu pozostaw to pole puste.

> [!NOTE]
> W programie Visual Studio 2005, tworząc niestandardową zawartość utworzy pustą zawartość kontrolki na stronie ASP.NET. W programie Visual Studio 2008 jednak tworzenia niestandardowej zawartości kopiuje zawartość domyślna strony wzorcowej do nowo utworzonego formantu zawartości. Jeśli używasz programu Visual Studio 2008 następnie, po utworzeniu nowej kontrolki zawartości upewnij się umożliwić wyczyszczenie zawartości skopiowanych z strony wzorcowej.


Rysunek 17 pokazuje strony Login.aspx po odwiedzeniu z poziomu przeglądarki po wprowadzeniu tej zmiany. Należy pamiętać, że nie Witaj, osoba nieznana lub Witamy z powrotem *username* wiadomości w lewym panelu nawigacyjnym &lt;div&gt; się podczas odwiedzania Default.aspx.


[![Na stronie logowania ukrywa znaczników LoginContent ContentPlaceHolder domyślne](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**Rysunek 17**: Na stronie logowania ukrywa znaczników domyślnych LoginContent ContentPlaceHolder firmy ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image51.png))


## <a name="step-5-logging-out"></a>Krok 5. Wylogowanie

W kroku 3 przyjrzeliśmy się tworzenie strony logowania do logowania się użytkownika w witrynie, ale mamy jeszcze zobaczyć, jak wylogować użytkownika. Oprócz metody logowania użytkownika, klasa FormsAuthentication udostępnia również podpowiedzi [metoda wylogowanie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). Metoda wylogowanie po prostu niszczy biletu uwierzytelniania formularzy, w tym samym rejestrowanie użytkowników spoza witryny.

Oferta wylogowania łącze jest wspólną cechą tego ASP.NET zawiera kontrolkę specjalnie do wylogowania użytkownika. [Kontrolki stanu logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) Wyświetla LinkButton logowania lub element LinkButton wylogowania, w zależności od stanu uwierzytelniania użytkownika. LinkButton logowania jest renderowany dla użytkowników anonimowych, natomiast LinkButton wylogowania jest wyświetlany użytkownikom uwierzytelnionym. Tekst dla nazwy logowania i wylogowania LinkButtons mogą być konfigurowane przez stanu logowania LoginText i LogoutText właściwości.

Klikając element LinkButton logowania powoduje odświeżenie strony, z której wystawiono przekierowanie do strony logowania. Klikając element LinkButton wylogowania powoduje, że kontrolki stanu logowania, można wywołać metody FormsAuthentication.SignOff, a następnie przekierowuje użytkownika do strony. Strona zalogowanego off użytkownik zostaje przekierowany do zależy od właściwości LogoutAction można przypisać do jednej z trzech następujących wartości:

- Odświeżanie — domyślnie; przekierowuje użytkownika do strony, na których zostały one po prostu odwiedzający. Jeśli strony, w której zostały one po prostu odwiedzający nie zezwala na użytkowników anonimowych, następnie FormsAuthenticationModule automatycznie przekierowuje użytkownika do strony logowania.

Być może zastanawiasz się, by Dlaczego przekierowanie odbywa się w tym miejscu. Jeśli użytkownik chce, aby nadal korzystać z tej samej stronie, dlaczego potrzebę jawnego przekierowania? Przyczyną jest, ponieważ po kliknięciu LinkButton wylogowanie, użytkownik nadal ma biletu uwierzytelniania formularzy w ich kolekcji plików cookie. W związku z tym żądanie zwrotu jest uwierzytelnionego żądania. Kontrolki stanu logowania wywołuje metodę Wyloguj się, ale tak się stanie po FormsAuthenticationModule uwierzytelnił użytkownika. W związku z tym jawne przekierowanie powoduje, że przeglądarkę, aby ponownie stronę. W czasie, przeglądarce ponownie zgłasza żądanie strony biletu uwierzytelniania formularzy została usunięta, a w związku z tym przychodzące żądanie jest anonimowy.

- Przekieruj — użytkownik jest przekierowywany do adresu URL określonego przez właściwość LogoutPageUrl stanu logowania.
- RedirectToLoginPage — użytkownik jest przekierowywany na stronę logowania.

Dodajmy kontrolki stanu logowania na stronie wzorcowej i skonfiguruj go pod kątem użycia opcji przekierowania wysłać użytkownika do strony, która wyświetla komunikat potwierdzający, że ich wylogowano Cię. Rozpocznij od utworzenia strony w katalogu głównym o nazwie Logout.aspx. Należy pamiętać skojarzyć tę stronę z Site.master strony wzorcowej. Następnie wprowadź komunikat w znaczniku strony wyjaśniających użytkownikowi, który został wylogowanie.

Następnie wróć do strony głównej Site.master i dodać kontrolki stanu logowania pod widoku logowania w LoginContent ContentPlaceHolder. Ustaw właściwość LogoutAction kontrolki stanu logowania przekierowania i jego właściwość LogoutPageUrl ~/Logout.aspx.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

Ponieważ stanu logowania znajduje się poza kontrolki widoku logowania, pojawi się zarówno anonimowych, jak i uwierzytelnionych użytkowników, ale to normalne, ponieważ nazwy logowania lub wylogowywania LinkButton będą poprawnie wyświetlać stanu logowania. Dodając kontrolki stanu logowania dziennika w Hiperlink w AnonymousTemplate jest zbędny, więc można go usunąć.

Rysunek 18 pokazuje Default.aspx, gdy odwiedza Jisun. Należy pamiętać, że kolumna po lewej stronie wyświetla komunikat, Witamy z powrotem, Jisun wraz z linkiem do wylogowania. Klikając przycisk wylogowania LinkButton powoduje odświeżenie strony, wylogowania Jisun systemu i przekierowuje jej Logout.aspx. Jak pokazano na rysunku 19, przez razem, gdy Jisun osiągnie Logout.aspx ona już został wyrejestrowany i dlatego jest anonimowy. W związku z tym kolumna po lewej stronie zawiera tekst powitania obcej osoby oraz link do strony logowania.


[![Default.aspx pokazuje Witamy z powrotem, Jisun wraz z LinkButton wylogowania](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**Rysunek 18**: Default.aspx pokazuje Zapraszamy ponownie, Jisun wraz z LinkButton wylogowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image54.png))


[![Logout.aspx pokazuje — Zapraszamy obcej osoby wraz z LinkButton logowania](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**Rysunek 19**: Logout.aspx pokazuje — Zapraszamy obcej osoby wraz z LinkButton logowania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image57.png))


> [!NOTE]
> Zachęcam Cię do dostosowania strony Logout.aspx, aby ukryć LoginContent ContentPlaceHolder strony wzorcowej (takich jak zrobiliśmy Login.aspx w kroku 4). Ponieważ element LinkButton logowania renderowany przez kontrolki stanu logowania (jeden pod Witaj, osoba nieznana) wysyła użytkownika do strony logowania, przekazując parametr querystring ReturnUrl bieżący adres URL. Krótko mówiąc Jeśli użytkownik zalogował się kliknie element LinkButton logowania tego stanu logowania, a następnie loguje się, zostanie przekierowany do Logout.aspx, który można łatwo zirytować użytkownika.


## <a name="summary"></a>Podsumowanie

W tym samouczku będziemy pracę z analizę przepływu pracy uwierzytelniania formularzy i skonfigurowała do wdrażania uwierzytelniania formularzy w aplikacji ASP.NET. Uwierzytelnianie formularzy jest obsługiwana przez FormsAuthenticationModule, która ma dwa obowiązki: Identyfikowanie użytkowników w oparciu o ich biletu uwierzytelniania formularzy i Przekierowanie nieautoryzowanych użytkowników do strony logowania.

Klasa uwierzytelniania formularzy programu .NET Framework zawiera metody do tworzenia, inspekcja i usuwania biletów uwierzytelniania formularzy. Właściwość Request.IsAuthenticated i obiektu użytkownika zapewnienia dodatkowego wsparcia programowego określająca, czy żądanie jest uwierzytelniane i informacje o tożsamości użytkownika. Dostępne są także formanty widoku logowania, stanu logowania i nazwa logowania w sieci Web, które dają deweloperom sposób szybki, niekorzystające z kodu umożliwiające wykonywanie wielu typowych zadań skojarzone z logowaniem. Będziemy sprawdzać te i inne kontrolki sieci Web skojarzone z logowaniem większej liczby szczegółów przyszłych samouczków.

W tym samouczku pod warunkiem pobieżną omówienie uwierzytelniania formularzy. Firma Microsoft nie Sprawdź opcje konfiguracji są formatowane przy, Przyjrzyj się jak cookieless działania biletów uwierzytelniania formularzy lub Poznaj, jak ASP.NET chroni zawartość biletu uwierzytelniania formularzy. Omówimy te tematy i innych elementów [następnego samouczka](forms-authentication-configuration-and-advanced-topics-vb.md).

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Zmian między wersjami usług IIS 6 i zabezpieczeń usług IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Kontrolki ASP.NET logowania](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Profesjonalne platformy ASP.NET 2.0 zabezpieczeń, członkostwo i zarządzanie rolami](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [&lt;Uwierzytelniania&gt; — Element](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;Formularzy&gt; elementu &lt;uwierzytelniania&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Szkolenie wideo na tematy zawarte w tym samouczku

- [Korzystanie z uwierzytelniania podstawowego formularzy na platformie ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>Informacje o autorze

Pracował nad Bento Scottem, autor wiele książek ASP/ASP.NET i założyciel 4GuysFromRolla.com, przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena  *[Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott można z Tobą skontaktować w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku obejmują Alicja Maziarz, Jan Suru i Teresa Murphy. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Poprzednie](security-basics-and-asp-net-support-vb.md)
> [dalej](forms-authentication-configuration-and-advanced-topics-vb.md)
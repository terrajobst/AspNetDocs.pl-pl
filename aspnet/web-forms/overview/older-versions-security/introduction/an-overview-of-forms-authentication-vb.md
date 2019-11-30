---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
title: Omówienie uwierzytelniania formularzy (VB) | Microsoft Docs
author: rick-anderson
description: W tym samouczku przejdziemy do implementacji zwykłej dyskusji. w szczególności zapoznaj się z zaimplementowaniem uwierzytelniania formularzy. Aplikacja sieci Web w...
ms.author: riande
ms.date: 01/14/2008
ms.assetid: 83267f7d-64d9-41ee-82cf-da91b1bf534d
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: d8ceb6b5290300992e52199caa9314c573de1942
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74626736"
---
# <a name="an-overview-of-forms-authentication-vb"></a>Omówienie uwierzytelniania formularzy (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_vb.pdf)

> W tym samouczku przejdziemy do implementacji zwykłej dyskusji. w szczególności zapoznaj się z zaimplementowaniem uwierzytelniania formularzy. Aplikacja sieci Web, która rozpoczyna konstruowanie w tym samouczku, będzie nadal skompilowana w kolejnych samouczkach, podobnie jak w przypadku przechodzenia z prostego uwierzytelniania formularzy do członkostwa i ról.
> 
> Obejrzyj ten film wideo, aby uzyskać więcej informacji na temat tego tematu: [używanie uwierzytelniania podstawowego formularzy w programie ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md).

## <a name="introduction"></a>Wprowadzenie

W [poprzednim samouczku](security-basics-and-asp-net-support-vb.md) omówiono różne opcje uwierzytelniania, autoryzacji i konta użytkownika udostępniane przez ASP.NET. W tym samouczku przejdziemy do implementacji zwykłej dyskusji. w szczególności zapoznaj się z zaimplementowaniem uwierzytelniania formularzy. Aplikacja sieci Web, która rozpoczyna konstruowanie w tym samouczku, będzie nadal skompilowana w kolejnych samouczkach, podobnie jak w przypadku przechodzenia z prostego uwierzytelniania formularzy do członkostwa i ról.

Ten samouczek rozpoczyna się od szczegółowego wyglądu przepływu pracy związanego z uwierzytelnianiem formularzy, który został rozłożony w poprzednim samouczku. Poniżej zostanie utworzona witryna sieci Web ASP.NET, za pomocą której można obejrzeć koncepcje uwierzytelniania formularzy. Następnie skonfigurujemy lokację do korzystania z uwierzytelniania formularzy, tworzenia prostej strony logowania i Dowiedz się, jak określić, w kodzie, czy użytkownik jest uwierzytelniany, a jeśli tak, to nazwa użytkownika, na którym się zalogowano.

Zrozumienie przepływu pracy uwierzytelniania formularzy, włączenie go w aplikacji sieci Web, a następnie utworzenie stron logowania i wylogowania to wszystkie istotne kroki w tworzeniu aplikacji ASP.NET, która obsługuje konta użytkowników i uwierzytelnia użytkowników za pomocą strony sieci Web. Ze względu na to, że te samouczki kompilują się po sobie nawzajem — zachęcamy do przechodzenia przez ten samouczek w całości przed przejściem do kolejnego, nawet jeśli masz już doświadczenie w konfigurowaniu uwierzytelniania formularzy w przeszłych projektach.

## <a name="understanding-the-forms-authentication-workflow"></a>Informacje o przepływie pracy uwierzytelniania formularzy

Gdy środowisko uruchomieniowe ASP.NET przetwarza żądanie dla zasobu ASP.NET, takie jak strona ASP.NET lub usługa sieci Web ASP.NET, żądanie zgłasza wiele zdarzeń w cyklu życia. Istnieją zdarzenia zgłoszone na bardzo początku i na końcu żądania, wywoływane, gdy żądanie jest uwierzytelniane i autoryzowane, zdarzenie zgłoszone w przypadku nieobsłużonego wyjątku i tak dalej. Aby wyświetlić pełną listę zdarzeń, zapoznaj się ze [zdarzeniami obiektu elemencie HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*Moduły HTTP* są klasami zarządzanymi, których kod jest wykonywany w odpowiedzi na konkretne zdarzenie w cyklu życia żądania. ASP.NET dostarcza wiele modułów HTTP, które wykonują podstawowe zadania w tle. Dwa wbudowane moduły HTTP, które są szczególnie przydatne w naszej dyskusji:

- **[FormsAuthenticationModule](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)** — uwierzytelnia użytkownika, sprawdzając bilet uwierzytelniania formularzy, który zwykle jest zawarty w kolekcji plików cookie użytkownika. Jeśli nie istnieje bilet uwierzytelniania formularzy, użytkownik jest anonimowy.
- **[UrlAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)** — określa, czy bieżący użytkownik ma uprawnienia dostępu do żądanego adresu URL. Ten moduł określa Urząd zgodnie z regułami autoryzacji określonymi w plikach konfiguracji aplikacji. ASP.NET obejmuje również [FileAuthorizationModule](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) , które określają urząd, sprawdzając wymagane pliki list ACL.

FormsAuthenticationModule próbuje uwierzytelnić użytkownika przed wykonaniem operacji UrlAuthorizationModule (i FileAuthorizationModule). Jeśli użytkownik wykonujący żądanie nie ma uprawnień dostępu do żądanego zasobu, moduł autoryzacji zakończy żądanie i zwróci stan [nieautoryzowany HTTP 401](http://www.checkupdown.com/status/E401.html) . W scenariuszach uwierzytelniania systemu Windows stan HTTP 401 jest zwracany do przeglądarki. Ten kod stanu powoduje, że przeglądarka monituje użytkownika o podanie poświadczeń za pośrednictwem modalnego okna dialogowego. W przypadku uwierzytelniania formularzy nieautoryzowany stan protokołu HTTP 401 nie jest nigdy wysyłany do przeglądarki, ponieważ FormsAuthenticationModule wykrywa ten stan i modyfikuje go, aby przekierować użytkownika do strony logowania (za pośrednictwem stanu [przekierowywania HTTP 302](http://www.checkupdown.com/status/E302.html) ).

Odpowiedzialność za stronę logowania jest określenie, czy poświadczenia użytkownika są prawidłowe, a jeśli tak, to w celu utworzenia biletu uwierzytelniania formularzy i przekierowania użytkownika z powrotem do strony, którą próbowano odwiedzać. Bilet uwierzytelniania jest uwzględniany w kolejnych żądaniach do stron w witrynie sieci Web, których FormsAuthenticationModule używa do identyfikowania użytkownika.

[![przepływ pracy uwierzytelniania formularzy](an-overview-of-forms-authentication-vb/_static/image2.png)](an-overview-of-forms-authentication-vb/_static/image1.png)

**Ilustracja 01**: przepływ pracy uwierzytelniania formularzy ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image3.png))

### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Zapamiętywanie biletu uwierzytelniania między wizytami stron

Po zalogowaniu biletu uwierzytelniania formularzy należy wysyłać z powrotem do serwera sieci Web w każdym żądaniu, tak aby użytkownik pozostawać zalogowany podczas przeglądania witryny. Jest to zwykle realizowane przez umieszczenie biletu uwierzytelniania w kolekcji plików cookie użytkownika. Pliki [cookie](http://en.wikipedia.org/wiki/HTTP_cookie) to małe pliki tekstowe, które znajdują się na komputerze użytkownika i są przesyłane w nagłówkach HTTP na każde żądanie do witryny sieci Web, która utworzyła plik cookie. W związku z tym po utworzeniu biletu uwierzytelniania formularzy i zapisaniu go w plikach cookie w przeglądarce każda kolejna wizyta w tej witrynie wysyła bilet uwierzytelniania wraz z żądaniem, co oznacza, że użytkownik identyfikuje użytkownika.

> [!NOTE]
> Demonstracja aplikacja sieci Web używana w każdym samouczku jest dostępna do pobrania. Ta aplikacja do pobrania została utworzona przy użyciu programu Visual Web Developer 2008 przeznaczonego dla .NET Framework w wersji 3,5. Ponieważ aplikacja jest przeznaczona dla programu .NET 3,5, jej plik Web. config zawiera dodatkowe elementy konfiguracji specyficzne dla 3,5. Długi scenariusz krótko, jeśli na komputerze nie jest jeszcze zainstalowany program .NET 3,5, aplikacja sieci Web do pobrania nie będzie działała bez uprzedniego usunięcia oznakowania specyficznego dla 3,5 z pliku Web. config.

Jednym z aspektów plików cookie jest ich wygaśnięcie, czyli Data i godzina odrzucenia pliku cookie przez przeglądarkę. Gdy plik cookie uwierzytelniania formularzy wygaśnie, użytkownik nie może być już uwierzytelniony i w związku z tym staje się anonimowy. Gdy użytkownik odwiedzający się z terminalu publicznego, może mieć możliwość wygaśnięcia biletu uwierzytelniania po zamknięciu przeglądarki. Podczas odwiedzania z domu jednak ten sam użytkownik może chcieć, aby bilet uwierzytelnienia został zapamiętany przez ponowne uruchomienie przeglądarki, aby nie musiały ponownie rejestrować się za każdym razem, gdy odwiedzają witrynę. Ta decyzja jest często podejmowana przez użytkownika w postaci pola wyboru Zapamiętaj mnie na stronie logowania. W kroku 3 sprawdzimy, jak zaimplementować pole wyboru Zapamiętaj mnie na stronie logowania. Poniższy samouczek dotyczy szczegółowych ustawień limitu czasu biletu uwierzytelniania.

> [!NOTE]
> Istnieje możliwość, że agent użytkownika używany do logowania się w witrynie sieci Web może nie obsługiwać plików cookie. W takim przypadku ASP.NET może używać biletów uwierzytelniania formularzy bez plików cookie. W tym trybie bilet uwierzytelniania jest zakodowany w adresie URL. Po użyciu biletów uwierzytelniania bez plików cookie i sposobu ich tworzenia i zarządzania w następnym samouczku zostanie wyświetlona wartość.

### <a name="the-scope-of-forms-authentication"></a>Zakres uwierzytelniania formularzy

FormsAuthenticationModule jest kodem zarządzanym, który jest częścią środowiska uruchomieniowego ASP.NET. W wersji 7 serwera sieci Web [Internet Information Services (IIS)](https://www.iis.net/) firmy Microsoft istnieje odrębna bariera między POTOKIEM http usługi IIS a potokiem środowiska uruchomieniowego ASP.NET. W krótkim czasie w usługach IIS w wersji 6 i starszych FormsAuthenticationModule jest wykonywane tylko wtedy, gdy żądanie jest delegowane z usług IIS do środowiska uruchomieniowego ASP.NET. Domyślnie usługi IIS przetwarzają zawartość statyczną, podobnie jak strony HTML i pliki CSS i obrazy — i są tylko w sposób nieskierowany do środowiska uruchomieniowego ASP.NET, gdy żądanie strony z rozszerzeniem. aspx,. asmx lub. ashx.

Usługi IIS 7 umożliwiają jednak zintegrowane usługi IIS i potoki ASP.NET. Za pomocą kilku ustawień konfiguracji można skonfigurować usługi IIS 7 do wywoływania FormsAuthenticationModule dla *wszystkich* żądań. Ponadto w przypadku usług IIS 7 można definiować reguły autoryzacji adresów URL dla plików dowolnego typu. Aby uzyskać więcej informacji, zobacz [zmiany między zabezpieczeniami usług IIS 6 i IIS7](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [zabezpieczenia platformy sieci Web](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements)i [zrozumienie autoryzacji adresów URL IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Długie scenariusze krótko, w wersjach wcześniejszych niż IIS 7, można używać uwierzytelniania formularzy do ochrony zasobów obsłużonych przez środowisko uruchomieniowe ASP.NET. Podobnie reguły autoryzacji adresów URL są stosowane tylko do zasobów obsłużonych przez środowisko uruchomieniowe ASP.NET. Jednak w przypadku usług IIS 7 można zintegrować FormsAuthenticationModule i UrlAuthorizationModule z potokiem HTTP usług IIS, rozszerzając tę funkcjonalność na wszystkie żądania.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Krok 1. Tworzenie witryny sieci Web ASP.NET dla tej serii samouczków

Aby dotrzeć do szerokiego grona odbiorców, ASP.NET witrynę sieci Web, którą będziemy kompilować w tej serii, zostanie utworzona za pomocą bezpłatnej wersji programu Visual Studio 2008, [Visual webdeveloper 2008](https://www.microsoft.com/express/vwd/). W bazie danych [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) zostanie wdrożony magazyn użytkowników SqlMembershipProvider. Jeśli używasz programu Visual Studio 2005 lub innej wersji programu Visual Studio 2008 lub SQL Server, nie martw się — kroki będą niemal identyczne, a wszystkie nieproste różnice będą wskazywane.

Zanim będziemy mogli skonfigurować uwierzytelnianie formularzy, potrzebujemy najpierw witryny sieci Web ASP.NET. Zacznij od utworzenia nowej witryny sieci Web ASP.NET opartej na systemie plików. Aby to osiągnąć, uruchom program Visual Web Developer, a następnie przejdź do menu plik i wybierz polecenie Nowa witryna sieci Web, wyświetlając okno dialogowe Nowa witryna sieci Web. Wybierz szablon witryna sieci Web ASP.NET, Ustaw listę rozwijaną lokalizacja na system plików, wybierz folder, w którym ma zostać umieszczona witryna sieci Web, i Ustaw język na VB. Spowoduje to utworzenie nowej witryny sieci Web z domyślną stroną ASP.NET. aspx, folderem danych\_i plikiem Web. config.

> [!NOTE]
> Program Visual Studio obsługuje dwa tryby zarządzania projektami: projekty witryny sieci Web i projekty aplikacji sieci Web. Projekty witryn sieci Web bez pliku projektu, natomiast projekty aplikacji sieci Web naśladuje architekturę projektu w programie Visual Studio .NET 2002/2003 — obejmują one plik projektu i kompilują kod źródłowy projektu w jeden zestaw, który znajduje się w folderze/bin. Program Visual Studio 2005 początkowo tylko obsługiwane projekty witryny sieci Web, mimo że model projektu aplikacji sieci Web został wprowadzony ponownie z dodatkiem Service Pack 1. Program Visual Studio 2008 oferuje modele projektów. Wersje Visual Web Developer 2005 i 2008, jednak obsługują tylko projekty witryn sieci Web. Będę używać modelu projektu witryny sieci Web. Jeśli używasz wersji innej niż Express i chcesz zamiast tego używać [modelu projektu aplikacji sieci Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) , możesz to zrobić, ale pamiętaj, że mogą wystąpić pewne rozbieżności między tym, co widzisz na ekranie, a także kroki, które należy wykonać, a także instrukcje zawarte w tych samouczkach.

[![utworzyć nową witrynę sieci Web opartą na systemie plików](an-overview-of-forms-authentication-vb/_static/image5.png)](an-overview-of-forms-authentication-vb/_static/image4.png)

**Ilustracja 02**. Tworzenie nowej witryny sieci Web opartej na systemie plików ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image6.png))

### <a name="adding-a-master-page"></a>Dodawanie strony wzorcowej

Następnie Dodaj nową stronę wzorcową do lokacji w katalogu głównym o nazwie Site. Master. [Strony wzorcowe](https://msdn.microsoft.com/library/wtxbf3hh.aspx) umożliwiają deweloperom strony Definiowanie szablonu w całej lokacji, który można zastosować do stron ASP.NET. Główną zaletą stron wzorcowych jest to, że ogólny wygląd witryny można zdefiniować w jednej lokalizacji, co ułatwia aktualizowanie lub dostosowywanie układu witryny.

[![dodać strony wzorcowej o nazwie Site. Master do witryny sieci Web](an-overview-of-forms-authentication-vb/_static/image8.png)](an-overview-of-forms-authentication-vb/_static/image7.png)

**Ilustracja 03**: Dodawanie strony wzorcowej o nazwie Site. Master do witryny sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image9.png))

W tym miejscu Zdefiniuj układ strony dla całej witryny na stronie wzorcowej. Możesz użyć widok Projekt i dodać wszelkie wymagane przez siebie kontrolki układu lub sieci Web lub ręcznie dodać znaczniki adiustacji do widoku źródła. Struktura układu strony wzorcowej w celu naśladowania układu używanego w *[pracy z danymi w](../../data-access/index.md)* serii samouczków ASP.NET 2,0 (patrz rysunek 4). Na stronie wzorcowej są stosowane [kaskadowe arkusze stylów](http://www.w3schools.com/css/default.asp) do pozycjonowania i stylów z ustawieniami CSS zdefiniowanymi w pliku style. CSS (który jest zawarty w tym samouczku związanym z pobieraniem). Chociaż nie możesz powiedzieć od znaczników przedstawionych poniżej, reguły CSS są zdefiniowane w taki sposób, aby Nawigacja &lt;DIV&gt;zawartość była pozycjonowana absolutnie, tak aby była wyświetlana po lewej stronie i ma stałą szerokość 200 pikseli.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample1.aspx)]

Strona wzorcowa definiuje zarówno statyczny układ strony, jak i regiony, które mogą być edytowane przez strony ASP.NET, które korzystają z strony wzorcowej. Te edytowalne regiony zawartości są wskazywane przez kontrolkę ContentPlaceHolder, która może być widoczna w obszarze &lt;bloku zawartości&gt;. Nasza Strona główna ma jeden element ContentPlaceHolder (kontrolka mainContent), ale strona wzorcowa może mieć wiele Elementy ContentPlaceHolders.

Po wprowadzeniu znacznika w widok Projekt zostanie wyświetlona strona wzorcowa. Wszystkie strony ASP.NET, które używają tej strony wzorcowej będą miały jednolity układ, z możliwością określenia znaczników dla regionu kontrolka mainContent.

[![stronę wzorcową, gdy jest wyświetlany w widoku projektu](an-overview-of-forms-authentication-vb/_static/image11.png)](an-overview-of-forms-authentication-vb/_static/image10.png)

**Rysunek 04**: Strona wzorcowa wyświetlana w widoku projektu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image12.png))

### <a name="creating-content-pages"></a>Tworzenie stron zawartości

W tym momencie mamy domyślną stronę. aspx w naszej witrynie sieci Web, ale nie używa ona właśnie utworzonej strony wzorcowej. Chociaż istnieje możliwość manipulowania deklaratywnym znacznikiem strony sieci Web w celu korzystania z strony wzorcowej, jeśli strona nie zawiera jeszcze żadnej zawartości, łatwiej ją usunąć i ponownie dodać do projektu, określając stronę wzorcową do użycia. W związku z tym Zacznij od usunięcia z projektu default. aspx.

Następnie kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań i wybierz polecenie Dodaj nowy formularz sieci Web o nazwie Default. aspx. Tym razem zaznacz pole wyboru Wybierz stronę wzorcową i wybierz z listy stronę wzorcową lokacja. Master.

[![dodać nową stronę Default. aspx wybierającą stronę wzorcową](an-overview-of-forms-authentication-vb/_static/image14.png)](an-overview-of-forms-authentication-vb/_static/image13.png)

**Ilustracja 05**. Dodawanie nowej strony Default. aspx Wybieranie strony wzorcowej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image15.png))

[![korzystać z strony wzorcowej witryny. Master](an-overview-of-forms-authentication-vb/_static/image17.png)](an-overview-of-forms-authentication-vb/_static/image16.png)

**Ilustracja 06**. Korzystanie z strony wzorcowej site. Master ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image18.png))

> [!NOTE]
> Jeśli używasz modelu projektu aplikacji sieci Web, okno dialogowe Dodaj nowy element nie zawiera pola wyboru Wybierz stronę wzorcową. Zamiast tego należy dodać element typu formularz zawartości sieci Web. Po wybraniu opcji formularz zawartości sieci Web i kliknięciu pozycji Dodaj program Visual Studio wyświetli to samo okno dialogowe Wybieranie wzorca pokazane na rysunku 6.

Nowa domyślna Adiustacja strony. aspx zawiera tylko dyrektywę @Page określającą ścieżkę do pliku strony głównej i kontrolkę zawartości dla elementu kontrolka mainContent.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample2.aspx)]

Na razie pozostaw puste pole default. aspx. Powrócimy do niego w dalszej części tego samouczka, aby dodać zawartość.

> [!NOTE]
> Nasza Strona główna zawiera sekcję menu lub inny interfejs nawigacji. Utworzymy taki interfejs w przyszłym samouczku.

## <a name="step-2-enabling-forms-authentication"></a>Krok 2. Włączanie uwierzytelniania formularzy

Po utworzeniu witryny sieci Web ASP.NET następnym zadaniem jest włączenie uwierzytelniania formularzy. Konfiguracja uwierzytelniania aplikacji jest określana za pomocą [elementu&lt;authentication&gt;](https://msdn.microsoft.com/library/532aee0e.aspx) w pliku Web. config. Element &lt;Authentication&gt; zawiera nazwany tryb pojedynczego atrybutu, który określa model uwierzytelniania używany przez aplikację. Ten atrybut może mieć jedną z następujących czterech wartości:

- **System Windows** — zgodnie z opisem w poprzednim samouczku, gdy aplikacja korzysta z uwierzytelniania systemu Windows, jest odpowiedzialny za serwer sieci Web do uwierzytelniania gościa. zwykle jest to realizowane za pośrednictwem podstawowego, szyfrowanego lub zintegrowanego uwierzytelniania systemu Windows.
- **Formularze**— użytkownicy są uwierzytelniani za pośrednictwem formularza na stronie sieci Web.
- **Paszport**— użytkownicy są uwierzytelniani za pomocą usługi Microsoft Passport Network.
- **Brak**— nie jest używany żaden model uwierzytelniania; Wszyscy Goście są anonimowi.

Domyślnie aplikacje ASP.NET używają uwierzytelniania systemu Windows. Aby zmienić typ uwierzytelniania na uwierzytelnianie formularzy, należy zmodyfikować atrybut trybu elementu &lt;uwierzytelnianie&gt; do formularzy.

Jeśli projekt nie zawiera jeszcze pliku Web. config, Dodaj go teraz, klikając prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wybierając pozycję Dodaj nowy element, a następnie dodając plik konfiguracji sieci Web.

[![, jeśli projekt nie zawiera jeszcze pliku Web. config, Dodaj go teraz](an-overview-of-forms-authentication-vb/_static/image20.png)](an-overview-of-forms-authentication-vb/_static/image19.png)

**Ilustracja 07**. Jeśli projekt nie zawiera jeszcze pliku Web. config, Dodaj go teraz ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image21.png))

Następnie zlokalizuj element &lt;Authentication&gt; i zaktualizuj go tak, aby korzystał z uwierzytelniania formularzy. Po tej zmianie znacznik pliku Web. config powinien wyglądać podobnie do poniższego:

[!code-xml[Main](an-overview-of-forms-authentication-vb/samples/sample3.xml)]

> [!NOTE]
> Ponieważ plik Web. config jest plikiem XML, wielkość liter jest ważna. Upewnij się, że ustawisz atrybut Mode na Forms, przy użyciu wielkiej litery F. Jeśli używasz innej wielkości liter, takiej jak formularze, podczas odwiedzania witryny za pomocą przeglądarki wystąpi błąd konfiguracji.

Element&gt; Authentication &lt;może opcjonalnie zawierać &lt;formularzy&gt; elementu podrzędnego, który zawiera ustawienia specyficzne dla uwierzytelniania formularzy. Teraz Użyjmy domyślnych ustawień uwierzytelniania formularzy. W następnym samouczku będziemy eksplorować &lt;Forms&gt; element podrzędny bardziej szczegółowo.

## <a name="step-3-building-the-login-page"></a>Krok 3. Budowanie strony logowania

Aby można było obsługiwać uwierzytelnianie formularzy, witryna sieci Web wymaga strony logowania. Zgodnie z opisem w sekcji omówienie przepływu pracy uwierzytelniania formularzy FormsAuthenticationModule automatycznie przekieruje użytkownika na stronę logowania, jeśli spróbuje uzyskać dostęp do strony, której nie mają uprawnienia do wyświetlania. Istnieją także ASP.NET formanty sieci Web, które będą wyświetlać link do strony logowania do użytkowników anonimowych. To begs pytanie, jaki jest adres URL strony logowania?

Domyślnie system uwierzytelniania formularzy oczekuje, że strona logowania jest nazywana login. aspx i umieszczona w katalogu głównym aplikacji sieci Web. Jeśli chcesz użyć innego adresu URL strony logowania, możesz to zrobić, określając je w pliku Web. config. Zobaczymy, jak to zrobić w następnym samouczku.

Strona logowania ma trzy obowiązki:

1. Podaj interfejs, który umożliwia odwiedzającemu wprowadzanie poświadczeń.
2. Ustal, czy przesłane poświadczenia są prawidłowe.
3. Zaloguj się do użytkownika, tworząc bilet uwierzytelniania formularzy.

### <a name="creating-the-login-pages-user-interface"></a>Tworzenie interfejsu użytkownika strony logowania

Zacznijmy od pierwszego zadania. Dodaj nową stronę ASP.NET do katalogu głównego witryny o nazwie login. aspx i skojarz ją z główną stroną programu site. Master.

[![dodać nową stronę ASP.NET o nazwie login. aspx](an-overview-of-forms-authentication-vb/_static/image23.png)](an-overview-of-forms-authentication-vb/_static/image22.png)

**Ilustracja 08**: Dodawanie nowej strony ASP.NET o nazwie login. aspx ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image24.png))

Typowy interfejs strony logowania składa się z dwóch pól tekstowych — jeden dla nazwy użytkownika, jeden dla swojego hasła — i przycisk służący do przesyłania formularza. Często witryn sieci Web zawierają pole wyboru Zapamiętaj mnie, że po zaznaczeniu tej opcji cały zwrotny bilet uwierzytelniania zostanie zachowany w ramach ponownych uruchomień przeglądarki.

Dodaj dwa pola tekstowe do login. aspx i ustaw ich właściwości ID odpowiednio na nazwę użytkownika i hasło. Ustaw również właściwość TextMode hasła na hasło. Następnie Dodaj kontrolkę CheckBox, ustawiając jej Właściwość ID na Kontrolka rememberMe i jej właściwość text, aby zapamiętać mnie. Poniżej można dodać przycisk o nazwie LoginButton, którego właściwość Text ma wartość login. Na koniec Dodaj kontrolkę sieci Web etykieta i ustaw jej Właściwość ID na InvalidCredentialsMessage, jej właściwość Text na nazwę użytkownika lub hasło jest nieprawidłowe. Spróbuj ponownie., jego właściwość ForeColor na czerwoną i jej właściwość Visible na wartość false.

W tym momencie ekran powinien wyglądać podobnie do zrzutu ekranu na rysunku 9, a składnia deklaracyjnej strony powinna wyglądać następująco:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample4.aspx)]

[![Strona logowania zawiera dwa pola tekstowe, pole wyboru, przycisk i etykietę](an-overview-of-forms-authentication-vb/_static/image26.png)](an-overview-of-forms-authentication-vb/_static/image25.png)

**Ilustracja 09**: Strona logowania zawiera dwa pola tekstowe, pole wyboru, przycisk i etykietę ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image27.png))

Na koniec Utwórz procedurę obsługi zdarzeń dla zdarzenia kliknięcia LoginButton. W projektancie kliknij dwukrotnie formant Button, aby utworzyć ten program obsługi zdarzeń.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Określanie, czy podane poświadczenia są prawidłowe

Teraz należy zaimplementować zadanie 2 w programie obsługi zdarzeń kliknięcia przycisku — określenie, czy podane poświadczenia są prawidłowe. Aby to zrobić, musi to być magazyn użytkowników, który przechowuje wszystkie poświadczenia użytkowników, dzięki czemu można określić, czy podane poświadczenia są zgodne z dowolnymi znanymi poświadczeniami.

Przed ASP.NET 2,0 deweloperzy byli zobowiązani do wdrożenia zarówno własnych magazynów użytkowników, jak i pisania kodu w celu zweryfikowania podanych poświadczeń w sklepie. Większość deweloperów implementuje magazyn użytkowników w bazie danych, tworząc tabelę o nazwie Users z kolumnami, takimi jak UserName, Password, email, LastLoginDate i tak dalej. Ta tabela, następnie ma jeden rekord na konto użytkownika. Sprawdzenie, czy poświadczenia podane przez użytkownika będą wymagały wysyłania zapytań do bazy danych pod kątem zgodnej nazwy użytkownika, a następnie zagwarantowania, że hasło w bazie danych odpowiada dostarczonemu hasłu.

W przypadku ASP.NET 2,0 deweloperzy powinni używać jednego z dostawców członkostwa do zarządzania magazynem użytkowników. W tej serii samouczków będziemy korzystać z usługi SqlMembershipProvider, która używa SQL Server bazy danych dla magazynu użytkownika. W przypadku korzystania z SqlMembershipProvider musimy zaimplementować określony schemat bazy danych, który zawiera tabele, widoki i procedury składowane oczekiwane przez dostawcę. Sprawdzimy, jak zaimplementować ten schemat w samouczku *[Tworzenie schematu członkostwa w SQL Server](../membership/creating-the-membership-schema-in-sql-server-vb.md)* . Gdy dostawca członkostwa jest używany, sprawdzanie poprawności poświadczeń użytkownika jest bardzo proste jako wywołanie metody ValidateUser [klasy członkostwa](https://msdn.microsoft.com/library/system.web.security.membership.aspx) [(*username*, *Password*)](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx), która zwraca wartość logiczną wskazującą, czy poprawność kombinacji *nazwy użytkownika* i *hasła* . W tej chwili nie możemy używać metody ValidateUser klasy członkostwa, ponieważ nie zaimplementowano jeszcze magazynu użytkownika SqlMembershipProvider.

Zamiast Poświęć czas na skompilowanie własnej niestandardowej tabeli bazy danych użytkowników (która byłaby przestarzała po zaimplementowaniu SqlMembershipProvider), spróbujmy na stałe wprowadzić prawidłowe poświadczenia na stronie logowania. W procedurze obsługi zdarzeń kliknięcia LoginButton Dodaj następujący kod:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample5.vb)]

Jak widać, istnieją trzy prawidłowe konta użytkowników — Scott, Jisun i sam — i wszystkie trzy mają takie samo hasło (hasło). Kod przechodzi przez tablice użytkowników i haseł szukających prawidłowej nazwy użytkownika i hasła. Jeśli nazwa użytkownika i hasło są prawidłowe, musimy zalogować użytkownika, a następnie przekierować je do odpowiedniej strony. Jeśli poświadczenia są nieprawidłowe, zostanie wyświetlona etykieta InvalidCredentialsMessage.

Po wprowadzeniu prawidłowych poświadczeń przez użytkownika należy wskazać, że są one następnie przekierowywane do odpowiedniej strony. Co to jest odpowiednia strona? Odwołaj ten element, gdy użytkownik odwiedzi stronę, której nie ma uprawnień do wyświetlania, FormsAuthenticationModule automatycznie przekierowuje je na stronę logowania. W takim przypadku zawiera żądany adres URL w ciągu QueryString za pośrednictwem parametru ReturnUrl. Oznacza to, że jeśli użytkownik podjął próbę odwiedzenia ProtectedPage. aspx i nie zezwolił na to, FormsAuthenticationModule przekieruje je do:

Login. aspx? ReturnUrl = ProtectedPage. aspx

Po pomyślnym zalogowaniu użytkownik powinien zostać przekierowany z powrotem do ProtectedPage. aspx. Alternatywnie użytkownicy mogą odwiedzić stronę logowania na własnym Volition. W takim przypadku po zalogowaniu się użytkownika należy je wysłać do domyślnej strony. aspx folderu głównego.

### <a name="logging-in-the-user"></a>Logowanie użytkownika

Przy założeniu, że podane poświadczenia są prawidłowe, musimy utworzyć bilet uwierzytelniania formularzy, co spowoduje Logowanie użytkownika do lokacji. [Klasa FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) w [przestrzeni nazw System. Web. Security](https://msdn.microsoft.com/library/system.web.security.aspx) zawiera różne metody logowania i wylogowywania użytkowników za pośrednictwem systemu uwierzytelniania formularzy. Chociaż istnieje kilka metod klasy FormsAuthentication, trzy interesy są następujące:

- [GetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) — tworzy bilet uwierzytelniania formularzy dla podanej nazwy *użytkownika*. Następnie ta metoda tworzy i zwraca obiekt HttpCookie, który przechowuje zawartość biletu uwierzytelniania. Jeśli *persistCookie* ma wartość true, tworzony jest trwały plik cookie.
- [SetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) — wywołuje metodę GetAuthCookie (*username*, *persistCookie*) do generowania pliku cookie uwierzytelniania formularzy. Następnie ta metoda dodaje plik cookie zwracany przez GetAuthCookie do kolekcji plików cookie (założono, że używane jest uwierzytelnianie formularzy oparte na plikach cookie; w przeciwnym razie ta metoda wywołuje wewnętrzną klasę, która obsługuje logikę biletu bez plików cookie).
- [RedirectFromLoginPage (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) — ta metoda wywołuje SetAuthCookie (*username*, *persistCookie*), a następnie przekierowuje użytkownika do odpowiedniej strony.

GetAuthCookie jest przydatny, gdy trzeba zmodyfikować bilet uwierzytelniania przed zapisaniem pliku cookie w kolekcji plików cookie. SetAuthCookie jest przydatne, jeśli chcesz utworzyć bilet uwierzytelniania formularzy i dodać go do kolekcji plików cookie, ale nie chcesz przekierować użytkownika do odpowiedniej strony. Być może chcesz je zachować na stronie logowania lub wysłać do pewnej strony alternatywnej.

Ponieważ chcemy zalogować użytkownika i przekierować je do odpowiedniej strony, użyjmy RedirectFromLoginPage. Zaktualizuj procedurę obsługi zdarzeń kliknięcia LoginButton, zastępując dwie Komentarze wierszy do wykonania następującym wierszem kodu:

FormsAuthentication. RedirectFromLoginPage (UserName. text, kontrolka rememberMe. Checked)

Podczas tworzenia biletu uwierzytelniania formularzy używamy właściwości text pola tekstowego UserName dla parametru *Nazwa użytkownika* biletu uwierzytelniania formularzy i stan zaznaczenia pola wyboru kontrolka rememberMe dla parametru *persistCookie* .

Aby przetestować stronę logowania, odwiedź ją w przeglądarce. Zacznij od wprowadzenia nieprawidłowych poświadczeń, takich jak nazwa użytkownika zgadza i nieprawidłowe hasło. Po kliknięciu przycisku Zaloguj zostanie wystawiony ogłaszanie zwrotne i zostanie wyświetlona etykieta InvalidCredentialsMessage.

[![etykieta InvalidCredentialsMessage jest wyświetlana po wprowadzeniu nieprawidłowych poświadczeń](an-overview-of-forms-authentication-vb/_static/image29.png)](an-overview-of-forms-authentication-vb/_static/image28.png)

**Rysunek 10**: etykieta InvalidCredentialsMessage jest wyświetlana po wprowadzeniu nieprawidłowych poświadczeń ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image30.png))

Następnie wprowadź prawidłowe poświadczenia i kliknij przycisk Zaloguj się. Tym razem, gdy następuje ogłaszanie zwrotne, zostanie utworzony bilet uwierzytelniania formularzy i nastąpi automatyczne przekierowanie do default. aspx. W tym momencie użytkownik zalogował się w witrynie sieci Web, chociaż nie ma żadnych wskazówek wizualnych wskazujących, że użytkownik jest aktualnie zalogowany. W kroku 4 zobaczymy, jak programowo określić, czy użytkownik jest zalogowany, jak również jak zidentyfikować użytkownika odwiedzającego stronę.

Krok 5 analizuje techniki rejestrowania użytkownika poza witryną sieci Web.

### <a name="securing-the-login-page"></a>Zabezpieczanie strony logowania

Gdy użytkownik wprowadzi swoje poświadczenia i przesyła formularz strony logowania, poświadczenia — łącznie z jego hasłem — są przesyłane przez Internet do serwera sieci Web w *postaci zwykłego tekstu*. Oznacza to, że każdy haker będący w trakcie wykrywania ruchu sieciowego zobaczy nazwę użytkownika i hasło. Aby tego uniknąć, należy zaszyfrować ruch sieciowy przy użyciu [protokołu SSL (Secure Sockets)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Dzięki temu poświadczenia (a także znacznik HTML całej strony) są szyfrowane od momentu opuszczenia przeglądarki do momentu odebrania ich przez serwer sieci Web.

Jeśli witryna sieci Web nie zawiera poufnych informacji, wystarczy użyć protokołu SSL na stronie logowania i na innych stronach, na których hasło użytkownika będzie wysyłane przez sieć w postaci zwykłego tekstu. Nie musisz martwić się o zabezpieczenie biletu uwierzytelniania formularzy, ponieważ domyślnie jest on szyfrowany i podpisany cyfrowo (aby zapobiec manipulowaniu). Dokładniejsze Omówienie zabezpieczeń biletów uwierzytelniania formularzy przedstawiono w poniższym samouczku.

> [!NOTE]
> Wiele witryn sieci Web finansowych i medycznych jest skonfigurowanych do używania protokołu SSL na *wszystkich* stronach dostępnych dla uwierzytelnionych użytkowników. Jeśli tworzysz taką witrynę sieci Web, możesz skonfigurować system uwierzytelniania formularzy tak, aby bilet uwierzytelniania formularzy był przesyłany tylko przez bezpieczne połączenie. W następnym samouczku zostaną omówione różne opcje konfiguracji uwierzytelniania formularzy, *[konfiguracja uwierzytelniania formularzy i Tematy zaawansowane](../membership/creating-the-membership-schema-in-sql-server-vb.md)* .

## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Krok 4. wykrywanie użytkowników uwierzytelnionych i określanie ich tożsamości

W tym momencie włączyłem funkcję uwierzytelniania formularzy i utworzono stronę logowania podstawowe, ale jeszcze nie udało nam się sprawdzić, czy użytkownik jest uwierzytelniany lub anonimowy. W niektórych scenariuszach firma Microsoft może chcieć wyświetlić różne dane lub informacje w zależności od tego, czy strona zostanie odwiedzana przez uwierzytelnionego, czy anonimowego użytkownika. Ponadto często musi znać tożsamość uwierzytelnionego użytkownika.

Uzupełnimy istniejącą stronę Default. aspx, aby zilustrować te techniki. W obszarze default. aspx Dodaj dwa kontrolki panelu, jeden o nazwie AuthenticatedMessagePanel i inny o nazwie AnonymousMessagePanel. Dodaj kontrolkę etykieta o nazwie WelcomeBackMessage na pierwszym panelu. W drugim panelu Dodaj kontrolkę hiperłącze, ustaw jej właściwość Text na logowanie i właściwość NavigateUrl do ~/login.aspx. W tym momencie znaczniki deklaratywne dla default. aspx powinny wyglądać podobnie do następujących:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample6.aspx)]

Ponieważ teraz prawdopodobnie odwołujesz się do Ciebie, pomysłem jest wyświetlenie tylko AuthenticatedMessagePanel do uwierzytelnionych osób odwiedzających, a tylko do AnonymousMessagePanel anonimowych odwiedzających. Aby to osiągnąć, należy ustawić widoczne właściwości tych paneli w zależności od tego, czy użytkownik jest zalogowany.

[Właściwość Request. IsAuthenticated](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) zwraca wartość logiczną wskazującą, czy żądanie zostało uwierzytelnione. Wprowadź następujący kod do strony,\_kod procedury obsługi zdarzeń ładowania:

[!code-vb[Main](an-overview-of-forms-authentication-vb/samples/sample7.vb)]

Korzystając z tego kodu, odwiedź stronę Default. aspx za pomocą przeglądarki. Przy założeniu, że użytkownik jest jeszcze zalogowany, zostanie wyświetlony link do strony logowania (patrz rysunek 11). Kliknij ten link, aby zalogować się do witryny. Zgodnie z opisem w kroku 3 po wprowadzeniu poświadczeń nastąpi powrót do default. aspx, ale ten czas na stronie wyświetla Witamy! komunikat (patrz rysunek 12).

[![po anonimowym odwiedzeniu zostanie wyświetlony link logowanie.](an-overview-of-forms-authentication-vb/_static/image32.png)](an-overview-of-forms-authentication-vb/_static/image31.png)

**Ilustracja 11**. po anonimowym odwiedzeniu zostanie wyświetlone łącze Zaloguj ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image33.png))

[![uwierzytelnieni użytkownicy są wyświetlani z powrotem Witamy! Komunikat](an-overview-of-forms-authentication-vb/_static/image35.png)](an-overview-of-forms-authentication-vb/_static/image34.png)

**Ilustracja 12**. Użytkownicy uwierzytelnieni są wyświetlani z powrotem na Zapraszamy! Komunikat ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image36.png))

Możemy określić tożsamość zalogowanego użytkownika za pomocą [właściwości użytkownika](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) [obiektu HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx). Obiekt HttpContext reprezentuje informacje o bieżącym żądaniu i jest domem dla takich typowych obiektów ASP.NET jako odpowiedzi, żądania i sesji, między innymi. Właściwość User reprezentuje kontekst zabezpieczeń bieżącego żądania HTTP i implementuje [Interfejs IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

Właściwość User jest ustawiana przez FormsAuthenticationModule. W przypadku gdy FormsAuthenticationModule odnajdzie bilet uwierzytelniania formularzy w żądaniu przychodzącym, tworzy nowy obiekt GenericPrincipal — i przypisuje go do właściwości użytkownika.

Obiekty główne (na przykład GenericPrincipal —) zawierają informacje o tożsamości użytkownika i rolach, do których należą. Interfejs IPrincipal definiuje dwa elementy członkowskie:

- [IsInRole (*rolename*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) — Metoda zwracająca wartość logiczną wskazującą, czy podmiot zabezpieczeń należy do określonej roli.
- [Identity](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) -Właściwość zwracająca obiekt, który implementuje [interfejs IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). Interfejs IIdentity definiuje trzy właściwości: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx)i [name](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Możemy określić nazwę bieżącego gościa przy użyciu następującego kodu:

Dim currentUsersName jako ciąg = User.Identity.Name

W przypadku korzystania z uwierzytelniania formularzy [obiekt FormsIdentity](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) jest tworzony dla właściwości Identity GenericPrincipal —. Klasa FormsIdentity zawsze zwraca formy ciągów dla swojej właściwości AuthenticationType i wartość true dla właściwości IsAuthenticated. Właściwość Name zwraca nazwę użytkownika określoną podczas tworzenia biletu uwierzytelniania formularzy. Oprócz tych trzech właściwości FormsIdentity obejmuje dostęp do podstawowego biletu uwierzytelniania za pośrednictwem jego [Właściwości biletu](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). Właściwość biletu zwraca obiekt typu [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), który ma właściwości, takie jak wygaśnięcie, IsPersistent, IssueDate, Name i tak dalej.

Ważną kwestią jest to, że parametr *username* określony w FormsAuthentication. GetAuthCookie (*username*, *PersistCookie*), FormsAuthentication. SetAuthCookie (*username*, *persistCookie*) i FormsAuthentication. RedirectFromLoginPage (*username*, *persistCookie*) Metoda jest taka sama jak wartość zwracana przez User.Identity.Name. Ponadto bilet uwierzytelniania utworzony przez te metody jest dostępny przez rzutowanie User. Identity na obiekt FormsIdentity, a następnie uzyskanie dostępu do właściwości biletu:

Dim ident jako FormsIdentity = CType (User. Identity, FormsIdentity)

Dim authTicket jako FormsAuthenticationTicket = ident. Równ

Przekażmy bardziej spersonalizowany komunikat w default. aspx. Zaktualizuj stronę\_obsługi zdarzeń ładowania, tak że właściwość text etykiety WelcomeBackMessage ma przypisany ciąg powitalny, *username*!

WelcomeBackMessage. text = "Witamy ponownie" &amp; User.Identity.Name &amp; "!"

Rysunek 13 przedstawia efekt modyfikacji (podczas logowania użytkownika Scott).

[![Komunikat powitalny zawiera nazwę aktualnie zalogowanego użytkownika](an-overview-of-forms-authentication-vb/_static/image38.png)](an-overview-of-forms-authentication-vb/_static/image37.png)

**Ilustracja 13**. Komunikat powitalny zawiera nazwę aktualnie zalogowanego użytkownika (kliknij,[Aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image39.png))

### <a name="using-the-loginview-and-loginname-controls"></a>Korzystanie z kontrolek widoku logowania i LoginName

Wyświetlanie innej zawartości do uwierzytelnionych i anonimowych użytkowników jest powszechnym wymaganiem; Wyświetla nazwę aktualnie zalogowanego użytkownika. Z tego powodu ASP.NET obejmuje dwie kontrolki sieci Web, które udostępniają te same funkcje, które przedstawiono na rysunku 13, ale bez konieczności pisania jednego wiersza kodu.

[Formant widoku logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) to formant sieci Web oparty na szablonach, który ułatwia wyświetlanie różnych danych do uwierzytelnionych i anonimowych użytkowników. Widoku logowania zawiera dwa wstępnie zdefiniowane szablony:

- AnonymousTemplate — wszystkie znaczniki dodane do tego szablonu są wyświetlane tylko dla anonimowych odwiedzających.
- LoggedInTemplate — znaczniki tego szablonu są wyświetlane tylko dla uwierzytelnionych użytkowników.

Dodajmy formant widoku logowania do strony głównej witryny, site. Master. Zamiast dodawać tylko formant widoku logowania, należy dodać nową kontrolkę ContentPlaceHolder, a następnie umieścić kontrolkę widoku logowania w tym nowym elemencie ContentPlaceHolder. Uzasadnienie dla tej decyzji będzie widoczne wkrótce.

> [!NOTE]
> Oprócz AnonymousTemplate i LoggedInTemplate, formant widoku logowania może zawierać szablony specyficzne dla ról. Szablony specyficzne dla ról pokazują adiustację tylko dla tych użytkowników, którzy należą do określonej roli. W przyszłym samouczku sprawdzimy funkcje oparte na rolach kontrolki widoku logowania.

Zacznij od dodania elementu ContentPlaceHolder o nazwie LoginContent na stronę wzorcową w obszarze nawigacji &lt;bloku DIV&gt; elemencie. Możesz po prostu przeciągnąć formant ContentPlaceHolder z przybornika na widok źródła, umieszczając w nim znaczniki bezpośrednio nad czynnością: menu będzie wyglądało w tym miejscu.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample8.aspx)]

Następnie Dodaj kontrolkę widoku logowania w elemencie LoginContent ContentPlaceHolder. Zawartość umieszczona w formantach ContentPlaceHolder na stronie wzorcowej jest traktowana jako *zawartość domyślna* dla elementu ContentPlaceHolder. Oznacza to, że strony ASP.NET, które używają tej strony wzorcowej, mogą określić własną zawartość dla każdego elementu ContentPlaceHolder lub użyć domyślnej zawartości strony wzorcowej.

Widoku logowania i inne kontrolki związane z logowaniem znajdują się na karcie Logowanie do przybornika.

[![kontrolkę widoku logowania w przyborniku](an-overview-of-forms-authentication-vb/_static/image41.png)](an-overview-of-forms-authentication-vb/_static/image40.png)

**Ilustracja 14**. kontrolka widoku logowania w przyborniku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image42.png))

Następnie Dodaj dwa &lt;elementy br/&gt; bezpośrednio po kontrolce widoku logowania, ale nadal w elemencie ContentPlaceHolder. W tym momencie, Nawigacja &lt;DIV&gt; znacznik elementu powinien wyglądać następująco:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample9.aspx)]

Szablony widoku logowania można definiować z poziomu projektanta lub znaczników deklaratywnych. W projektancie programu Visual Studio rozwiń tag inteligentny widoku logowania, który wyświetla listę skonfigurowanych szablonów na liście rozwijanej. Wpisz tekst Hello, dziwny do AnonymousTemplate; następnie Dodaj kontrolkę hiperłącze i ustaw jej właściwości text i NavigateUrl, aby zalogować się i ~/login.aspx.

Po skonfigurowaniu AnonymousTemplate przejdź do LoggedInTemplate, a następnie wprowadź tekst "Witaj ponownie". Następnie przeciągnij formant LoginName z przybornika do LoggedInTemplate, umieszczając go bezpośrednio po tekście "Witaj wstecz". [Kontrolka LoginName](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), jak jej nazwa oznacza, zawiera nazwę aktualnie zalogowanego użytkownika. Wewnętrznie formant LoginName po prostu wyprowadza Właściwość User.Identity.Name

Po wprowadzeniu tych dodatków do szablonów widoku logowania, znacznik powinien wyglądać podobnie do poniższego:

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample10.aspx)]

Dodanie tej opcji do strony wzorcowej site. Master spowoduje wyświetlenie innego komunikatu w zależności od tego, czy użytkownik jest uwierzytelniony. Ilustracja 15 pokazuje domyślną stronę. aspx, gdy jest ona odwiedzana przez użytkownika Jisun. Powitalny, Jisun komunikat jest powtórzony dwa razy: raz w sekcji nawigacyjnej strony wzorcowej po lewej stronie (za pośrednictwem dodanej kontrolki widoku logowania) i jeden raz w obszarze zawartości default. aspx (za pośrednictwem kontrolek panel i logika programowa).

[![formant widoku logowania wyświetla powitalny, Jisun.](an-overview-of-forms-authentication-vb/_static/image44.png)](an-overview-of-forms-authentication-vb/_static/image43.png)

**Ilustracja 15**. kontrolka widoku logowania wyświetla powitalny, Jisun. ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image45.png))

Ponieważ dodaliśmy widoku logowania do strony wzorcowej, może ona pojawić się na każdej stronie w naszej witrynie. Mogą jednak znajdować się na stronach sieci Web, w których nie ma być pokazywany ten komunikat. Jedną z takich stron jest strona logowania, ponieważ w tym miejscu pojawił się link do strony logowania. Ponieważ na stronie wzorcowej została umieszczona kontrolka widoku logowania, można zastąpić to ustawienie domyślne na naszej stronie zawartości. Otwórz login. aspx i przejdź do projektanta. Ponieważ nie zdefiniowano jawnie kontrolki zawartości w Login. aspx dla LoginContent ContentPlaceHolder na stronie wzorca, na stronie logowania zostanie wyświetlona domyślna Adiustacja strony głównej dla tego elementu ContentPlaceHolder. Można to sprawdzić za pomocą projektanta — LoginContent ContentPlaceHolder pokazuje znaczników domyślnych (kontrolki widoku logowania).

[![Strona logowania zawiera domyślną zawartość dla LoginContent ContentPlaceHolder strony głównej](an-overview-of-forms-authentication-vb/_static/image47.png)](an-overview-of-forms-authentication-vb/_static/image46.png)

**Ilustracja 16**. Strona logowania zawiera domyślną zawartość dla LoginContent ContentPlaceHolder strony wzorcowej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image48.png))

Aby zastąpić domyślne znaczniki dla LoginContent ContentPlaceHolder, po prostu kliknij prawym przyciskiem myszy region w Projektancie i wybierz opcję Utwórz niestandardową zawartość z menu kontekstowego. (W przypadku korzystania z programu Visual Studio 2008 element ContentPlaceHolder zawiera tag inteligentny, który w przypadku wybrania oferuje tę samą opcję). Spowoduje to dodanie nowej kontrolki zawartości do znacznika strony, co umożliwi nam zdefiniowanie zawartości niestandardowej na tej stronie. W tym miejscu możesz dodać niestandardowy komunikat, taki jak Zaloguj się, ale po prostu pozostaw to pole puste.

> [!NOTE]
> W programie Visual Studio 2005 Tworzenie zawartości niestandardowej tworzy pustą kontrolkę zawartości na stronie ASP.NET. Jednak w programie Visual Studio 2008 Tworzenie niestandardowej zawartości powoduje skopiowanie domyślnej zawartości strony wzorcowej do nowo utworzonej kontrolki zawartości. Jeśli używasz programu Visual Studio 2008, po utworzeniu nowej kontrolki zawartości upewnij się, że zawartość skopiowana z strony wzorcowej jest wyczyszczona.

Rysunek 17 przedstawia stronę login. aspx wyświetlaną w przeglądarce po wprowadzeniu tej zmiany. Zwróć uwagę, że nie ma żadnych powitania, nieznajomego ani powitalnego, komunikatu *username* w lewym obszarze nawigacji &lt;DIV&gt;, ponieważ podczas odwiedzania domyślnego. aspx.

[![Strona logowania ukrywa domyślną adiustację LoginContent elementu ContentPlaceHolder](an-overview-of-forms-authentication-vb/_static/image50.png)](an-overview-of-forms-authentication-vb/_static/image49.png)

**Ilustracja 17**. Strona logowania ukrywa domyślne oznaczenie elementu LoginContent ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image51.png))

## <a name="step-5-logging-out"></a>Krok 5. Wylogowywanie

W kroku 3 zawarto informacje na temat tworzenia strony logowania w celu zarejestrowania użytkownika w witrynie, ale jeszcze zapoznaj się z tematem jak wylogować użytkownika. Oprócz metod rejestrowania użytkownika w programie Klasa FormsAuthentication również udostępnia [metodę wylogowaniu](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). Metoda wylogowaniu po prostu niszczy bilet uwierzytelniania formularzy, a tym samym rejestruje użytkownika poza lokacją.

Oferowanie linku do wylogowania to typowa funkcja, która ASP.NET zawiera kontrolkę zaprojektowaną specjalnie do rejestrowania użytkownika. [Kontrolka stanu logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) Wyświetla identyfikator logowania element LinkButton lub element LinkButton, w zależności od stanu uwierzytelniania użytkownika. Element LinkButton logowania jest renderowany dla użytkowników anonimowych, podczas gdy element LinkButton wylogowania jest wyświetlany dla uwierzytelnionych użytkowników. Tekst dla logowania i wylogowania LinkButtons można skonfigurować za pomocą właściwości LoginText i LogoutText stanu logowania.

Kliknięcie element LinkButton logowania powoduje odświeżenie, z którego zostanie wysłane przekierowanie na stronę logowania. Kliknięcie element LinkButton wylogowania powoduje wywołanie przez formant stanu logowania metody FormsAuthentication. przygotowania, a następnie przekierowanie użytkownika do strony. Strona, do której zalogowany jest użytkownik, jest zależna od właściwości LogoutAction, która może być przypisana do jednej z trzech następujących wartości:

- Odśwież — wartość domyślna; przekierowuje użytkownika do właśnie odwiedzanej strony. Jeśli właśnie odwiedzana strona nie zezwala na użytkowników anonimowych, FormsAuthenticationModule automatycznie przekieruje użytkownika na stronę logowania.

W tym miejscu może być chcesz wiedzieć, dlaczego przekierowanie zostało wykonane. Jeśli użytkownik chce pozostawać na tej samej stronie, dlaczego jest to potrzebne w przypadku jawnego przekierowania? Przyczyną jest to, że po kliknięciu element LinkButton wylogowania użytkownik nadal ma bilet uwierzytelniania formularzy w kolekcji plików cookie. W związku z tym żądanie ogłaszania zwrotnego jest żądaniem uwierzytelnionym. Kontrolka stanu logowania wywołuje metodę wylogowaniu, która jest wykonywana po uwierzytelnieniu użytkownika przez FormsAuthenticationModule. W związku z tym jawne przekierowanie powoduje ponowne zażądanie strony przez przeglądarkę. Gdy przeglądarka żąda ponownego żądania strony, bilet uwierzytelniania formularzy został usunięty i w związku z tym żądanie przychodzące jest anonimowe.

- Przekierowanie — użytkownik jest przekierowywany do adresu URL określonego przez właściwość LogoutPageUrl stanu logowania.
- RedirectToLoginPage — użytkownik zostanie przekierowany do strony logowania.

Dodajmy kontrolkę stanu logowania do strony wzorcowej i skonfigurujesz ją do korzystania z opcji przekierowania, aby wysłać użytkownikowi komunikat z potwierdzeniem, że zostały wylogowane. Zacznij od utworzenia strony w katalogu głównym o nazwie Wyloguj. aspx. Nie zapomnij, aby skojarzyć Tę stronę z lokacją. Master Master. Następnie wprowadź komunikat na znaczniku strony, wyjaśniając użytkownikowi, że został wylogowany.

Następnie wróć do strony wzorcowej site. Master i Dodaj kontrolkę stanu logowania pod widoku logowaniaą w LoginContent ContentPlaceHolder. Ustaw właściwość LogoutAction kontrolki stanu logowania na Redirect, a jej Właściwość LogoutPageUrl na wartość ~/logout.aspx.

[!code-aspx[Main](an-overview-of-forms-authentication-vb/samples/sample11.aspx)]

Ponieważ stanu logowania znajduje się poza kontrolką widoku logowania, zostanie ona wyświetlona zarówno dla użytkowników anonimowych, jak i uwierzytelnionych, ale jest to prawidłowe, ponieważ stanu logowania będzie poprawnie wyświetlać identyfikator logowania lub element LinkButton. Po dodaniu kontrolki stanu logowania, plik HyperLink in w AnonymousTemplate jest zbędny, dlatego należy go usunąć.

Na rysunku 18 są wyświetlane wartości Default. aspx podczas odwiedzin Jisun. Zwróć uwagę, że w lewej kolumnie jest wyświetlany komunikat, Witamy w Jisun oraz link do wylogowania. Kliknięcie element LinkButton wylogowania powoduje odświeżenie, podpisanie Jisun z systemu, a następnie przekierowanie do wylogowania. aspx. Jak pokazano na rysunku 19, Jisun osiągnie wylogowanie. aspx został już wylogowany i dlatego jest anonimowy. W związku z tym lewa kolumna zawiera tekst Welcome, dziwneer i link do strony logowania.

[![default. aspx przedstawia powitalny, Jisun wraz z wylogowaniem element LinkButton](an-overview-of-forms-authentication-vb/_static/image53.png)](an-overview-of-forms-authentication-vb/_static/image52.png)

**Ilustracja 18**. default. aspx pokazuje powitalny, Jisun wraz z wylogowaniem element LinkButton ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image54.png))

[![Wyloguj. aspx zawiera Welcome, dziwne i element LinkButton logowania](an-overview-of-forms-authentication-vb/_static/image56.png)](an-overview-of-forms-authentication-vb/_static/image55.png)

**Ilustracja 19**. wylogowanie. aspx zawiera Welcome, dziwne i element LinkButton logowania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-vb/_static/image57.png))

> [!NOTE]
> Zachęcam do dostosowania strony Wyloguj. aspx, aby ukryć LoginContent ContentPlaceHolder strony wzorcowej (podobnie jak w przypadku logowania. aspx w kroku 4). Przyczyną jest to, że element linkbuttona logowania renderowana przez formant stanu logowania (jedna pod Hello, dziwnego) wysyła użytkownika do strony logowania przekazującej bieżący adres URL w parametrze QueryString ReturnUrl. W krótkim czasie, jeśli użytkownik, który wyloguje się, kliknie stanu logowania logowania element LinkButton, a następnie zaloguje się ponownie, nastąpi przekierowanie do wylogowania. aspx, który mógłby łatwo pomylić użytkownika.

## <a name="summary"></a>Podsumowanie

W tym samouczku rozpocząłmy badanie przepływu pracy uwierzytelniania formularzy, a następnie w celu zaimplementowania uwierzytelniania formularzy w aplikacji ASP.NET. Uwierzytelnianie formularzy jest obsługiwane przez FormsAuthenticationModule, który ma dwie obowiązki: Identyfikowanie użytkowników na podstawie biletu uwierzytelniania formularzy i przekierowywanie nieautoryzowanych użytkowników na stronę logowania.

Klasa FormsAuthentication .NET Framework obejmuje metody tworzenia, inspekcji i usuwania biletów uwierzytelniania formularzy. Właściwość Request. IsAuthenticated i obiekt User zapewniają dodatkową pomoc techniczną do określenia, czy żądanie jest uwierzytelniane, oraz informacje o tożsamości użytkownika. Dostępne są również kontrolki sieci Web widoku logowania, stanu logowania i LoginName, które umożliwiają deweloperom szybkie i bezpłatne korzystanie z kodu na potrzeby wykonywania wielu typowych zadań związanych z logowaniem. Sprawdzimy te i inne kontrolki sieci Web powiązane z logowaniem bardziej szczegółowo w kolejnych samouczkach.

W tym samouczku przedstawiono Omówienie uwierzytelniania formularzy. Nie sprawdzono dostępnych opcji konfiguracji, należy zapoznać się z tematem działania biletów uwierzytelniania formularzy bez plików cookie lub poznać sposób, w jaki ASP.NET chroni zawartość biletu uwierzytelniania formularzy. W [następnym samouczku](forms-authentication-configuration-and-advanced-topics-vb.md)omówiono te tematy i wiele więcej.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Zmiany między usług IIS 6 i zabezpieczeniami IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Kontrolki ASP.NET logowania](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Professional ASP.NET 2,0 — zabezpieczenia, członkostwo i zarządzanie rolami](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Element&gt; &lt;Authentication](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;formularzy&gt; elementu do uwierzytelniania &lt;&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Szkolenia wideo dotyczące tematów zawartych w tym samouczku

- [Korzystanie z uwierzytelniania podstawowego formularzy na platformie ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

### <a name="about-the-author"></a>Informacje o autorze

Scott Mitchell, autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to *[Sams ASP.NET 2,0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott można uzyskać w [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka obejmują Alicja Maziarz, Jan suru i Teresa Murphy. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Poprzednie](security-basics-and-asp-net-support-vb.md)
> [dalej](forms-authentication-configuration-and-advanced-topics-vb.md)

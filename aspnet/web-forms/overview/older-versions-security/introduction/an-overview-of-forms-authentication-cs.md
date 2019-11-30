---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
title: Omówienie uwierzytelniania formularzy (C#) | Microsoft Docs
author: rick-anderson
description: Tworzenie tras niestandardowych
ms.author: riande
ms.date: 01/14/2008
ms.assetid: de2d65b9-aadc-42ba-abe1-4e87e66521a0
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 009c3f84e00d648ede4a15e530ceac2d23e01eec
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74620750"
---
# <a name="an-overview-of-forms-authentication-c"></a>Omówienie uwierzytelniania formularzy (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_CS.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_cs.pdf)

> W tym samouczku przejdziemy do implementacji zwykłej dyskusji. w szczególności zapoznaj się z zaimplementowaniem uwierzytelniania formularzy. Aplikacja sieci Web, która rozpoczyna konstruowanie w tym samouczku, będzie nadal skompilowana w kolejnych samouczkach, podobnie jak w przypadku przechodzenia z prostego uwierzytelniania formularzy do członkostwa i ról.
> 
> Obejrzyj ten film wideo, aby uzyskać więcej informacji na temat tego tematu: [używanie uwierzytelniania podstawowego formularzy w programie ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md).

## <a name="introduction"></a>Wprowadzenie

W [poprzednim samouczku](security-basics-and-asp-net-support-cs.md) omówiono różne opcje uwierzytelniania, autoryzacji i konta użytkownika udostępniane przez ASP.NET. W tym samouczku przejdziemy do implementacji zwykłej dyskusji. w szczególności zapoznaj się z zaimplementowaniem uwierzytelniania formularzy. Aplikacja sieci Web, która rozpoczyna konstruowanie w tym samouczku, będzie nadal skompilowana w kolejnych samouczkach, podobnie jak w przypadku przechodzenia z prostego uwierzytelniania formularzy do członkostwa i ról.

Ten samouczek rozpoczyna się od szczegółowego wyglądu przepływu pracy związanego z uwierzytelnianiem formularzy, który został rozłożony w poprzednim samouczku. Poniżej zostanie utworzona witryna sieci Web ASP.NET, za pomocą której można obejrzeć koncepcje uwierzytelniania formularzy. Następnie skonfigurujemy lokację do korzystania z uwierzytelniania formularzy, tworzenia prostej strony logowania i Dowiedz się, jak określić, w kodzie, czy użytkownik jest uwierzytelniany, a jeśli tak, to nazwa użytkownika, na którym się zalogowano.

Zrozumienie przepływu pracy uwierzytelniania formularzy, włączenie go w aplikacji sieci Web, a następnie utworzenie stron logowania i wylogowania to wszystkie istotne kroki w tworzeniu aplikacji ASP.NET, która obsługuje konta użytkowników i uwierzytelnia użytkowników za pomocą strony sieci Web. Ze względu na to, że te samouczki kompilują się po sobie nawzajem — zachęcamy do pracy z tym samouczkiem w całości przed przejściem do kolejnego, nawet jeśli masz już doświadczenie w konfigurowaniu uwierzytelniania formularzy w przeszłych projektach.

## <a name="understanding-the-forms-authentication-workflow"></a>Informacje o przepływie pracy uwierzytelniania formularzy

Gdy środowisko uruchomieniowe ASP.NET przetwarza żądanie dla zasobu ASP.NET, takie jak strona ASP.NET lub usługa sieci Web ASP.NET, żądanie zgłasza wiele zdarzeń w cyklu życia. Istnieją zdarzenia zgłoszone na bardzo początku i na końcu żądania, wywoływane, gdy żądanie jest uwierzytelniane i autoryzowane, zdarzenie zgłoszone w przypadku nieobsłużonego wyjątku i tak dalej. Aby wyświetlić pełną listę zdarzeń, zapoznaj się ze [zdarzeniami obiektu elemencie HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*Moduły HTTP* są klasami zarządzanymi, których kod jest wykonywany w odpowiedzi na konkretne zdarzenie w cyklu życia żądania. ASP.NET dostarcza wiele modułów HTTP, które wykonują podstawowe zadania w tle. Dwa wbudowane moduły HTTP, które są szczególnie przydatne w naszej dyskusji:

- **[`FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)** — uwierzytelnia użytkownika, sprawdzając bilet uwierzytelniania formularzy, który zwykle jest zawarty w kolekcji plików cookie użytkownika. Jeśli nie istnieje bilet uwierzytelniania formularzy, użytkownik jest anonimowy.
- **[`UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)** — określa, czy bieżący użytkownik ma uprawnienia dostępu do żądanego adresu URL. Ten moduł określa Urząd zgodnie z regułami autoryzacji określonymi w plikach konfiguracji aplikacji. ASP.NET zawiera również [`FileAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) , które określają urząd, sprawdzając wymagane pliki list ACL.

`FormsAuthenticationModule` próbuje uwierzytelnić użytkownika przed wykonaniem `UrlAuthorizationModule` (i `FileAuthorizationModule`). Jeśli użytkownik wykonujący żądanie nie ma uprawnień dostępu do żądanego zasobu, moduł autoryzacji zakończy żądanie i zwróci stan [nieautoryzowany HTTP 401](http://www.checkupdown.com/status/E401.html) . W scenariuszach uwierzytelniania systemu Windows stan HTTP 401 jest zwracany do przeglądarki. Ten kod stanu powoduje, że przeglądarka monituje użytkownika o podanie poświadczeń za pośrednictwem modalnego okna dialogowego. W przypadku uwierzytelniania formularzy nieautoryzowany stan protokołu HTTP 401 nie jest nigdy wysyłany do przeglądarki, ponieważ FormsAuthenticationModule wykrywa ten stan i modyfikuje go, aby przekierować użytkownika do strony logowania (za pośrednictwem stanu [przekierowywania HTTP 302](http://www.checkupdown.com/status/E302.html) ).

Odpowiedzialność za stronę logowania jest określenie, czy poświadczenia użytkownika są prawidłowe, a jeśli tak, to w celu utworzenia biletu uwierzytelniania formularzy i przekierowania użytkownika z powrotem do strony, którą próbowano odwiedzać. Bilet uwierzytelniania jest uwzględniany w kolejnych żądaniach do stron w witrynie sieci Web, których `FormsAuthenticationModule` używa do identyfikowania użytkownika.

![Przepływ pracy uwierzytelniania formularzy](an-overview-of-forms-authentication-cs/_static/image1.png)

**Rysunek 1**. przepływ pracy uwierzytelniania formularzy

### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Zapamiętywanie biletu uwierzytelniania między wizytami stron

Po zalogowaniu biletu uwierzytelniania formularzy należy wysyłać z powrotem do serwera sieci Web w każdym żądaniu, tak aby użytkownik pozostawać zalogowany podczas przeglądania witryny. Jest to zwykle realizowane przez umieszczenie biletu uwierzytelniania w kolekcji plików cookie użytkownika. Pliki [cookie](http://en.wikipedia.org/wiki/HTTP_cookie) to małe pliki tekstowe, które znajdują się na komputerze użytkownika i są przesyłane w nagłówkach HTTP na każde żądanie do witryny sieci Web, która utworzyła plik cookie. W związku z tym po utworzeniu biletu uwierzytelniania formularzy i zapisaniu go w plikach cookie w przeglądarce każda kolejna wizyta w tej witrynie wysyła bilet uwierzytelniania wraz z żądaniem, co oznacza, że użytkownik identyfikuje użytkownika.

Jednym z aspektów plików cookie jest ich wygaśnięcie, czyli Data i godzina odrzucenia pliku cookie przez przeglądarkę. Gdy plik cookie uwierzytelniania formularzy wygaśnie, użytkownik nie może być już uwierzytelniony i w związku z tym staje się anonimowy. Gdy użytkownik odwiedzający się z terminalu publicznego, może mieć możliwość wygaśnięcia biletu uwierzytelniania po zamknięciu przeglądarki. Podczas odwiedzania z domu jednak ten sam użytkownik może chcieć, aby bilet uwierzytelnienia został zapamiętany przez ponowne uruchomienie przeglądarki, aby nie musiały ponownie rejestrować się za każdym razem, gdy odwiedzają witrynę. Ta decyzja jest często podejmowana przez użytkownika w postaci pola wyboru "Zapamiętaj mnie" na stronie logowania. W kroku 3 sprawdzimy, jak zaimplementować pole wyboru "Zapamiętaj mnie" na stronie logowania. Poniższy samouczek dotyczy szczegółowych ustawień limitu czasu biletu uwierzytelniania.

> [!NOTE]
> Istnieje możliwość, że agent użytkownika używany do logowania się w witrynie sieci Web może nie obsługiwać plików cookie. W takim przypadku ASP.NET może używać biletów uwierzytelniania formularzy bez plików cookie. W tym trybie bilet uwierzytelniania jest zakodowany w adresie URL. Po użyciu biletów uwierzytelniania bez plików cookie i sposobu ich tworzenia i zarządzania w następnym samouczku zostanie wyświetlona wartość.

### <a name="the-scope-of-forms-authentication"></a>Zakres uwierzytelniania formularzy

`FormsAuthenticationModule` jest kodem zarządzanym, który jest częścią środowiska uruchomieniowego ASP.NET. W wersji 7 serwera sieci Web [Internet Information Services (IIS)](https://www.iis.net/) firmy Microsoft istnieje odrębna bariera między POTOKIEM http usługi IIS a potokiem środowiska uruchomieniowego ASP.NET. W skrócie w usługach IIS w wersji 6 i starszych `FormsAuthenticationModule` wykonywane tylko wtedy, gdy żądanie jest delegowane z usług IIS do środowiska uruchomieniowego ASP.NET. Domyślnie usługi IIS przetwarzają zawartość statyczną, na przykład strony HTML i pliki CSS i obrazy, a także wyłączają żądania do środowiska uruchomieniowego ASP.NET po zażądaniu strony z rozszerzeniem. aspx,. asmx lub. ashx.

Usługi IIS 7 umożliwiają jednak zintegrowane usługi IIS i potoki ASP.NET. Za pomocą kilku ustawień konfiguracji można skonfigurować usługi IIS 7 do wywoływania FormsAuthenticationModule dla *wszystkich* żądań. Ponadto w przypadku usług IIS 7 można definiować reguły autoryzacji adresów URL dla plików dowolnego typu. Aby uzyskać więcej informacji, zobacz [zmiany między zabezpieczeniami usług IIS 6 i IIS7](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [zabezpieczenia platformy sieci Web](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements)i [zrozumienie autoryzacji adresów URL IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Długie scenariusze krótko, w wersjach wcześniejszych niż IIS 7, można używać uwierzytelniania formularzy do ochrony zasobów obsłużonych przez środowisko uruchomieniowe ASP.NET. Podobnie reguły autoryzacji adresów URL są stosowane tylko do zasobów obsłużonych przez środowisko uruchomieniowe ASP.NET. Jednak w przypadku usług IIS 7 można zintegrować FormsAuthenticationModule i UrlAuthorizationModule z potokiem HTTP usług IIS, rozszerzając tę funkcjonalność na wszystkie żądania.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Krok 1. Tworzenie witryny sieci Web ASP.NET dla tej serii samouczków

Aby dotrzeć do szerokiego grona odbiorców, ASP.NET witrynę sieci Web, którą będziemy kompilować w tej serii, zostanie utworzona za pomocą bezpłatnej wersji programu Visual Studio 2008, [Visual webdeveloper 2008](https://www.microsoft.com/express/vwd/). Zaimplementujemy magazyn `SqlMembershipProvider` użytkowników w bazie danych [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) . Jeśli używasz programu Visual Studio 2005 lub innej wersji programu Visual Studio 2008 lub SQL Server, nie martw się — kroki będą niemal identyczne, a wszystkie nieproste różnice będą wskazywane.

> [!NOTE]
> Demonstracja aplikacja sieci Web używana w każdym samouczku jest dostępna do pobrania. Ta aplikacja do pobrania została utworzona przy użyciu programu Visual Web Developer 2008 przeznaczonego dla .NET Framework w wersji 3,5. Ponieważ aplikacja jest przeznaczona dla programu .NET 3,5, jej plik Web. config zawiera dodatkowe elementy konfiguracji specyficzne dla 3,5. Długi scenariusz krótko, jeśli na komputerze nie jest jeszcze zainstalowany program .NET 3,5, aplikacja sieci Web do pobrania nie będzie działała bez uprzedniego usunięcia oznakowania specyficznego dla 3,5 z pliku Web. config.

Zanim będziemy mogli skonfigurować uwierzytelnianie formularzy, potrzebujemy najpierw witryny sieci Web ASP.NET. Zacznij od utworzenia nowej witryny sieci Web ASP.NET opartej na systemie plików. Aby to osiągnąć, uruchom program Visual Web Developer, a następnie przejdź do menu plik i wybierz polecenie Nowa witryna sieci Web, wyświetlając okno dialogowe Nowa witryna sieci Web. Wybierz szablon witryna sieci Web ASP.NET, Ustaw listę rozwijaną lokalizacja na system plików, wybierz folder, w którym ma zostać umieszczona witryna sieci Web, i Ustaw język C#na. Spowoduje to utworzenie nowej witryny sieci Web z domyślną stroną ASP.NET. aspx, folderem danych\_i plikiem Web. config.

> [!NOTE]
> Program Visual Studio obsługuje dwa tryby zarządzania projektami: projekty witryny sieci Web i projekty aplikacji sieci Web. Projekty witryn sieci Web bez pliku projektu, natomiast projekty aplikacji sieci Web naśladuje architekturę projektu w programie Visual Studio .NET 2002/2003 — obejmują one plik projektu i kompilują kod źródłowy projektu w jeden zestaw, który znajduje się w folderze/bin. Program Visual Studio 2005 początkowo tylko obsługiwane projekty witryny sieci Web, mimo że model projektu aplikacji sieci Web został wprowadzony ponownie z dodatkiem Service Pack 1. Program Visual Studio 2008 oferuje modele projektów. Wersje Visual Web Developer 2005 i 2008, jednak obsługują tylko projekty witryn sieci Web. Będę używać modelu projektu witryny sieci Web. Jeśli używasz wersji innej niż Express i chcesz zamiast tego używać [modelu projektu aplikacji sieci Web](https://msdn.microsoft.com/library/aa730880%28vs.80%29.aspx) , możesz to zrobić, ale pamiętaj, że mogą wystąpić pewne rozbieżności między tym, co widzisz na ekranie, a także kroki, które należy wykonać, a także instrukcje zawarte w tych samouczkach.

[![utworzyć nową witrynę sieci Web opartą na systemie plików](an-overview-of-forms-authentication-cs/_static/image3.png)](an-overview-of-forms-authentication-cs/_static/image2.png)

**Rysunek 2**. Tworzenie nowej witryny sieci Web opartej na systemie plików ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image4.png))

### <a name="adding-a-master-page"></a>Dodawanie strony wzorcowej

Następnie Dodaj nową stronę wzorcową do lokacji w katalogu głównym o nazwie Site. Master. [Strony wzorcowe](https://msdn.microsoft.com/library/wtxbf3hh.aspx) umożliwiają deweloperom strony Definiowanie szablonu w całej lokacji, który można zastosować do stron ASP.NET. Główną zaletą stron wzorcowych jest to, że ogólny wygląd witryny można zdefiniować w jednej lokalizacji, co ułatwia aktualizowanie lub dostosowywanie układu witryny.

[![dodać strony wzorcowej o nazwie Site. Master do witryny sieci Web](an-overview-of-forms-authentication-cs/_static/image6.png)](an-overview-of-forms-authentication-cs/_static/image5.png)

**Rysunek 3**. Dodawanie strony wzorcowej o nazwie Site. Master do witryny sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image7.png))

W tym miejscu Zdefiniuj układ strony dla całej witryny na stronie wzorcowej. Możesz użyć widok Projekt i dodać wszelkie wymagane przez siebie kontrolki układu lub sieci Web lub ręcznie dodać znaczniki adiustacji do widoku źródła. Struktura układu strony wzorcowej w celu naśladowania układu używanego w *[pracy z danymi w](../../data-access/index.md)* serii samouczków ASP.NET 2,0 (patrz rysunek 4). Na stronie wzorcowej są stosowane [kaskadowe arkusze stylów](http://www.w3schools.com/css/default.asp) do pozycjonowania i stylów z ustawieniami CSS zdefiniowanymi w pliku style. CSS (który jest zawarty w tym samouczku związanym z pobieraniem). Chociaż nie możesz powiedzieć od znaczników przedstawionych poniżej, reguły CSS są zdefiniowane w taki sposób, aby Nawigacja &lt;DIV&gt;zawartość była pozycjonowana absolutnie, tak aby była wyświetlana po lewej stronie i ma stałą szerokość 200 pikseli.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample1.aspx)]

Strona wzorcowa definiuje zarówno statyczny układ strony, jak i regiony, które mogą być edytowane przez strony ASP.NET, które korzystają z strony wzorcowej. Te edytowalne regiony zawartości są wskazywane przez kontrolkę `ContentPlaceHolder`, która może być widoczna w obszarze blok zawartości &lt;&gt;. Nasza Strona główna ma jeden `ContentPlaceHolder` (kontrolka mainContent), ale strona wzorcowa może mieć wiele Elementy ContentPlaceHolders.

Po wprowadzeniu znacznika w widok Projekt zostanie wyświetlona strona wzorcowa. Wszystkie strony ASP.NET, które używają tej strony wzorcowej będą miały jednolity układ, z możliwością określenia znaczników dla regionu `MainContent`.

[![stronę wzorcową, gdy jest wyświetlany w widoku projektu](an-overview-of-forms-authentication-cs/_static/image9.png)](an-overview-of-forms-authentication-cs/_static/image8.png)

**Rysunek 4**. Strona wzorcowa wyświetlana w widoku projektu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image10.png))

### <a name="creating-content-pages"></a>Tworzenie stron zawartości

W tym momencie mamy domyślną stronę. aspx w naszej witrynie sieci Web, ale nie używa ona właśnie utworzonej strony wzorcowej. Chociaż istnieje możliwość manipulowania deklaratywnym znacznikiem strony sieci Web w celu korzystania z strony wzorcowej, jeśli strona nie zawiera jeszcze żadnej zawartości, łatwiej ją usunąć i ponownie dodać do projektu, określając stronę wzorcową do użycia. W związku z tym Zacznij od usunięcia z projektu default. aspx.

Następnie kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań i wybierz polecenie Dodaj nowy formularz sieci Web o nazwie Default. aspx. Tym razem zaznacz pole wyboru "Wybierz stronę wzorcową" i wybierz z listy stronę wzorcową lokacja. Master.

[![dodać nową stronę Default. aspx wybierającą stronę wzorcową](an-overview-of-forms-authentication-cs/_static/image12.png)](an-overview-of-forms-authentication-cs/_static/image11.png)

**Rysunek 5**. Dodawanie nowej strony Default. aspx Wybieranie strony wzorcowej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image13.png))

![Korzystanie z strony wzorcowej witryny. Master](an-overview-of-forms-authentication-cs/_static/image14.png)

**Ilustracja 6**. Korzystanie z strony wzorcowej witryny. Master

> [!NOTE]
> Jeśli używasz modelu projektu aplikacji sieci Web, okno dialogowe Dodaj nowy element nie zawiera pola wyboru "Wybierz stronę wzorcową". Zamiast tego należy dodać element typu "formularz zawartości sieci Web". Po wybraniu opcji "formularz zawartości sieci Web" i kliknięciu pozycji Dodaj program Visual Studio wyświetli to samo okno dialogowe Wybieranie wzorca pokazane na rysunku 6.

Nowa domyślna Adiustacja strony. aspx zawiera tylko dyrektywę @Page określającą ścieżkę do pliku strony głównej i kontrolkę zawartości dla elementu kontrolka mainContent.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample2.aspx)]

Na razie pozostaw puste pole default. aspx. Powrócimy do niego w dalszej części tego samouczka, aby dodać zawartość.

> [!NOTE]
> Nasza Strona główna zawiera sekcję menu lub inny interfejs nawigacji. Utworzymy taki interfejs w przyszłym samouczku.

## <a name="step-2-enabling-forms-authentication"></a>Krok 2. Włączanie uwierzytelniania formularzy

Po utworzeniu witryny sieci Web ASP.NET następnym zadaniem jest włączenie uwierzytelniania formularzy. Konfiguracja uwierzytelniania aplikacji jest określana za pomocą [elementu`<authentication>`](https://msdn.microsoft.com/library/532aee0e.aspx) w pliku Web. config. Element `<authentication>` zawiera nazwany tryb pojedynczego atrybutu, który określa model uwierzytelniania używany przez aplikację. Ten atrybut może mieć jedną z następujących czterech wartości:

- **System Windows** — zgodnie z opisem w poprzednim samouczku, gdy aplikacja korzysta z uwierzytelniania systemu Windows, jest odpowiedzialny za serwer sieci Web do uwierzytelniania gościa. zwykle jest to realizowane za pośrednictwem podstawowego, szyfrowanego lub zintegrowanego uwierzytelniania systemu Windows.
- **Formularze**— użytkownicy są uwierzytelniani za pośrednictwem formularza na stronie sieci Web.
- **Paszport**— użytkownicy są uwierzytelniani przy użyciu usługi Microsoft Passport Network.
- **Brak**— nie jest używany żaden model uwierzytelniania; Wszyscy Goście są anonimowi.

Domyślnie aplikacje ASP.NET używają uwierzytelniania systemu Windows. Aby zmienić typ uwierzytelniania na uwierzytelnianie formularzy, należy zmodyfikować atrybut trybu `<authentication>` elementu na Forms.

Jeśli projekt nie zawiera jeszcze pliku Web. config, Dodaj go teraz, klikając prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wybierając pozycję Dodaj nowy element, a następnie dodając plik konfiguracji sieci Web.

[![, jeśli projekt nie zawiera jeszcze pliku Web. config, Dodaj go teraz](an-overview-of-forms-authentication-cs/_static/image16.png)](an-overview-of-forms-authentication-cs/_static/image15.png)

**Rysunek 7**. Jeśli projekt nie zawiera jeszcze pliku Web. config, Dodaj go teraz ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image17.png))

Następnie Znajdź `<authentication>` element i zaktualizuj go, aby używać uwierzytelniania formularzy. Po tej zmianie znacznik pliku Web. config powinien wyglądać podobnie do poniższego:

[!code-xml[Main](an-overview-of-forms-authentication-cs/samples/sample3.xml)]

> [!NOTE]
> Ponieważ plik Web. config jest plikiem XML, wielkość liter jest ważna. Upewnij się, że ustawisz atrybut Mode na Forms, przy użyciu wielkiej litery "F". Jeśli używasz innej wielkości liter, na przykład "Forms", podczas odwiedzania witryny za pomocą przeglądarki zostanie wyświetlony komunikat o błędzie konfiguracji.

Element `<authentication>` może opcjonalnie zawierać `<forms>` elementu podrzędnego, który zawiera ustawienia specyficzne dla uwierzytelniania formularzy. Teraz Użyjmy domyślnych ustawień uwierzytelniania formularzy. W następnym samouczku poszukamy elementu podrzędnego `<forms>` w bardziej szczegółowy sposób.

## <a name="step-3-building-the-login-page"></a>Krok 3. Budowanie strony logowania

Aby można było obsługiwać uwierzytelnianie formularzy, witryna sieci Web wymaga strony logowania. Zgodnie z opisem w sekcji "Omówienie przepływu pracy uwierzytelniania formularzy" `FormsAuthenticationModule` automatycznie przekieruje użytkownika na stronę logowania, jeśli spróbujesz uzyskać dostęp do strony, której nie mają uprawnienia do wyświetlania. Istnieją także ASP.NET formanty sieci Web, które będą wyświetlać link do strony logowania do użytkowników anonimowych. To begs pytanie "jaki jest adres URL strony logowania?"

Domyślnie system uwierzytelniania formularzy oczekuje, że strona logowania jest nazywana login. aspx i umieszczona w katalogu głównym aplikacji sieci Web. Jeśli chcesz użyć innego adresu URL strony logowania, możesz to zrobić, określając je w pliku Web. config. Zobaczymy, jak to zrobić w następnym samouczku.

Strona logowania ma trzy obowiązki:

1. Podaj interfejs, który umożliwia odwiedzającemu wprowadzanie poświadczeń.
2. Ustal, czy przesłane poświadczenia są prawidłowe.
3. "Zaloguj się", tworząc bilet uwierzytelniania formularzy.

### <a name="creating-the-login-pages-user-interface"></a>Tworzenie interfejsu użytkownika strony logowania

Zacznijmy od pierwszego zadania. Dodaj nową stronę ASP.NET do katalogu głównego witryny o nazwie login. aspx i skojarz ją z główną stroną programu site. Master.

[![dodać nową stronę ASP.NET o nazwie login. aspx](an-overview-of-forms-authentication-cs/_static/image19.png)](an-overview-of-forms-authentication-cs/_static/image18.png)

**Ilustracja 8**. Dodawanie nowej strony ASP.NET o nazwie login. aspx ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image20.png))

Typowy interfejs strony logowania składa się z dwóch pól tekstowych — jeden dla nazwy użytkownika, jeden dla swojego hasła — i przycisk służący do przesyłania formularza. Często witryn sieci Web zawierają pole wyboru "Zapamiętaj mnie", które w przypadku zaznaczenia tej opcji spowoduje, że w ramach ponownych uruchomień przeglądarki będzie trwały bilet uwierzytelniania.

Dodaj dwa pola tekstowe do login. aspx i ustaw ich `ID` właściwości odpowiednio do nazwy użytkownika i hasła. Ustaw również właściwość `TextMode` hasła na hasło. Następnie Dodaj kontrolkę CheckBox, ustawiając jej Właściwość `ID` na Kontrolka rememberMe i jej Właściwość `Text` na "Zapamiętaj mnie". Poniżej można dodać przycisk o nazwie LoginButton, którego właściwość `Text` jest ustawiona na wartość "login". Na koniec Dodaj kontrolkę sieci Web etykieta i ustaw jej Właściwość `ID` na InvalidCredentialsMessage, jej Właściwość `Text` na wartość "Twoja nazwa użytkownika lub hasło jest nieprawidłowe. Spróbuj ponownie. ", jego właściwość `ForeColor` na czerwoną i jej Właściwość `Visible` na wartość false.

W tym momencie ekran powinien wyglądać podobnie do zrzutu ekranu na rysunku 9, a składnia deklaracyjnej strony powinna wyglądać następująco:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample4.aspx)]

[![Strona logowania zawiera dwa pola tekstowe, pole wyboru, przycisk i etykietę](an-overview-of-forms-authentication-cs/_static/image22.png)](an-overview-of-forms-authentication-cs/_static/image21.png)

**Ilustracja 9**. Strona logowania zawiera dwa pola tekstowe, pole wyboru, przycisk i etykietę ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image23.png))

Na koniec Utwórz procedurę obsługi zdarzeń dla zdarzenia kliknięcia LoginButton. W projektancie kliknij dwukrotnie formant Button, aby utworzyć ten program obsługi zdarzeń.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Określanie, czy podane poświadczenia są prawidłowe

Teraz należy zaimplementować zadanie 2 w programie obsługi zdarzeń kliknięcia przycisku — określając, czy podane poświadczenia są prawidłowe. Aby to zrobić, musi to być magazyn użytkowników, który przechowuje wszystkie poświadczenia użytkowników, dzięki czemu można określić, czy podane poświadczenia są zgodne z dowolnymi znanymi poświadczeniami.

Przed ASP.NET 2,0 deweloperzy byli zobowiązani do wdrożenia zarówno własnych magazynów użytkowników, jak i pisania kodu w celu zweryfikowania podanych poświadczeń w sklepie. Większość deweloperów implementuje magazyn użytkowników w bazie danych, tworząc tabelę o nazwie Users z kolumnami, takimi jak UserName, Password, email, LastLoginDate i tak dalej. Ta tabela, następnie ma jeden rekord na konto użytkownika. Sprawdzenie, czy poświadczenia podane przez użytkownika będą wymagały wysyłania zapytań do bazy danych pod kątem zgodnej nazwy użytkownika, a następnie zagwarantowania, że hasło w bazie danych odpowiada dostarczonemu hasłu.

W przypadku ASP.NET 2,0 deweloperzy powinni używać jednego z dostawców członkostwa do zarządzania magazynem użytkowników. W tej serii samouczków będziemy korzystać z usługi SqlMembershipProvider, która używa SQL Server bazy danych dla magazynu użytkownika. W przypadku korzystania z SqlMembershipProvider musimy zaimplementować określony schemat bazy danych, który zawiera tabele, widoki i procedury składowane oczekiwane przez dostawcę. Sprawdzimy, jak zaimplementować ten schemat w samouczku ***Tworzenie schematu członkostwa w SQL Server*** . Gdy dostawca członkostwa jest używany, sprawdzanie poprawności poświadczeń użytkownika jest bardzo proste jako wywołanie metody ValidateUser [klasy członkostwa](https://msdn.microsoft.com/library/system.web.security.membership.aspx) [(*username*, *Password*)](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx), która zwraca wartość logiczną wskazującą, czy poprawność kombinacji *nazwy użytkownika* i *hasła* . W tej chwili nie możemy używać metody ValidateUser klasy członkostwa, ponieważ nie zaimplementowano jeszcze magazynu użytkownika SqlMembershipProvider.

Zamiast Poświęć czas na skompilowanie własnej niestandardowej tabeli bazy danych użytkowników (która byłaby przestarzała po zaimplementowaniu SqlMembershipProvider), spróbujmy na stałe wprowadzić prawidłowe poświadczenia na stronie logowania. W procedurze obsługi zdarzeń kliknięcia LoginButton Dodaj następujący kod:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample5.cs)]

Jak widać, istnieją trzy prawidłowe konta użytkowników — Scott, Jisun i sam — a wszystkie trzy mają takie samo hasło ("hasło"). Kod przechodzi przez tablice użytkowników i haseł szukających prawidłowej nazwy użytkownika i hasła. Jeśli nazwa użytkownika i hasło są prawidłowe, musimy zalogować użytkownika, a następnie przekierować je do odpowiedniej strony. Jeśli poświadczenia są nieprawidłowe, zostanie wyświetlona etykieta InvalidCredentialsMessage.

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

FormsAuthentication. RedirectFromLoginPage (UserName. text, kontrolka rememberMe. Checked);

Podczas tworzenia biletu uwierzytelniania formularzy używamy właściwości text pola tekstowego UserName dla parametru *Nazwa użytkownika* biletu uwierzytelniania formularzy i stan zaznaczenia pola wyboru kontrolka rememberMe dla parametru *persistCookie* .

Aby przetestować stronę logowania, odwiedź ją w przeglądarce. Zacznij od wprowadzenia nieprawidłowych poświadczeń, takich jak nazwa użytkownika "zgadza" i hasło "złe". Po kliknięciu przycisku Zaloguj zostanie wystawiony ogłaszanie zwrotne i zostanie wyświetlona etykieta InvalidCredentialsMessage.

[![etykieta InvalidCredentialsMessage jest wyświetlana po wprowadzeniu nieprawidłowych poświadczeń](an-overview-of-forms-authentication-cs/_static/image25.png)](an-overview-of-forms-authentication-cs/_static/image24.png)

**Rysunek 10**: etykieta InvalidCredentialsMessage jest wyświetlana po wprowadzeniu nieprawidłowych poświadczeń ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image26.png))

Następnie wprowadź prawidłowe poświadczenia i kliknij przycisk Zaloguj się. Tym razem, gdy następuje ogłaszanie zwrotne, zostanie utworzony bilet uwierzytelniania formularzy i nastąpi automatyczne przekierowanie do default. aspx. W tym momencie użytkownik zalogował się w witrynie sieci Web, chociaż nie ma żadnych wskazówek wizualnych wskazujących, że użytkownik jest aktualnie zalogowany. W kroku 4 zobaczymy, jak programowo określić, czy użytkownik jest zalogowany, jak również jak zidentyfikować użytkownika odwiedzającego stronę.

Krok 5 analizuje techniki rejestrowania użytkownika poza witryną sieci Web.

### <a name="securing-the-login-page"></a>Zabezpieczanie strony logowania

Gdy użytkownik wprowadzi swoje poświadczenia i przesyła formularz strony logowania, poświadczenia — łącznie z jego hasłem — są przesyłane przez Internet do serwera sieci Web w *postaci zwykłego tekstu*. Oznacza to, że każdy haker będący w trakcie wykrywania ruchu sieciowego zobaczy nazwę użytkownika i hasło. Aby tego uniknąć, należy zaszyfrować ruch sieciowy przy użyciu [protokołu SSL (Secure Sockets)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Dzięki temu poświadczenia (a także znacznik HTML całej strony) są szyfrowane od momentu opuszczenia przeglądarki do momentu odebrania ich przez serwer sieci Web.

Jeśli witryna sieci Web nie zawiera poufnych informacji, wystarczy użyć protokołu SSL na stronie logowania i na innych stronach, na których hasło użytkownika będzie wysyłane przez sieć w postaci zwykłego tekstu. Nie musisz martwić się o zabezpieczenie biletu uwierzytelniania formularzy, ponieważ domyślnie jest on szyfrowany i podpisany cyfrowo (aby zapobiec manipulowaniu). Dokładniejsze Omówienie zabezpieczeń biletów uwierzytelniania formularzy przedstawiono w poniższym samouczku.

> [!NOTE]
> Wiele witryn sieci Web finansowych i medycznych jest skonfigurowanych do używania protokołu SSL na *wszystkich* stronach dostępnych dla uwierzytelnionych użytkowników. Jeśli tworzysz taką witrynę sieci Web, możesz skonfigurować system uwierzytelniania formularzy tak, aby bilet uwierzytelniania formularzy był przesyłany tylko przez bezpieczne połączenie. W następnym samouczku zostaną omówione różne opcje konfiguracji uwierzytelniania formularzy, *[konfiguracja uwierzytelniania formularzy i Tematy zaawansowane](forms-authentication-configuration-and-advanced-topics-cs.md)* .

## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Krok 4. wykrywanie użytkowników uwierzytelnionych i określanie ich tożsamości

W tym momencie włączyłem funkcję uwierzytelniania formularzy i utworzono stronę logowania podstawowe, ale jeszcze nie udało nam się sprawdzić, czy użytkownik jest uwierzytelniany lub anonimowy. W niektórych scenariuszach firma Microsoft może chcieć wyświetlić różne dane lub informacje w zależności od tego, czy strona zostanie odwiedzana przez uwierzytelnionego, czy anonimowego użytkownika. Ponadto często musi znać tożsamość uwierzytelnionego użytkownika.

Uzupełnimy istniejącą stronę Default. aspx, aby zilustrować te techniki. W obszarze default. aspx Dodaj dwa kontrolki panelu, jeden o nazwie AuthenticatedMessagePanel i inny o nazwie AnonymousMessagePanel. Dodaj kontrolkę etykieta o nazwie WelcomeBackMessage na pierwszym panelu. W drugim panelu Dodaj kontrolkę hiperłącze, ustaw jej właściwość Text na wartość "log in" i jej właściwość NavigateUrl na wartość "~/login.aspx". W tym momencie znaczniki deklaratywne dla default. aspx powinny wyglądać podobnie do następujących:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample6.aspx)]

Ponieważ teraz prawdopodobnie odwołujesz się do Ciebie, pomysłem jest wyświetlenie tylko AuthenticatedMessagePanel do uwierzytelnionych osób odwiedzających, a tylko do AnonymousMessagePanel anonimowych odwiedzających. Aby to osiągnąć, należy ustawić widoczne właściwości tych paneli w zależności od tego, czy użytkownik jest zalogowany.

[Właściwość Request. IsAuthenticated](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) zwraca wartość logiczną wskazującą, czy żądanie zostało uwierzytelnione. Wprowadź następujący kod do strony,\_kod procedury obsługi zdarzeń ładowania:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample7.cs)]

Korzystając z tego kodu, odwiedź stronę Default. aspx za pomocą przeglądarki. Przy założeniu, że użytkownik jest jeszcze zalogowany, zostanie wyświetlony link do strony logowania (patrz rysunek 11). Kliknij ten link, aby zalogować się do witryny. Zgodnie z opisem w kroku 3 po wprowadzeniu poświadczeń nastąpi powrót do default. aspx, ale ten czas na stronie zostanie wyświetlony komunikat "Witamy!" komunikat (patrz rysunek 12).

![Po anonimowym odwiedzeniu jest wyświetlane łącze logowanie.](an-overview-of-forms-authentication-cs/_static/image27.png)

**Ilustracja 11**. po anonimowej odwiedzaniu zostanie wyświetlony link logowanie.

![Użytkownicy uwierzytelnieni są wyświetlani](an-overview-of-forms-authentication-cs/_static/image28.png)

**Ilustracja 12**. Użytkownicy uwierzytelnieni są pokazani "Witamy!" Komunikat

Możemy określić tożsamość zalogowanego użytkownika za pomocą [właściwości użytkownika](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) [obiektu HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx). Obiekt HttpContext reprezentuje informacje o bieżącym żądaniu i jest domem dla takich typowych obiektów ASP.NET jako odpowiedzi, żądania i sesji, między innymi. Właściwość User reprezentuje kontekst zabezpieczeń bieżącego żądania HTTP i implementuje [Interfejs IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

Właściwość User jest ustawiana przez FormsAuthenticationModule. W przypadku gdy FormsAuthenticationModule odnajdzie bilet uwierzytelniania formularzy w żądaniu przychodzącym, tworzy nowy obiekt GenericPrincipal — i przypisuje go do właściwości użytkownika.

Obiekty główne (na przykład GenericPrincipal —) zawierają informacje o tożsamości użytkownika i rolach, do których należą. Interfejs IPrincipal definiuje dwa elementy członkowskie:

- [IsInRole (*rolename*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) — Metoda zwracająca wartość logiczną wskazującą, czy podmiot zabezpieczeń należy do określonej roli.
- [Identity](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) — Właściwość zwracająca obiekt, który implementuje [interfejs IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). Interfejs IIdentity definiuje trzy właściwości: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx)i [name](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Możemy określić nazwę bieżącego gościa przy użyciu następującego kodu:

ciąg currentUsersName = User.Identity.Name;

W przypadku korzystania z uwierzytelniania formularzy [obiekt FormsIdentity](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) jest tworzony dla właściwości Identity GenericPrincipal —. Klasa FormsIdentity zawsze zwraca ciąg "Forms" dla jego właściwości AuthenticationType i ma wartość true dla właściwości IsAuthenticated. Właściwość Name zwraca nazwę użytkownika określoną podczas tworzenia biletu uwierzytelniania formularzy. Oprócz tych trzech właściwości FormsIdentity obejmuje dostęp do podstawowego biletu uwierzytelniania za pośrednictwem jego [Właściwości biletu](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). Właściwość biletu zwraca obiekt typu [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), który ma właściwości, takie jak wygaśnięcie, IsPersistent, IssueDate, Name i tak dalej.

Ważną kwestią jest to, że parametr *username* określony w FormsAuthentication. GetAuthCookie (*username*, *PersistCookie*), FormsAuthentication. SetAuthCookie (*username*, *persistCookie*) i FormsAuthentication. RedirectFromLoginPage (*username*, *persistCookie*) Metoda jest taka sama jak wartość zwracana przez User.Identity.Name. Ponadto bilet uwierzytelniania utworzony przez te metody jest dostępny przez rzutowanie User. Identity na obiekt FormsIdentity, a następnie uzyskanie dostępu do właściwości biletu:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample8.cs)]

Przekażmy bardziej spersonalizowany komunikat w default. aspx. Zaktualizuj stronę\_obsługi zdarzeń ładowania, tak że właściwość text etykiety WelcomeBackMessage ma przypisany ciąg "Witaj wstecz, *username*!"

WelcomeBackMessage. text = "Witaj wstecz," + User.Identity.Name + "!";

Rysunek 13 przedstawia efekt modyfikacji (podczas logowania użytkownika Scott).

![Komunikat powitalny zawiera nazwę aktualnie zalogowanego użytkownika](an-overview-of-forms-authentication-cs/_static/image29.png)

**Ilustracja 13**. Komunikat powitalny zawiera nazwę aktualnie zalogowanego użytkownika

### <a name="using-the-loginview-and-loginname-controls"></a>Korzystanie z kontrolek widoku logowania i LoginName

Wyświetlanie innej zawartości do uwierzytelnionych i anonimowych użytkowników jest powszechnym wymaganiem; Wyświetla nazwę aktualnie zalogowanego użytkownika. Z tego powodu ASP.NET obejmuje dwie kontrolki sieci Web, które udostępniają te same funkcje, które przedstawiono na rysunku 13, ale bez konieczności pisania jednego wiersza kodu.

[Formant widoku logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) to formant sieci Web oparty na szablonach, który ułatwia wyświetlanie różnych danych do uwierzytelnionych i anonimowych użytkowników. Widoku logowania zawiera dwa wstępnie zdefiniowane szablony:

- AnonymousTemplate — wszystkie znaczniki dodawane do tego szablonu są wyświetlane tylko dla użytkowników anonimowych.
- LoggedInTemplate — znaczniki tego szablonu są wyświetlane tylko dla uwierzytelnionych użytkowników.

Dodajmy formant widoku logowania do strony głównej witryny, site. Master. Zamiast dodawać tylko formant widoku logowania, należy dodać nową kontrolkę ContentPlaceHolder, a następnie umieścić kontrolkę widoku logowania w tym nowym elemencie ContentPlaceHolder. Uzasadnienie dla tej decyzji będzie widoczne wkrótce.

> [!NOTE]
> Oprócz AnonymousTemplate i LoggedInTemplate, formant widoku logowania może zawierać szablony specyficzne dla ról. Szablony specyficzne dla ról pokazują adiustację tylko dla tych użytkowników, którzy należą do określonej roli. W przyszłym samouczku sprawdzimy funkcje oparte na rolach kontrolki widoku logowania.

Zacznij od dodania elementu ContentPlaceHolder o nazwie LoginContent na stronę wzorcową w obszarze nawigacji &lt;bloku DIV&gt; elemencie. Możesz po prostu przeciągnąć kontrolkę ContentPlaceHolder z przybornika na widok źródła, umieszczając w tym miejscu znaczniki. Opis.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample9.aspx)]

Następnie Dodaj kontrolkę widoku logowania w elemencie LoginContent ContentPlaceHolder. Zawartość umieszczona w formantach ContentPlaceHolder na stronie wzorcowej jest traktowana jako *zawartość domyślna* dla elementu ContentPlaceHolder. Oznacza to, że strony ASP.NET, które używają tej strony wzorcowej, mogą określić własną zawartość dla każdego elementu ContentPlaceHolder lub użyć domyślnej zawartości strony wzorcowej.

Widoku logowania i inne kontrolki związane z logowaniem znajdują się na karcie Logowanie do przybornika.

![Kontrolka widoku logowania w przyborniku](an-overview-of-forms-authentication-cs/_static/image30.png)

**Ilustracja 14**. kontrolka widoku logowania w przyborniku

Następnie Dodaj dwa &lt;elementy br/&gt; bezpośrednio po kontrolce widoku logowania, ale nadal w elemencie ContentPlaceHolder. W tym momencie, Nawigacja &lt;DIV&gt; znacznik elementu powinien wyglądać następująco:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample10.aspx)]

Szablony widoku logowania można definiować z poziomu projektanta lub znaczników deklaratywnych. W projektancie programu Visual Studio rozwiń tag inteligentny widoku logowania, który wyświetla listę skonfigurowanych szablonów na liście rozwijanej. Wpisz tekst "Hello, dziwnego" do AnonymousTemplate; następnie Dodaj kontrolkę hiperłącze i ustaw jej właściwości text i NavigateUrl na wartość "log in" i "~/login.aspx".

Po skonfigurowaniu AnonymousTemplate przejdź do LoggedInTemplate, a następnie wprowadź tekst "Witaj ponownie". Następnie przeciągnij formant LoginName z przybornika do LoggedInTemplate, umieszczając go bezpośrednio po tekście "Witaj wstecz". [Kontrolka LoginName](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), jak jej nazwa oznacza, zawiera nazwę aktualnie zalogowanego użytkownika. Wewnętrznie formant LoginName po prostu wyprowadza Właściwość User.Identity.Name

Po wprowadzeniu tych dodatków do szablonów widoku logowania, znacznik powinien wyglądać podobnie do poniższego:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample11.aspx)]

Dodanie tej opcji do strony wzorcowej site. Master spowoduje wyświetlenie innego komunikatu w zależności od tego, czy użytkownik jest uwierzytelniony. Ilustracja 15 pokazuje domyślną stronę. aspx, gdy jest ona odwiedzana przez użytkownika Jisun. Komunikat "Witaj wstecz, Jisun" jest powtórzony dwukrotnie: raz w sekcji nawigacyjnej strony wzorcowej po lewej stronie (za pośrednictwem dodanej kontrolki widoku logowania) i jeden raz w obszarze zawartości default. aspx (za pomocą kontrolek panelu i logiki programowej).

![Zostanie wyświetlona kontrolka widoku logowania](an-overview-of-forms-authentication-cs/_static/image31.png)

**Ilustracja 15**. kontrolka widoku logowania wyświetla "Witaj wstecz, Jisun".

Ponieważ dodaliśmy widoku logowania do strony wzorcowej, może ona pojawić się na każdej stronie w naszej witrynie. Mogą jednak znajdować się na stronach sieci Web, w których nie ma być pokazywany ten komunikat. Jedną z takich stron jest strona logowania, ponieważ w tym miejscu pojawił się link do strony logowania. Ponieważ na stronie wzorcowej została umieszczona kontrolka widoku logowania, można zastąpić to ustawienie domyślne na naszej stronie zawartości. Otwórz login. aspx i przejdź do projektanta. Ponieważ nie zdefiniowano jawnie kontrolki zawartości w Login. aspx dla LoginContent ContentPlaceHolder na stronie wzorca, na stronie logowania zostanie wyświetlona domyślna Adiustacja strony głównej dla tego elementu ContentPlaceHolder. Można to sprawdzić za pomocą projektanta — LoginContent ContentPlaceHolder pokazuje znaczników domyślnych (kontrolki widoku logowania).

[![Strona logowania zawiera domyślną zawartość dla LoginContent ContentPlaceHolder strony głównej](an-overview-of-forms-authentication-cs/_static/image33.png)](an-overview-of-forms-authentication-cs/_static/image32.png)

**Ilustracja 16**. Strona logowania zawiera domyślną zawartość dla LoginContent ContentPlaceHolder strony wzorcowej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image34.png))

Aby zastąpić domyślne znaczniki dla LoginContent ContentPlaceHolder, po prostu kliknij prawym przyciskiem myszy region w Projektancie i wybierz opcję Utwórz niestandardową zawartość z menu kontekstowego. (W przypadku korzystania z programu Visual Studio 2008 element ContentPlaceHolder zawiera tag inteligentny, który w przypadku wybrania oferuje tę samą opcję). Spowoduje to dodanie nowej kontrolki zawartości do znacznika strony, co umożliwi nam zdefiniowanie zawartości niestandardowej na tej stronie. W tym miejscu możesz dodać niestandardowy komunikat, taki jak "Zaloguj się...", ale po prostu pozostaw to pole puste.

> [!NOTE]
> W programie Visual Studio 2005 Tworzenie zawartości niestandardowej tworzy pustą kontrolkę zawartości na stronie ASP.NET. Jednak w programie Visual Studio 2008 Tworzenie niestandardowej zawartości powoduje skopiowanie domyślnej zawartości strony wzorcowej do nowo utworzonej kontrolki zawartości. Jeśli używasz programu Visual Studio 2008, po utworzeniu nowej kontrolki zawartości upewnij się, że zawartość skopiowana z strony wzorcowej jest wyczyszczona.

Rysunek 17 przedstawia stronę login. aspx wyświetlaną w przeglądarce po wprowadzeniu tej zmiany. Zwróć uwagę, że w lewym okienku nawigacyjnym nie ma komunikatu "Hello, dziwnego" lub "Witamy w powrocie *" &lt;* DIV&gt;, ponieważ podczas odwiedzania domyślnego. aspx.

[![Strona logowania ukrywa domyślną adiustację LoginContent elementu ContentPlaceHolder](an-overview-of-forms-authentication-cs/_static/image36.png)](an-overview-of-forms-authentication-cs/_static/image35.png)

**Ilustracja 17**. Strona logowania ukrywa domyślne oznaczenie elementu LoginContent ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image37.png))

## <a name="step-5-logging-out"></a>Krok 5. Wylogowywanie

W kroku 3 zawarto informacje na temat tworzenia strony logowania w celu zarejestrowania użytkownika w witrynie, ale jeszcze zapoznaj się z tematem jak wylogować użytkownika. Oprócz metod rejestrowania użytkownika w programie Klasa FormsAuthentication również udostępnia [metodę wylogowaniu](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). Metoda wylogowaniu po prostu niszczy bilet uwierzytelniania formularzy, a tym samym rejestruje użytkownika poza lokacją.

Oferowanie linku do wylogowania to typowa funkcja, która ASP.NET zawiera kontrolkę zaprojektowaną specjalnie do rejestrowania użytkownika. [Kontrolka stanu logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) wyświetla "login" element LinkButton lub "Wyloguj", w zależności od stanu uwierzytelniania użytkownika. Element LinkButton "login" jest renderowany dla użytkowników anonimowych, podczas gdy dla uwierzytelnionych użytkowników jest wyświetlany komunikat o wylogowaniu "element LinkButton". Tekst dla LinkButtons "login" i "Wyloguj" można skonfigurować za pomocą właściwości LoginText i LogoutText stanu logowania.

Kliknięcie elementu "login" element LinkButton powoduje ogłoszenie zwrotne, z którego jest wysyłane przekierowanie na stronę logowania. Kliknięcie przycisku "Wyloguj" element LinkButton powoduje wywołanie przez formant stanu logowania metody FormsAuthentication. przygotowania, a następnie przekierowanie użytkownika do strony. Strona, do której zalogowany jest użytkownik, jest zależna od właściwości LogoutAction, która może być przypisana do jednej z trzech następujących wartości:

- Odśwież — wartość domyślna; przekierowuje użytkownika do właśnie odwiedzanej strony. Jeśli właśnie odwiedzana strona nie zezwala na użytkowników anonimowych, FormsAuthenticationModule automatycznie przekieruje użytkownika na stronę logowania.

W tym miejscu może być chcesz wiedzieć, dlaczego przekierowanie zostało wykonane. Jeśli użytkownik chce pozostawać na tej samej stronie, dlaczego jest to potrzebne w przypadku jawnego przekierowania? Przyczyną jest to, że po kliknięciu element LinkButton "Wyloguj" użytkownik nadal ma bilet uwierzytelniania formularzy w kolekcji plików cookie. W związku z tym żądanie ogłaszania zwrotnego jest żądaniem uwierzytelnionym. Kontrolka stanu logowania wywołuje metodę wylogowaniu, która jest wykonywana po uwierzytelnieniu użytkownika przez FormsAuthenticationModule. W związku z tym jawne przekierowanie powoduje ponowne zażądanie strony przez przeglądarkę. Gdy przeglądarka żąda ponownego żądania strony, bilet uwierzytelniania formularzy został usunięty i w związku z tym żądanie przychodzące jest anonimowe.

- Redirect — użytkownik jest przekierowywany do adresu URL określonego przez właściwość LogoutPageUrl stanu logowania.
- RedirectToLoginPage — użytkownik zostanie przekierowany do strony logowania.

Dodajmy kontrolkę stanu logowania do strony wzorcowej i skonfigurujesz ją do korzystania z opcji przekierowania, aby wysłać użytkownikowi komunikat z potwierdzeniem, że zostały wylogowane. Zacznij od utworzenia strony w katalogu głównym o nazwie Wyloguj. aspx. Nie zapomnij, aby skojarzyć Tę stronę z lokacją. Master Master. Następnie wprowadź komunikat na znaczniku strony, wyjaśniając użytkownikowi, że został wylogowany.

Następnie wróć do strony wzorcowej site. Master i Dodaj kontrolkę stanu logowania pod widoku logowaniaą w LoginContent ContentPlaceHolder. Ustaw właściwość LogoutAction formantu stanu logowania na wartość redirect i jej Właściwość LogoutPageUrl na wartość "~/Logout.aspx".

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample12.aspx)]

Ponieważ stanu logowania znajduje się poza kontrolką widoku logowania, zostanie ona wyświetlona zarówno dla użytkowników anonimowych, jak i uwierzytelnionych, ale jest to poprawne, ponieważ stanu logowania będzie poprawnie wyświetlała element LinkButton "login" lub "Wyloguj". Po dodaniu kontrolki stanu logowania, hiperlink "log in" w AnonymousTemplate jest zbędny, więc usuń go.

Na rysunku 18 są wyświetlane wartości Default. aspx podczas odwiedzin Jisun. Zwróć uwagę, że w lewej kolumnie jest wyświetlany komunikat "Witaj wstecz, Jisun" wraz z linkiem do wylogowania. Kliknięcie element LinkButton wylogowania powoduje odświeżenie, podpisanie Jisun z systemu, a następnie przekierowanie do wylogowania. aspx. Jak pokazano na rysunku 19, Jisun osiągnie wylogowanie. aspx został już wylogowany i dlatego jest anonimowy. W związku z tym lewa kolumna zawiera tekst "Welcome, dziwne" i link do strony logowania.

[![default. aspx zawiera](an-overview-of-forms-authentication-cs/_static/image39.png)](an-overview-of-forms-authentication-cs/_static/image38.png)

**Ilustracja 18**. default. aspx pokazuje "Witaj wstecz, Jisun" wraz z element LinkButton "Wyloguj" ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image40.png))

[![Wyloguj. aspx](an-overview-of-forms-authentication-cs/_static/image42.png)](an-overview-of-forms-authentication-cs/_static/image41.png)

**Ilustracja 19**. wylogowanie. aspx pokazuje "Welcome, dziwne" i "login" element LinkButton ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image43.png))

> [!NOTE]
> Zachęcam do dostosowania strony Wyloguj. aspx, aby ukryć LoginContent ContentPlaceHolder strony wzorcowej (podobnie jak w przypadku logowania. aspx w kroku 4). Przyczyną jest fakt, że element LinkButton "login" renderowany przez formant stanu logowania (jeden pod "Hello, dziwny") wysyła użytkownika do strony logowania przekazującej bieżący adres URL w parametrze QueryString ReturnUrl. W krótkim czasie, jeśli użytkownik, który wyloguje się, kliknie stanu logowania "login", a następnie zaloguje się, zostanie ponownie przekierowany do wylogowania. aspx, który mógłby łatwo pomylić użytkownika.

## <a name="summary"></a>Podsumowanie

W tym samouczku rozpocząłmy badanie przepływu pracy uwierzytelniania formularzy, a następnie w celu zaimplementowania uwierzytelniania formularzy w aplikacji ASP.NET. Uwierzytelnianie formularzy jest obsługiwane przez FormsAuthenticationModule, który ma dwie obowiązki: Identyfikowanie użytkowników na podstawie biletu uwierzytelniania formularzy i przekierowywanie nieautoryzowanych użytkowników na stronę logowania.

Klasa FormsAuthentication .NET Framework obejmuje metody tworzenia, inspekcji i usuwania biletów uwierzytelniania formularzy. Właściwość Request. IsAuthenticated i obiekt User zapewniają dodatkową pomoc techniczną do określenia, czy żądanie jest uwierzytelniane, oraz informacje o tożsamości użytkownika. Dostępne są również kontrolki sieci Web widoku logowania, stanu logowania i LoginName, które umożliwiają deweloperom szybkie i bezpłatne korzystanie z kodu na potrzeby wykonywania wielu typowych zadań związanych z logowaniem. Sprawdzimy te i inne kontrolki sieci Web powiązane z logowaniem bardziej szczegółowo w kolejnych samouczkach.

W tym samouczku przedstawiono Omówienie uwierzytelniania formularzy. Nie sprawdzono dostępnych opcji konfiguracji, należy zapoznać się z tematem działania biletów uwierzytelniania formularzy bez plików cookie lub poznać sposób, w jaki ASP.NET chroni zawartość biletu uwierzytelniania formularzy. W [następnym samouczku](forms-authentication-configuration-and-advanced-topics-cs.md)omówiono te tematy i wiele więcej.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Zmiany między usług IIS 6 i zabezpieczeniami IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Kontrolki ASP.NET logowania](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Professional ASP.NET 2,0 — zabezpieczenia, członkostwo i zarządzanie rolami](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Element `<authentication>`](https://msdn.microsoft.com/library/532aee0e.aspx)
- [Element `<forms>` dla `<authentication>`](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Szkolenia wideo dotyczące tematów zawartych w tym samouczku

- [Korzystanie z uwierzytelniania podstawowego formularzy na platformie ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Osoba dokonująca przeglądu potencjalnych klientów dla tego samouczka została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka obejmują Alicja Maziarz, Jan suru i Teresa Murphy. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](security-basics-and-asp-net-support-cs.md)
> [dalej](forms-authentication-configuration-and-advanced-topics-cs.md)

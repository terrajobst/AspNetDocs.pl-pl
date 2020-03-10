---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
title: Przetwarzanie nieobsługiwanych wyjątków (VB) | Microsoft Docs
author: rick-anderson
description: Gdy wystąpi błąd w czasie wykonywania w aplikacji sieci Web w środowisku produkcyjnym, ważne jest powiadomienie dewelopera i zarejestrowanie błędu, aby mógł zostać zdiagnozowany na La...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 051296f0-9519-4e78-835c-d868da13b0a0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 8d2e12af29bd95ce9898fa6a5e81be8b7a761f76
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586066"
---
# <a name="processing-unhandled-exceptions-vb"></a>Przetwarzanie nieobsługiwanych wyjątków (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/web-forms/overview/older-versions-getting-started/deploying-web-site-projects/processing-unhandled-exceptions-vb/samples) ([jak pobrać](/aspnet/core/tutorials/index#how-to-download-a-sample))

> Gdy wystąpi błąd w czasie wykonywania w aplikacji sieci Web w środowisku produkcyjnym, ważne jest powiadomienie dewelopera i zarejestrowanie błędu, aby mógł zostać zdiagnozowany w późniejszym czasie. Ten samouczek zawiera omówienie sposobu, w jaki program ASP.NET przetwarza błędy środowiska uruchomieniowego i sprawdza jeden ze sposobów, aby można było wykonać kod niestandardowy za każdym razem, gdy nieobsłużony wyjątek jest ustawiony na środowisko uruchomieniowe ASP.NET.

## <a name="introduction"></a>Wprowadzenie

Gdy wystąpił nieobsługiwany wyjątek w aplikacji ASP.NET, jest on bąbelkowy do środowiska uruchomieniowego ASP.NET, które wywołuje zdarzenie `Error` i wyświetla odpowiednią stronę błędu. Istnieją trzy różne typy stron błędów: błąd środowiska uruchomieniowego żółty ekran o śmierci (YSOD); Szczegóły wyjątku YSOD; i niestandardowe strony błędów. W [poprzednim samouczku](displaying-a-custom-error-page-vb.md) skonfigurujemy aplikację pod kątem używania niestandardowej strony błędu dla użytkowników zdalnych i YSOD szczegóły wyjątku dla użytkowników odwiedzających lokalnie.

Za pomocą przyjaznej dla człowieka strony błędu niestandardowego, która pasuje do wyglądu i działania witryny, preferowany jest domyślny błąd środowiska uruchomieniowego YSOD, ale wyświetlenie niestandardowej strony błędu jest tylko jedną częścią kompleksowego rozwiązania do obsługi błędów. Gdy wystąpi błąd w aplikacji w środowisku produkcyjnym, ważne jest, aby deweloperzy byli powiadamiani o błędzie, aby mogli Unearth przyczynę wyjątku i rozwiązać ten problem. Ponadto szczegóły błędu powinny być rejestrowane, aby można było zbadać błąd i zdiagnozować go w późniejszym czasie.

W tym samouczku pokazano, jak uzyskać dostęp do szczegółów nieobsługiwanego wyjątku, aby mogły być rejestrowane i zostać powiadomione przez twórcę. Dwa samouczki po tej stronie eksplorują biblioteki błędów, które po zakończeniu konfiguracji będą automatycznie powiadamiać deweloperów o błędach środowiska uruchomieniowego i rejestrować ich szczegóły.

> [!NOTE]
> Informacje przedstawione w tym samouczku są najbardziej przydatne, jeśli trzeba przetwarzać Nieobsłużone wyjątki w sposób unikatowy lub dostosowany. W przypadkach, gdy wystarczy zarejestrować wyjątek i powiadomić dewelopera, przy użyciu biblioteki rejestrowania błędów jest to sposób. Następne dwa samouczki zawierają przegląd dwóch takich bibliotek.

## <a name="executing-code-when-theerrorevent-is-raised"></a>Wykonywanie kodu po wywołaniu zdarzenia`Error`

Zdarzenia zapewniają obiektowi mechanizm sygnalizujący, że wystąpił interesujący element, oraz dla innego obiektu, aby wykonać kod w odpowiedzi. Jako deweloper ASP.NET, że zastanawiasz się nad zdarzeniami. Jeśli chcesz uruchomić jakiś kod, gdy odwiedzający kliknie określony przycisk, utworzysz procedurę obsługi zdarzeń dla zdarzenia `Click` tego przycisku i umieścisz w nim kod. Ponieważ środowisko uruchomieniowe ASP.NET wywołuje swoje [zdarzenie`Error`](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) , gdy wystąpi nieobsługiwany wyjątek, następuje, że kod rejestrowania szczegółów błędu zostałby przemieszczony w programie obsługi zdarzeń. Ale jak utworzyć procedurę obsługi zdarzeń dla zdarzenia `Error`?

Zdarzenie `Error` jest jednym z wielu zdarzeń w [klasie`HttpApplication`](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) , które są wywoływane na określonych etapach potoku HTTP w trakcie okresu istnienia żądania. Na przykład [zdarzenie`BeginRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx) klasy `HttpApplication` jest zgłaszane na początku każdego żądania; [`AuthenticateRequest` zdarzenie](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) jest zgłaszane, gdy moduł zabezpieczeń zidentyfikował zleceniodawcę. Te zdarzenia `HttpApplication` umożliwiają deweloperom stronicowym wykonywanie logiki niestandardowej w różnych punktach w okresie istnienia żądania.

Procedury obsługi zdarzeń dla zdarzeń `HttpApplication` mogą być umieszczane w specjalnym pliku o nazwie `Global.asax`. Aby utworzyć ten plik w witrynie sieci Web, Dodaj nowy element do katalogu głównego witryny sieci Web przy użyciu szablonu globalnej klasy aplikacji o nazwie `Global.asax`.

[![](processing-unhandled-exceptions-vb/_static/image2.png)](processing-unhandled-exceptions-vb/_static/image1.png)

**Rysunek 1**. Dodawanie `Global.asax` do aplikacji sieci Web  
 ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](processing-unhandled-exceptions-vb/_static/image3.png))

Zawartość i struktura pliku `Global.asax` utworzonego przez program Visual Studio różni się nieco w zależności od tego, czy używasz projektu aplikacji sieci Web (WAP), czy projektu witryny sieci Web (WSP). W przypadku serwera WAP `Global.asax` jest implementowana jako dwa osobne pliki — `Global.asax` i `Global.asax.vb`. Plik `Global.asax` zawiera Nothing, ale dyrektywa `@Application` odwołująca się do pliku `.vb`; programy obsługi zdarzeń są zdefiniowane w pliku `Global.asax.vb`. Dla WSPs jest tworzony tylko jeden plik, `Global.asax`i programy obsługi zdarzeń zdefiniowane w bloku `<script runat="server">`.

Plik `Global.asax` utworzony w ramach szablonu globalnej klasy aplikacji w programie Visual Studio zawiera programy obsługi zdarzeń o nazwach `Application_BeginRequest`, `Application_AuthenticateRequest`i `Application_Error`, które są obsługą zdarzeń `HttpApplication` `BeginRequest`, `AuthenticateRequest`i `Error`. Istnieją również procedury obsługi zdarzeń o nazwie `Application_Start`, `Session_Start`, `Application_End`i `Session_End`, które są programami obsługi zdarzeń, które są uruchamiane podczas uruchamiania aplikacji sieci Web, gdy zostanie uruchomiona nowa sesja, gdy aplikacja zostanie zakończona, a sesja zostanie zakończona odpowiednio. Plik `Global.asax` utworzony w programie WSP przez program Visual Studio zawiera tylko `Application_Error`, `Application_Start`, `Session_Start`, `Application_End`i `Session_End` programów obsługi zdarzeń.

> [!NOTE]
> Podczas wdrażania aplikacji ASP.NET należy skopiować plik `Global.asax` do środowiska produkcyjnego. Plik `Global.asax.vb`, który jest tworzony w ramach serwera WAP, nie musi być kopiowany do środowiska produkcyjnego, ponieważ ten kod jest kompilowany do zestawu projektu.

Procedury obsługi zdarzeń utworzone przez szablon globalnej klasy aplikacji programu Visual Studio nie są wyczerpujące. Można dodać program obsługi zdarzeń dla dowolnego zdarzenia `HttpApplication`, nadając nazwę `Application_EventName`programu obsługi zdarzeń. Na przykład można dodać następujący kod do pliku `Global.asax`, aby utworzyć procedurę obsługi zdarzeń dla [zdarzenia`AuthorizeRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx):

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample1.vb)]

Analogicznie, można usunąć wszystkie programy obsługi zdarzeń utworzone przez szablon globalnej klasy aplikacji, które nie są potrzebne. W tym samouczku Wymagamy tylko programu obsługi zdarzeń dla zdarzenia `Error`. Możesz usunąć inne programy obsługi zdarzeń z pliku `Global.asax`.

> [!NOTE]
> *Moduły HTTP* oferują inny sposób definiowania programów obsługi zdarzeń dla zdarzeń `HttpApplication`. Moduły HTTP są tworzone jako plik klasy, który może być umieszczony bezpośrednio w projekcie aplikacji sieci Web lub oddzielony do oddzielnej biblioteki klas. Ponieważ można je oddzielić do biblioteki klas, moduły HTTP oferują bardziej elastyczny i wielokrotny model do tworzenia programów obsługi zdarzeń `HttpApplication`. Ponieważ plik `Global.asax` jest specyficzny dla aplikacji sieci Web, w której znajduje się, moduły HTTP można kompilować do zestawów, a w tym momencie dodanie modułu HTTP do witryny sieci Web jest proste, ponieważ powoduje to usunięcie zestawu w folderze `Bin` i zarejestrowanie modułu w `Web.config`. Ten samouczek nie ma na celu tworzenia i używania modułów HTTP, ale dwie biblioteki rejestrowania błędów używane w poniższych dwóch samouczkach są implementowane jako moduły HTTP. Aby uzyskać więcej informacji na temat zalet modułów HTTP, odwołuje się do [programu przy użyciu modułów i programów obsługi HTTP w celu utworzenia podłączanych składników ASP.NET](https://msdn.microsoft.com/library/aa479332.aspx).

## <a name="retrieving-information-about-the-unhandled-exception"></a>Pobieranie informacji o nieobsługiwanym wyjątku

W tym momencie mamy plik Global. asax z programem obsługi zdarzeń `Application_Error`. Gdy ten program obsługi zdarzeń jest wykonywany, musimy powiadomić dewelopera o błędzie i zarejestrować jego szczegóły. Aby wykonać te zadania, najpierw musimy określić szczegóły zgłoszonego wyjątku. Użyj [metody`GetLastError`](https://msdn.microsoft.com/library/system.web.httpserverutility.getlasterror.aspx) obiektu serwera, aby pobrać szczegóły nieobsłużonego wyjątku, które spowodowało wyzwolenie zdarzenia `Error`.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample2.vb)]

Metoda `GetLastError` zwraca obiekt typu `Exception`, który jest typem podstawowym dla wszystkich wyjątków w .NET Framework. Jednak w kodzie powyżej mam rzutowanie obiektu wyjątku zwróconego przez `GetLastError` do obiektu `HttpException`. Jeśli `Error` zdarzenie jest wyzwalane, ponieważ wystąpił wyjątek podczas przetwarzania zasobu ASP.NET, wygenerowany wyjątek jest opakowany w `HttpException`. Aby uzyskać rzeczywisty wyjątek, który wytrącł zdarzenie błędu, użyj właściwości `InnerException`. Jeśli zdarzenie `Error` zostało zgłoszone z powodu wyjątku opartego na protokole HTTP, takiego jak żądanie nieistniejącej strony, zostanie zgłoszony `HttpException`, ale nie ma wyjątku wewnętrznego.

Poniższy kod używa `GetLastErrormessage` do pobierania informacji o wyjątku, który wyzwolił zdarzenie `Error`, przechowując `HttpException` w zmiennej o nazwie `lastErrorWrapper`. Następnie zapisuje typ, komunikat i ślad stosu źródłowego wyjątku w trzech zmiennych ciągu, sprawdzając, czy `lastErrorWrapper` jest faktyczny wyjątek, który wyzwolił zdarzenie `Error` (w przypadku wyjątków opartych na protokole HTTP) lub jeśli jest tylko otoką dla wyjątku zgłoszonego podczas przetwarzania żądania.

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample3.vb)]

W tym momencie masz wszystkie informacje potrzebne do napisania kodu, który będzie rejestrował szczegóły wyjątku w tabeli bazy danych. Można utworzyć tabelę bazy danych z kolumnami dla każdej z nich ze szczegółami błędu — typu, komunikatu, śladu stosu i tak dalej, jak również innych użytecznych fragmentów informacji, takich jak adres URL żądanej strony i nazwa aktualnie zalogowanego użytkownika. W programie obsługi zdarzeń `Application_Error` następnie nawiąż połączenie z bazą danych i Wstaw rekord do tabeli. Analogicznie, można dodać kod, aby poinformować dewelopera o błędzie za pośrednictwem poczty e-mail.

Biblioteki rejestrowania błędów, które są badane w następnych dwóch samouczkach, zapewniają takie funkcje, więc nie ma potrzeby tworzenia tego rejestrowania błędów i powiadamiania. Jednak, aby zilustrować, że zdarzenie `Error` jest zgłaszane i że można użyć programu obsługi zdarzeń `Application_Error` do zarejestrowania szczegółów błędu i powiadomienia dewelopera, dodajmy kod, który powiadamia dewelopera o wystąpieniu błędu.

## <a name="notifying-a-developer-when-an-unhandled-exception-occurs"></a>Powiadamianie dewelopera o wystąpieniu nieobsługiwanego wyjątku

Gdy wystąpił nieobsługiwany wyjątek w środowisku produkcyjnym, ważne jest, aby ostrzec zespół programistyczny, aby mógł ocenić błąd i określić, jakie akcje muszą zostać podjęte. Na przykład jeśli wystąpi błąd podczas łączenia się z bazą danych, należy dokładnie sprawdzić parametry połączenia i, być może, otworzyć bilet pomocy technicznej w firmie hostingowej w sieci Web. Jeśli wystąpił wyjątek z powodu błędu programowania, może być konieczne dodanie dodatkowego kodu lub logiki walidacji w celu uniknięcia takich błędów w przyszłości.

Klasy .NET Framework w [przestrzeni nazw`System.Net.Mail`](https://msdn.microsoft.com/library/system.net.mail.aspx) ułatwiają wysyłanie wiadomości e-mail. [Klasa`MailMessage`](https://msdn.microsoft.com/library/system.net.mail.mailmessage.aspx) reprezentuje wiadomość e-mail i ma właściwości, takie jak `To`, `From`, `Subject`, `Body`i `Attachments`. `SmtpClass` jest używany do wysyłania obiektu `MailMessage` przy użyciu określonego serwera SMTP; Ustawienia serwera SMTP można określić programowo lub deklaratywnie w [elemencie`<system.net>`](https://msdn.microsoft.com/library/6484zdc1.aspx) w `Web.config file`. Aby uzyskać więcej informacji na temat wysyłania wiadomości e-mail w aplikacji ASP.NET, zapoznaj się z artykułem, [wysyłaniem poczty e-mail w ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)i [System .NET. mail — często zadawane pytania](http://systemnetmail.com/).

> [!NOTE]
> Element `<system.net>` zawiera ustawienia serwera SMTP używane przez klasę `SmtpClient` podczas wysyłania wiadomości e-mail. Firma hostingowa w sieci Web może mieć serwer SMTP, za pomocą którego można wysyłać wiadomości e-mail z aplikacji. Zapoznaj się z sekcją pomocy technicznej hosta sieci Web, aby uzyskać informacje na temat ustawień serwera SMTP, które powinny być używane w aplikacji sieci Web.

Dodaj następujący kod do programu obsługi zdarzeń `Application_Error`, aby wysłać deweloperowi wiadomość e-mail po wystąpieniu błędu:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample4.vb)]

Chociaż powyższy kod jest dość długi, jego większość tworzy kod HTML, który pojawia się w wiadomości e-mail wysyłanej do dewelopera. Kod zaczyna się od odwołującego się do `HttpException` zwróconego przez metodę `GetLastError` (`lastErrorWrapper`). Rzeczywisty wyjątek, który został zgłoszony przez żądanie, jest pobierany za pośrednictwem `lastErrorWrapper.InnerException` i jest przypisywany do zmiennej `lastError`. Informacje śledzenia typu, komunikatu i stosu są pobierane z `lastError` i przechowywane w trzech zmiennych ciągu.

Następnie tworzony jest obiekt `MailMessage` o nazwie `mm`. Treść wiadomości e-mail jest sformatowana w formacie HTML i wyświetla adres URL żądanej strony, nazwę aktualnie zalogowanego użytkownika oraz informacje o wyjątku (typ, komunikat i ślad stosu). Jedną z ciekawych rzeczy dotyczących klasy `HttpException` jest możliwość generowania kodu HTML używanego do tworzenia żółtego ekranu z informacjami o wyjątku (YSOD) przez wywołanie [metody GetHtmlErrorMessage](https://msdn.microsoft.com/library/system.web.httpexception.gethtmlerrormessage.aspx). Ta metoda jest używana tutaj, aby pobrać szczegóły wyjątku YSOD Markup i dodać je do wiadomości e-mail jako załącznik. Jeden z wyrazów Przestroga: Jeśli wyjątek, który wyzwolił zdarzenie `Error` był wyjątek oparty na protokole HTTP (na przykład żądanie nieistniejącej strony), Metoda `GetHtmlErrorMessage` zwróci `null`.

Ostatnim krokiem jest wysłanie `MailMessage`. W tym celu należy utworzyć nową metodę `SmtpClient` i wywołać jej metodę `Send`.

> [!NOTE]
> Przed użyciem tego kodu w aplikacji sieci Web należy zmienić wartości `ToAddress` i `FromAddress` stałe z support@example.com na dowolny adres e-mail, do którego należy wysłać wiadomość e-mail z powiadomieniem o błędzie. Należy również określić ustawienia serwera SMTP w sekcji `<system.net>` w `Web.config`. Skontaktuj się z dostawcą hosta sieci Web, aby określić ustawienia serwera SMTP, które mają być używane.

W przypadku tego kodu w dowolnym momencie wystąpił błąd podczas gdy projektant otrzymuje wiadomość e-mail z podsumowaniem błędu i zawiera YSOD. W poprzednim samouczku przedstawiono błąd w czasie wykonywania, odwiedzając gatunek. aspx i przekazując nieprawidłową wartość `ID` za pomocą ciągu QueryString, takiego jak `Genre.aspx?ID=foo`. Odwiedzanie strony z plikiem `Global.asax` w miejscu spowoduje takie samo środowisko użytkownika jak w poprzednim samouczku — w środowisku programistycznym nadal zobaczysz szczegóły wyjątku żółtego ekranu zgonu, podczas gdy w środowisku produkcyjnym zobaczysz stronę błędu niestandardowego. Oprócz tego istniejącego zachowania deweloper otrzymuje wiadomość e-mail.

**Rysunek 2** przedstawia wiadomość e-mail odebraną podczas odwiedzania `Genre.aspx?ID=foo`. Treść wiadomości e-mail podsumowuje informacje o wyjątku, podczas gdy załącznik `YSOD.htm` wyświetla zawartość, która jest wyświetlana w YSOD szczegóły wyjątku (patrz **rysunek 3**).

[![](processing-unhandled-exceptions-vb/_static/image5.png)](processing-unhandled-exceptions-vb/_static/image4.png)

**Rysunek 2**. deweloper otrzymuje powiadomienie E-mail za każdym razem, gdy wystąpił nieobsługiwany wyjątek  
 ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](processing-unhandled-exceptions-vb/_static/image6.png))

[![](processing-unhandled-exceptions-vb/_static/image8.png)](processing-unhandled-exceptions-vb/_static/image7.png)

**Rysunek 3**. powiadomienie E-mail zawiera szczegóły wyjątku YSOD jako załącznik  
 ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](processing-unhandled-exceptions-vb/_static/image9.png))

## <a name="what-about-using-the-custom-error-page"></a>Jak korzystać z niestandardowej strony błędu?

W tym samouczku pokazano, jak używać `Global.asax` i programu obsługi zdarzeń `Application_Error` do wykonywania kodu w przypadku wystąpienia nieobsługiwanego wyjątku. W tym celu program obsługi zdarzeń został użyty w celu powiadomienia dewelopera o błędzie. możemy ją rozłożyć, aby rejestrować szczegółowe informacje o błędach w bazie danych. Obecność programu obsługi zdarzeń `Application_Error` nie ma wpływu na środowisko użytkownika końcowego. Nadal widzisz skonfigurowaną stronę błędu, być może YSOD szczegóły błędu, błąd środowiska uruchomieniowego YSOD lub niestandardową stronę błędu.

W przypadku korzystania ze strony błędów niestandardowych jest to bardzo naturalne, aby zastanawiać się, czy plik `Global.asax` i `Application_Error`. Gdy wystąpi błąd, użytkownik jest pokazywany niestandardowa strona błędu, dlatego nie można umieścić kodu w celu powiadomienia dewelopera i zarejestrować szczegóły błędu w klasie związanej z kodem na stronie błędu niestandardowego? Chociaż można na pewno dodać kod do klasy związanej z kodem niestandardową strony błędu, nie masz dostępu do szczegółów wyjątku, który wyzwolił zdarzenie `Error` w przypadku korzystania z techniki poświęconej w poprzednim samouczku. Wywołanie metody `GetLastError` ze strony błędów niestandardowych zwraca `Nothing`.

Przyczyną takiego zachowania jest fakt, że strona błędu niestandardowego zostanie osiągnięta za pośrednictwem przekierowania. Gdy nieobsługiwany wyjątek osiągnie środowisko uruchomieniowe ASP.NET, aparat ASP.NET zgłasza zdarzenie `Error` (które wykonuje procedurę obsługi zdarzeń `Application_Error`), a następnie *przekierowuje* użytkownika na stronę błędu niestandardowego przez wystawienie `Response.Redirect(customErrorPageUrl)`. Metoda `Response.Redirect` wysyła odpowiedź do klienta z kodem stanu HTTP 302, co sprawia, że przeglądarka zażąda nowego adresu URL, a mianowicie strony błędu niestandardowego. Przeglądarka automatycznie żąda tej nowej strony. Można stwierdzić, że niestandardowa strona błędu została zażądana niezależnie od strony, z której pochodzi błąd, ponieważ pasek adresu przeglądarki zmienia się na adres URL strony błędu niestandardowego (patrz **rysunek 4**).

[![](processing-unhandled-exceptions-vb/_static/image11.png)](processing-unhandled-exceptions-vb/_static/image10.png)

**Ilustracja 4**. gdy wystąpi błąd, przeglądarka zostanie przekierowana do adresu URL strony błędu niestandardowego  
 ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](processing-unhandled-exceptions-vb/_static/image12.png))

Efektem netto jest to, że żądanie, w którym wystąpił nieobsługiwany wyjątek, następuje, gdy serwer reaguje na Przekierowanie HTTP 302. Kolejne żądanie do niestandardowej strony błędu to zupełnie nowe żądanie; w tym momencie aparat ASP.NET odrzucił informacje o błędzie, a ponadto nie ma sposobu kojarzenia nieobsłużonego wyjątku w poprzednim żądaniu z nowym żądaniem dla niestandardowej strony błędu. Dlatego `GetLastError` zwraca `null`, gdy zostanie wywołana ze strony błędu niestandardowego.

Jednak można mieć niestandardową stronę błędu wykonywaną w ramach tego samego żądania, które spowodowało błąd. Metoda [`Server.Transfer(url)`](https://msdn.microsoft.com/library/system.web.httpserverutility.transfer.aspx) przenosi wykonywanie do określonego adresu URL i przetwarza je w ramach tego samego żądania. Kod w obsłudze zdarzeń `Application_Error` można przenieść do klasy z kodem związanym ze stroną błędu niestandardowego, zastępując go w `Global.asax` następującym kodem:

[!code-vb[Main](processing-unhandled-exceptions-vb/samples/sample5.vb)]

Teraz, gdy wystąpił nieobsługiwany wyjątek, program obsługi zdarzeń `Application_Error` przenosi formant do odpowiedniej niestandardowej strony błędu na podstawie kodu stanu HTTP. Ponieważ formant został przeniesiony, strona błędu niestandardowego ma dostęp do nieobsługiwanych informacji o wyjątku za pośrednictwem `Server.GetLastError` i może powiadomić dewelopera o błędzie i zarejestrować jego szczegóły. Wywołanie `Server.Transfer` uniemożliwia przekierowaniu użytkownika do niestandardowej strony błędu przez aparat ASP.NET. Zamiast tego zawartość strony błędu niestandardowego jest zwracana jako odpowiedź do strony, która wygenerowała błąd.

## <a name="summary"></a>Podsumowanie

Gdy wystąpił nieobsługiwany wyjątek w aplikacji sieci Web ASP.NET, środowisko uruchomieniowe ASP.NET wywołuje zdarzenie `Error` i wyświetla skonfigurowaną stronę błędu. Możemy powiadomić dewelopera o błędzie, rejestrować jego szczegóły lub przetwarzać w inny sposób, tworząc procedurę obsługi zdarzeń dla zdarzenia błędu. Istnieją dwa sposoby tworzenia obsługi zdarzeń dla zdarzeń `HttpApplication`, takich jak `Error`: w pliku `Global.asax` lub w module protokołu HTTP. W tym samouczku pokazano, jak utworzyć procedurę obsługi zdarzeń `Error` w pliku `Global.asax`, który powiadamia deweloperów o błędzie za pomocą wiadomości e-mail.

Tworzenie procedury obsługi zdarzeń `Error` jest przydatne, jeśli trzeba przetwarzać Nieobsłużone wyjątki w sposób unikatowy lub dostosowany. Jednak utworzenie własnego programu obsługi zdarzeń `Error` w celu zarejestrowania wyjątku lub powiadomienia dewelopera nie jest najbardziej wydajnym wykorzystaniem czasu, ponieważ istnieje już bezpłatny i łatwy do użycia biblioteki rejestrowania błędów, które mogą być skonfigurowane w ciągu kilku minut. Następne dwa samouczki przeanalizują dwie takie biblioteki.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [ASP.NET modułów HTTP i obsługi HTTP — Omówienie](https://support.microsoft.com/kb/307985)
- [Bezpieczne reagowanie na Nieobsłużone wyjątki — przetwarzanie nieobsługiwanych wyjątków](http://aspnet.4guysfromrolla.com/articles/091306-1.aspx)
- [Klasa `HttpApplication` i obiekt aplikacji ASP.NET](http://www.eggheadcafe.com/articles/20030211.asp)
- [Obsługa protokołu HTTP i moduły HTTP w programie ASP.NET](http://www.15seconds.com/Issue/020417.htm)
- [Wysyłanie wiadomości E-mail w ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [Informacje o pliku `Global.asax`](http://aspalliance.com/1114_Understanding_the_Globalasax_file.all)
- [Tworzenie podłączanych składników ASP.NET za pomocą modułów i programów HTTP](https://msdn.microsoft.com/library/aa479332.aspx)
- [Praca z plikiem `Global.asax` ASP.NET](http://articles.techrepublic.com.com/5100-10878_11-5771721.html)
- [Praca z wystąpieniami `HttpApplication`](https://msdn.microsoft.com/library/a0xez8f2.aspx)

> [!div class="step-by-step"]
> [Poprzednie](displaying-a-custom-error-page-vb.md)
> [dalej](logging-error-details-with-asp-net-health-monitoring-vb.md)

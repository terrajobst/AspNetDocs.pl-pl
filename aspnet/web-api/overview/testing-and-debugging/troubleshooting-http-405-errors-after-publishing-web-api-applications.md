---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: Rozwiązywanie problemów z aplikacjami sieci Web API2 działających w programie Visual Studio i niepowodzeniem na produkcyjnym serwerze usług IIS
author: rmcmurray
description: Rozwiązywanie problemów z aplikacjami sieci Web API2 działających w programie Visual Studio i niepowodzeniem na produkcyjnym serwerze usług IIS
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: 1b47f1ade3619cfd010260352f6a96985ab3598b
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445710"
---
# <a name="troubleshoot-web-api2-apps-that-work-in-visual-studio-and-fail-on-a-production-iis-server"></a>Rozwiązywanie problemów z aplikacjami sieci Web API2 działających w programie Visual Studio i niepowodzeniem na produkcyjnym serwerze usług IIS

> W tym dokumencie wyjaśniono, jak rozwiązywać problemy z aplikacjami sieci Web API2 wdrożonymi na serwerze produkcyjnym usług IIS. Rozwiązuje ona typowe błędy HTTP 405 i 501.
> 
> ## <a name="software-used-in-this-tutorial"></a>Oprogramowanie używane w tym samouczku
> 
> 
> - [Internet Information Services (IIS)](https://www.iis.net/) (wersja 7 lub nowsza)
> - [Interfejs Web API](../../index.md) 

Aplikacje interfejsu API sieci Web zwykle używają kilku czasowników HTTP: GET, POST, PUT, DELETE i czasami poprawek. W takim przypadku deweloperzy mogą działać w sytuacjach, w których te zlecenia są implementowane przez inny moduł usług IIS na serwerze produkcyjnym usług IIS, który prowadzi do sytuacji, w której kontroler internetowego interfejsu API działa prawidłowo w programie Visual Studio lub na serwerze deweloperskim. Po wdrożeniu na produkcyjnym serwerze usług IIS Zwróć błąd HTTP 405.

## <a name="what-causes-http-405-errors"></a>Co powoduje błędy HTTP 405

Pierwszy krok w kierunku uczenia się, jak rozwiązywać problemy z błędami HTTP 405, to zrozumienie, co faktycznie oznacza błąd HTTP 405. Podstawowym dokumentem regulującym protokół HTTP jest [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), który definiuje kod stanu HTTP 405 jako ***metodę niedozwolony***, a dodatkowo opisuje ten kod stanu jako sytuację, w której &quot;metoda określona w wierszu żądania nie jest dozwolona dla zasób identyfikowany przez identyfikator URI żądania.&quot; innymi słowy, czasownik HTTP jest niedozwolony dla określonego adresu URL, którego żądał klient HTTP.

Poniżej przedstawiono kilka najbardziej używanych metod HTTP, zgodnie z definicją w dokumencie RFC 2616, RFC 4918 i RFC 5789:

| Metoda HTTP | Opis |
| --- | --- |
| **Pobierz** | Ta metoda służy do pobierania danych z identyfikatora URI i prawdopodobnie najczęściej używana metoda HTTP. |
| **MTP** | Ta metoda jest podobnie jak Metoda GET, z tą różnicą, że nie pobiera danych z identyfikatora URI żądania — po prostu Pobiera stan HTTP. |
| **POUBOJOWEGO** | Ta metoda jest zwykle używana do wysyłania nowych danych do identyfikatora URI. WPIS jest często używany do przesyłania danych formularza. |
| **Ubrani** | Ta metoda jest zwykle używana do wysyłania danych pierwotnych do identyfikatora URI; Parametr PUT jest często używany do przesyłania danych JSON lub XML do aplikacji interfejsu API sieci Web. |
| **USUNIĘTY** | Ta metoda służy do usuwania danych z identyfikatora URI. |
| **Opcje** | Ta metoda jest zwykle używana do pobierania listy metod HTTP, które są obsługiwane przez identyfikator URI. |
| **KOPIUJ PRZENIESIENIE** | Te dwie metody są używane z protokołem WebDAV, a ich celem nie jest wyjaśnienie. |
| **MKCOL** | Ta metoda jest używana w połączeniu z protokołem WebDAV i służy do tworzenia kolekcji (np. katalogu) o określonym identyfikatorze URI. |
| **PROPFIND PROPPATCH** | Te dwie metody są używane z protokołem WebDAV i służą do wykonywania zapytań lub ustawiania właściwości identyfikatora URI. |
| **ODBLOKOWYWANIE BLOKADY** | Te dwie metody są używane w połączeniu z protokołem WebDAV i służą do blokowania/odblokowywania zasobu identyfikowanego przez identyfikator URI żądania podczas tworzenia. |
| **WYSŁANA** | Ta metoda służy do modyfikowania istniejącego zasobu HTTP. |

Jeśli jedna z tych metod HTTP jest skonfigurowana do użycia na serwerze, serwer odpowie ze stanem HTTP i innymi danymi, które są odpowiednie dla żądania. (Na przykład Metoda GET może odebrać odpowiedź HTTP 200 ***OK*** , a metoda Put może odebrać odpowiedź HTTP 201 ***utworzoną*** ).

Jeśli metoda HTTP nie jest skonfigurowana do użycia na serwerze, serwer odpowie z błędem ***zaimplementowanym*** http 501.

Jeśli jednak metoda HTTP jest skonfigurowana do użycia na serwerze, ale została wyłączona dla danego identyfikatora URI, serwer odpowie przy użyciu metody HTTP 405 ***niedozwolonego*** błędu.

## <a name="example-http-405-error"></a>Przykład błędu HTTP 405

W poniższym przykładzie żądanie HTTP i odpowiedź ilustrują sytuację, w której klient HTTP próbuje umieścić wartość w aplikacji interfejsu API sieci Web na serwerze sieci Web, a serwer zwraca błąd HTTP, który stwierdza, że metoda PUT nie jest dozwolona:

Żądanie HTTP:

[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]

Odpowiedź HTTP:

[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]

W tym przykładzie klient HTTP wysłał prawidłowe żądanie JSON do adresu URL aplikacji interfejsu API sieci Web na serwerze sieci Web, ale serwer zwrócił komunikat o błędzie HTTP 405, który wskazuje, że metoda PUT była niedozwolona dla adresu URL. Z drugiej strony, jeśli identyfikator URI żądania nie jest zgodny z trasą dla aplikacji interfejsu API sieci Web, serwer zwróci błąd ***nie znaleziono*** HTTP 404.

## <a name="resolve-http-405-errors"></a>Rozwiązywanie błędów HTTP 405

Istnieje kilka powodów, dla których może nie być dozwolone określone zlecenie HTTP, ale istnieje jeden główny scenariusz, który jest wiodącą przyczyną tego błędu w usługach IIS: wiele programów obsługi jest zdefiniowanych dla tego samego czasownika/metody, a jeden z programów obsługi blokuje oczekiwaną procedurę obsługi z przetwarzanie żądania. W wyniku wyjaśnienia program IIS przetwarza od początku do ostatniego na podstawie wpisów programu obsługi zamówień w plikach *ApplicationHost. config* i *Web. config* , gdzie pierwsza zgodna kombinacja ścieżki, czasowników, zasobów itp., będzie używana do obsługi żądanie.

Poniższy przykład to fragment z pliku *ApplicationHost. config* dla serwera IIS, który ZWRÓCIŁ błąd HTTP 405 podczas używania metody PUT do przesyłania danych do aplikacji internetowego interfejsu API. Na tym fragmencie są zdefiniowane kilka obsługi protokołu HTTP, a każdy program obsługi ma inny zestaw metod HTTP, dla których jest skonfigurowany — ostatni wpis na liście to procedura obsługi zawartości statycznej, która jest domyślnym programem obsługi, który jest używany po innych procedurach obsługi Chanc e aby przejrzeć żądanie:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

W poprzednim przykładzie procedura obsługi WebDAV i obsługa adresów URL bez rozszerzenia dla ASP.NET (który jest używany dla interfejsu API sieci Web) jest jasno zdefiniowana dla oddzielnych list metod HTTP. Należy pamiętać, że program obsługi DLL ISAPI jest skonfigurowany dla wszystkich metod HTTP, chociaż taka konfiguracja niekoniecznie spowoduje wystąpienie błędu. Jednak w przypadku rozwiązywania problemów z błędami HTTP 405 należy wziąć pod uwagę ustawienia konfiguracji, takie jak to konieczne.

W powyższym przykładzie nie wystąpił problem z programem obsługi DLL ISAPI. w rzeczywistości problem nie został zdefiniowany w pliku *ApplicationHost. config* serwera IIS — problem został spowodowany przez wpis wprowadzony w pliku *Web. config* podczas tworzenia aplikacji internetowego interfejsu API w programie Visual Studio. Poniższy fragment z pliku *Web. config* aplikacji przedstawia lokalizację problemu:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

W tym fragmencie program obsługi adresów URL bez rozszerzeń dla ASP.NET został ponownie zdefiniowany w celu uwzględnienia dodatkowych metod HTTP, które będą używane z aplikacją internetowego interfejsu API. Jednak ponieważ podobny zestaw metod HTTP jest zdefiniowany dla programu obsługi WebDAV, występuje konflikt. W tym konkretnym przypadku program obsługi WebDAV został zdefiniowany i załadowany przez usługi IIS, nawet jeśli protokół WebDAV jest wyłączony dla witryny sieci Web zawierającej aplikację internetowego interfejsu API. Podczas przetwarzania żądania HTTP PUT Usługa IIS wywołuje moduł WebDAV, ponieważ jest on zdefiniowany dla zlecenia PUT. Po wywołaniu modułu WebDAV sprawdza jego konfigurację i zauważa, że jest wyłączony, więc zwróci metodę HTTP 405 ***niedozwolony*** błąd dla każdego żądania podobnego do żądania WebDAV. Aby rozwiązać ten problem, należy usunąć WebDAV z listy modułów HTTP dla witryny sieci Web, w której jest zdefiniowana aplikacja internetowego interfejsu API. W poniższym przykładzie pokazano, co może wyglądać następująco:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

Ten scenariusz często występuje po opublikowaniu aplikacji ze środowiska programistycznego w środowisku produkcyjnym usług IIS. jest to spowodowane tym, że lista programów obsługi/modułów jest różna w środowiskach deweloperskich i produkcyjnych. Na przykład, jeśli używasz programu Visual Studio 2012 lub nowszego do tworzenia aplikacji internetowego interfejsu API, IIS Express jest domyślnym serwerem sieci Web na potrzeby testowania. Ten serwer sieci Web jest skalowanej wersji pełnej funkcji usług IIS, która jest dostarczana z produktem serwerowym, a ten serwer deweloperskich sieci Web zawiera kilka zmian, które zostały dodane do scenariuszy programistycznych. Na przykład moduł WebDAV jest często instalowany na produkcyjnym serwerze sieci Web, na którym działa pełna wersja usług IIS, chociaż może nie być w użyciu. Wersja deweloperskia usług IIS (IIS Express) zainstaluje moduł WebDAV, ale wpisy dla modułu WebDAV są celowo oznaczone jako komentarz, więc moduł WebDAV nigdy nie jest ładowany na IIS Express, chyba że użytkownik zmieni konfigurację IIS Express ustawienia umożliwiające dodanie funkcji WebDAV do instalacji IIS Express. W związku z tym aplikacja sieci Web może prawidłowo funkcjonować na komputerze deweloperskim, ale podczas publikowania aplikacji internetowego interfejsu API na serwerze produkcyjnym sieci Web usług IIS mogą wystąpić błędy HTTP 405.

## <a name="http-501-errors"></a>Błędy HTTP 501

* Wskazuje, że określone funkcje nie zostały zaimplementowane na serwerze.
* Zazwyczaj oznacza to, że w ustawieniach usług IIS nie ma zdefiniowanej procedury obsługi, która pasuje do żądania HTTP:
  * Prawdopodobnie wskazuje, że coś nie zostało poprawnie zainstalowane w usługach IIS lub
  * Coś zmodyfikowano ustawienia usług IIS tak, aby nie było zdefiniowanych programów obsługi, które obsługują określoną metodę HTTP.

Aby rozwiązać ten problem, należy ponownie zainstalować dowolną aplikację, która próbuje użyć metody HTTP, dla której nie ma odpowiednich modułów lub definicji programu obsługi.

## <a name="summary"></a>Podsumowanie

Błędy HTTP 405 są spowodowane błędami, gdy serwer sieci Web nie zezwala na dostęp do żądanego adresu URL. Ten stan jest często widoczny, gdy określony program obsługi został zdefiniowany dla konkretnego zlecenia, a program obsługi zastępują procedurę obsługi, która oczekuje na przetworzenie żądania.

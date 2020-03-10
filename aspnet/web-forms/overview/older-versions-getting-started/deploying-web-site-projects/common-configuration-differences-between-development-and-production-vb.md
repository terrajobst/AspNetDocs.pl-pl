---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
title: Typowe różnice konfiguracji między programowaniem a produkcją (VB) | Microsoft Docs
author: rick-anderson
description: We wcześniejszych samouczkach wdrożono naszą witrynę sieci Web przez skopiowanie wszystkich odpowiednich plików ze środowiska deweloperskiego do środowiska produkcyjnego. Jednak i...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 548e75f6-4d6c-4cb4-8da8-417915eb8393
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
msc.type: authoredcontent
ms.openlocfilehash: cc65af6eb4fca8b3b805e11e26da468a958a4221
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632294"
---
# <a name="common-configuration-differences-between-development-and-production-vb"></a>Typowe różnice konfiguracji między środowiskami deweloperskim i produkcyjnym (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz plik PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_vb.pdf)

> We wcześniejszych samouczkach wdrożono naszą witrynę sieci Web przez skopiowanie wszystkich odpowiednich plików ze środowiska deweloperskiego do środowiska produkcyjnego. Nie zdarza się jednak, że istnieją różnice w konfiguracji między środowiskami, co wymaga, aby każde środowisko miało unikatowy plik Web. config. Ten samouczek sprawdza typowe różnice w konfiguracji i analizuje strategie dotyczące obsługi osobnych informacji konfiguracyjnych.

## <a name="introduction"></a>Wprowadzenie

Ostatnie dwa samouczki przeprowadzą Cię przez wdrażanie prostej aplikacji sieci Web. W samouczku [*wdrażanie witryny przy użyciu klienta FTP*](deploying-your-site-using-an-ftp-client-vb.md) pokazano, jak używać autonomicznego klienta FTP do kopiowania niezbędnych plików ze środowiska deweloperskiego do produkcyjnego. Poprzedni samouczek, [*wdrażanie witryny przy użyciu programu Visual Studio*](deploying-your-site-using-visual-studio-vb.md), został przeszukany przy użyciu narzędzia kopiowania witryny sieci Web programu Visual Studio i opcji publikowania. W obu samouczkach każdy plik w środowisku produkcyjnym był kopią pliku w środowisku deweloperskim. Jednak w środowisku produkcyjnym nie jest nietypowa sytuacja, w której pliki konfiguracyjne są inne niż w środowisku programistycznym. Konfiguracja aplikacji sieci Web jest przechowywana w pliku `Web.config` i zwykle zawiera informacje o zasobach zewnętrznych, takich jak serwery baz danych, sieci Web i poczty e-mail. Sprawdza również zachowanie aplikacji w pewnych sytuacjach, takich jak sposób działania podejmowane w przypadku wystąpienia nieobsługiwanego wyjątku.

W przypadku wdrażania aplikacji sieci Web ważne jest, aby informacje o konfiguracji były kończyć się w środowisku produkcyjnym. W większości przypadków nie można skopiować pliku `Web.config` w środowisku deweloperskim do środowiska produkcyjnego. Zamiast tego dostosowana wersja `Web.config` musi zostać przekazana do środowiska produkcyjnego. W tym samouczku krótko opisano niektóre z bardziej typowych różnic konfiguracji; zawiera również podsumowanie niektórych technik utrzymywania różnych informacji konfiguracyjnych między środowiskami.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Typowe różnice konfiguracji między środowiskami deweloperskim i produkcyjnym

Plik `Web.config` obejmuje asortyment informacji o konfiguracji dla aplikacji ASP.NET. Niektóre z tych informacji konfiguracyjnych są takie same, niezależnie od środowiska. Na przykład ustawienia uwierzytelniania i reguły autoryzacji adresów URL są pisane w `<authentication>` pliku `Web.config` i `<authorization>` elementy są zwykle takie same niezależnie od środowiska. Inne informacje o konfiguracji, takie jak informacje o zasobach zewnętrznych — zwykle różnią się w zależności od środowiska.

Parametry połączeń bazy danych są przykładowymi informacjami o konfiguracji, które różnią się w zależności od środowiska. Gdy aplikacja sieci Web komunikuje się z serwerem bazy danych, musi najpierw nawiązać połączenie i osiągnąć za pomocą [parametrów połączenia](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Chociaż istnieje możliwość, że parametry połączenia bazy danych są kodowane bezpośrednio na stronach sieci Web lub w kodzie, który łączy się z bazą danych, najlepiej jest umieścić ją `Web.config`[elementu`<connectionStrings>`](https://msdn.microsoft.com/library/bf7sd233.aspx) , tak aby informacje o parametrach połączenia były w jednej lokalizacji scentralizowanej. Często inna baza danych jest używana podczas opracowywania, niż jest używana w środowisku produkcyjnym; w związku z tym informacje o parametrach połączenia muszą być unikatowe dla każdego środowiska.

> [!NOTE]
> W przyszłości samouczki eksplorują wdrażanie aplikacji opartych na danych, a w tym momencie będziemy szczegółowe do konkretnych metod przechowywania parametrów połączenia bazy danych w pliku konfiguracji.

Zamierzone zachowanie środowiska programistycznego i produkcyjnego różni się znacznie. Aplikacja sieci Web w środowisku programistycznym jest tworzona, przetestowana i debugowana przez małą grupę deweloperów. W środowisku produkcyjnym, które są używane przez wielu różnych równoczesnych użytkowników. ASP.NET zawiera szereg funkcji, które ułatwiają deweloperom testowanie i debugowanie aplikacji, ale te funkcje powinny być wyłączone ze względu na wydajność i bezpieczeństwo w środowisku produkcyjnym. Przyjrzyjmy się kilku takim ustawieniom konfiguracyjnym.

### <a name="configuration-settings-that-impact-performance"></a>Ustawienia konfiguracji mające wpływ na wydajność

Gdy strona ASP.NET jest odwiedzana po raz pierwszy (lub po raz pierwszy po zmianie), jego znaczniki deklaratywne muszą zostać przekonwertowane na klasę, a ta klasa musi być skompilowana. Jeśli aplikacja sieci Web używa automatycznej kompilacji, należy również skompilować także klasę z kodem związanym ze stroną. Można skonfigurować asortyment opcji kompilacji za pomocą [elementu`<compilation>`](https://msdn.microsoft.com/library/s10awwz0.aspx)pliku `Web.config`.

Atrybut Debug jest jednym z najważniejszych atrybutów elementu `<compilation>`. Jeśli atrybut `debug` ma wartość "true", skompilowane zestawy zawierają symbole debugowania, które są konieczne podczas debugowania aplikacji w programie Visual Studio. Ale symbole debugowania zwiększają rozmiar zestawu i nakładają dodatkowe wymagania dotyczące pamięci podczas uruchamiania kodu. Ponadto, gdy atrybut `debug` ma wartość "true", dowolna zawartość zwrócona przez `WebResource.axd` nie jest buforowana, co oznacza, że za każdym razem, gdy użytkownik odwiedzi stronę, będzie musiał ponownie pobrać zawartość statyczną zwróconą przez `WebResource.axd`.

> [!NOTE]
> `WebResource.axd` to wbudowana procedura obsługi HTTP wprowadzona w ASP.NET 2,0, która służy do pobierania zasobów osadzonych, takich jak pliki skryptów, obrazy, pliki CSS i inna zawartość. Aby uzyskać więcej informacji na temat działania `WebResource.axd` i sposobu korzystania z niego w celu uzyskania dostępu do zasobów osadzonych z niestandardowych kontrolek serwera, zobacz [Uzyskiwanie dostępu do zasobów osadzonych za pośrednictwem adresu URL przy użyciu `WebResource.axd`](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).

Atrybut `debug` elementu `<compilation>` jest zwykle ustawiony na wartość "true" w środowisku deweloperskim. W rzeczywistości ten atrybut musi być ustawiony na wartość "true" w celu debugowania aplikacji sieci Web; Jeśli spróbujesz debugować aplikację ASP.NET z poziomu programu Visual Studio, a atrybut `debug` ma wartość "false", w programie Visual Studio zostanie wyświetlony komunikat z informacją o tym, że aplikacja nie może być debugowana, dopóki atrybut `debug` nie zostanie ustawiony na wartość "true" i będzie oferować tę zmianę.

**Nie** należy mieć atrybutu `debug` ustawionego na wartość "true" w środowisku produkcyjnym ze względu na jego wpływ na wydajność. Dokładniejsze omówienie tego tematu znajdują się w blogu [Scott Guthrie](https://weblogs.asp.net/scottgu/), [nie uruchamiając w tym celu aplikacji produkcyjnych ASP.NET z włączonym `debug="true"`](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Błędy i śledzenie niestandardowe

Gdy wystąpił nieobsługiwany wyjątek w aplikacji ASP.NET, jest on bąbelky do środowiska uruchomieniowego, w którym następuje jeden z trzech rzeczy:

- Zostanie wyświetlony ogólny komunikat o błędzie środowiska uruchomieniowego. Ta strona informuje użytkownika, że wystąpił błąd w czasie wykonywania, ale nie zawiera szczegółowych informacji o błędzie.
- Zostanie wyświetlony komunikat szczegóły wyjątku, który zawiera informacje na temat zgłoszonego wyjątku.
- Zostanie wyświetlona strona błędu niestandardowego, która jest utworzoną stroną ASP.NET, w której jest wyświetlany dowolny żądany komunikat.

Co się dzieje w przypadku nieobsługiwanego wyjątku zależy od [sekcji`<customErrors>`](https://msdn.microsoft.com/library/h0hfz6fc.aspx)pliku `Web.config`.

Podczas tworzenia i testowania aplikacji pomocne jest wyświetlenie szczegółów dowolnego wyjątku w przeglądarce. Jednak pokazywanie szczegółowych informacji o wyjątkach w aplikacji w środowisku produkcyjnym jest potencjalnym zagrożeniem bezpieczeństwa. Co więcej, jest to unflattering i sprawia, że witryna sieci Web wygląda niezawodnie. Najlepiej, w przypadku nieobsłużonego wyjątku aplikacja sieci Web w środowisku deweloperskim będzie wyświetlać szczegóły wyjątku, podczas gdy ta sama aplikacja w produkcji będzie zawierać niestandardową stronę błędu.

> [!NOTE]
> Domyślne ustawienie sekcji `<customErrors>` pokazuje komunikat szczegóły wyjątku tylko wtedy, gdy strona jest odwiedzana za pomocą localhost, i w przeciwnym razie pokazuje ogólną stronę błędu czasu wykonywania. Nie jest to idealne rozwiązanie, ale zapewnia, że zachowanie domyślne nie ujawnia szczegółowych informacji o wyjątku dla odwiedzających nielokalnych. Przyszły samouczek sprawdza sekcję `<customErrors>` bardziej szczegółowo i pokazuje, jak ma być wyświetlana niestandardowa strona błędu w przypadku wystąpienia błędu w środowisku produkcyjnym.

Funkcja śledzenia jest używana przez inną funkcję ASP.NET. Śledzenie, jeśli ta funkcja jest włączona, rejestruje informacje o każdym żądaniu przychodzącym i udostępnia specjalną stronę sieci Web `Trace.axd`do wyświetlania ostatnich szczegółów żądania. Możesz włączyć i skonfigurować śledzenie za pomocą [elementu`<trace>`](https://msdn.microsoft.com/library/6915t83k.aspx) w `Web.config`.

Po włączeniu śledzenia upewnij się, że jest ono wyłączone w środowisku produkcyjnym. Ponieważ informacje o śledzeniu obejmują pliki cookie, dane sesji i inne potencjalnie poufne informacje, należy wyłączyć śledzenie w środowisku produkcyjnym. Dobrą wiadomością jest to, że funkcja śledzenia jest domyślnie wyłączona, a plik `Trace.axd` jest dostępny tylko za pomocą hosta lokalnego. Jeśli zmienisz te ustawienia domyślne podczas tworzenia, upewnij się, że są one wyłączone w środowisku produkcyjnym.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Techniki obsługi osobnych informacji konfiguracyjnych

Różne ustawienia konfiguracji w środowiskach deweloperskich i produkcyjnych komplikują proces wdrażania. W poprzednich dwóch samouczkach proces wdrażania dotyczył skopiowania wszystkich niezbędnych plików z programowania do produkcyjnego, ale takie podejście działa tylko wtedy, gdy informacje o konfiguracji są takie same w obu środowiskach. Istnieje wiele technik wdrażania aplikacji z różnymi informacjami o konfiguracji. Poinformujmy niektóre z tych opcji dla hostowanych aplikacji sieci Web.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Ręczne wdrażanie pliku konfiguracji środowiska produkcyjnego

Najprostszym podejściem jest zachowanie dwóch wersji pliku `Web.config`: jeden dla środowiska programistycznego i jeden dla środowiska produkcyjnego. Wdrożenie lokacji do środowiska produkcyjnego wiąże się z kopiowaniem wszystkich plików na serwer produkcyjny w środowisku programistycznym *z wyjątkiem* pliku `Web.config`. Zamiast tego plik `Web.config` specyficzny dla środowiska produkcyjnego zostanie skopiowany do środowiska produkcyjnego.

Takie podejście nie jest bardzo zaawansowane, ale jest łatwe do wdrożenia, ponieważ informacje o konfiguracji są rzadko zmieniane. Najlepiej sprawdza się w przypadku aplikacji z małym zespołem programistycznym hostowanym na jednym serwerze sieci Web, którego informacje o konfiguracji są rzadko zmieniane. Jest to najbardziej Tenable podczas ręcznego wdrażania plików aplikacji przy użyciu autonomicznego klienta FTP. W przypadku korzystania z narzędzia do kopiowania witryny sieci Web programu Visual Studio lub opcji publikowania należy najpierw wymienić plik `Web.config` specyficzny dla wdrożenia z konkretnym środowiskiem produkcyjnym przed wdrożeniem, a następnie wymienić je ponownie po zakończeniu wdrażania.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Zmiana konfiguracji w procesie kompilacji lub wdrożenia

Z tego względu dyskusje zakładają proces kompilowania i wdrażania ad hoc. Wiele większych projektów oprogramowania ma więcej formalnych procesów korzystających z narzędzi typu open source, opartych na domu lub innych firm. W przypadku takich projektów można prawdopodobnie dostosować proces kompilacji lub wdrożenia, aby odpowiednio zmodyfikować informacje o konfiguracji przed ich wypchnięciem do środowiska produkcyjnego. Jeśli tworzysz aplikację sieci Web przy użyciu programu [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [NAnt](http://nant.sourceforge.net/)lub innego narzędzia do kompilacji, możesz dodać krok kompilacji, aby zmodyfikować plik `Web.config`, aby uwzględnić ustawienia specyficzne dla produkcji. Lub przepływ pracy wdrożenia może programowo połączyć się z serwerem kontroli źródła i pobrać odpowiedni plik `Web.config`.

Rzeczywiste podejście do uzyskiwania odpowiednich informacji konfiguracyjnych do środowiska produkcyjnego różni się znacznie w zależności od narzędzi i przepływu pracy. W związku z tym nie będziemy w dalszej części w tym temacie. Jeśli używasz popularnego narzędzia do kompilacji, takiego jak MSBuild lub NAnt, możesz znaleźć artykuły i samouczki dotyczące wdrażania odpowiednie dla tych narzędzi za pośrednictwem wyszukiwania w sieci Web.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Zarządzanie różnicami w konfiguracji za pomocą dodatku projektu wdrożenia sieci Web

W 2006 firmie Microsoft wydano dodatek projektowy Web Development dla programu Visual Studio 2005. Dodatek dla programu Visual Studio 2008 został opublikowany w 2008. Ten dodatek pozwala deweloperom ASP.NET utworzyć oddzielny projekt wdrożenia sieci Web wraz z projektem aplikacji sieci Web, który po skompilowaniu jawnie kompiluje aplikację sieci Web i kopiuje pliki do wdrożenia w lokalnym katalogu wyjściowym. Projekt aplikacji sieci Web używa programu MSBuild w tle.

Domyślnie plik `Web.config` środowiska programistycznego jest kopiowany do katalogu wyjściowego, ale projekt wdrożenia sieci Web można skonfigurować w celu dostosowania

Informacje o konfiguracji, które są kopiowane do tego katalogu w następujący sposób:

- Za pomocą `Web.config` zastąpienie sekcji pliku, w którym należy określić sekcję do zastąpienia i plik XML, który zawiera tekst zastępczy.
- Podając ścieżkę do pliku źródłowego konfiguracji zewnętrznej. Po wybraniu tej opcji projekt wdrożenia sieci Web kopiuje określony plik `Web.config` do katalogu wyjściowego (a nie plik `Web.config` używany w środowisku programistycznym).
- Dodając niestandardowe reguły do pliku programu MSBuild używanego przez projekt wdrażania w sieci Web.

Aby wdrożyć aplikację sieci Web, Skompiluj projekt wdrożenia sieci Web, a następnie skopiuj pliki z folderu wyjściowego projektu do środowiska produkcyjnego.

Aby dowiedzieć się więcej o korzystaniu z projektu wdrażania sieci Web, zapoznaj się z artykułem [dotyczącym](https://msdn.microsoft.com/magazine/default.aspx) [projektów wdrażania sieci web](https://msdn.microsoft.com/magazine/cc163448.aspx) z kwietnia 2007.

> [!NOTE]
> Nie można użyć projektu wdrożenia sieci Web z programem Visual Web Developer, ponieważ projekt wdrożenia sieci Web jest zaimplementowany jako dodatek programu Visual Studio i wersje Visual Studio Express (w tym Visual Web Developer) nie obsługują dodatków.

## <a name="summary"></a>Podsumowanie

Zasoby zewnętrzne i zachowanie aplikacji sieci Web w trakcie opracowywania są zwykle inne niż w przypadku, gdy ta sama aplikacja jest w środowisku produkcyjnym. Na przykład parametry połączenia bazy danych, opcje kompilacji i zachowanie w przypadku wystąpienia nieobsłużonego wyjątku często różnią się między środowiskami. Proces wdrażania musi uwzględniać te różnice. Zgodnie z opisem w tym samouczku najprostszym podejściem jest ręczne skopiowanie alternatywnego pliku konfiguracji do środowiska produkcyjnego. W przypadku korzystania z dodatku projektu wdrażania w sieci Web lub z bardziej formalnym procesem kompilowania i wdrażania, który może obsłużyć te dostosowania, możliwe są bardziej elegancki rozwiązania.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Wyjaśniono parametry połączenia](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [Parametry połączenia z bazą danych @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Nie uruchamiaj aplikacji ASP.NET produkcyjnych z włączonym `debug="true"`](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Bezpieczne reagowanie na Nieobsłużone wyjątki — wyświetlanie przyjaznych dla użytkownika stron błędów](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Jak: użyć projektu programu Visual Studio 2008 Web Deployment?](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Ustawienia konfiguracji klucza podczas wdrażania bazy danych](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- Pobieranie [projektów wdrażania internetowego programu Visual studio 2008](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [programu Visual Studio 2005 pobieranie projektów wdrażania w sieci Web](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Projekty wdrażania w sieci Web vs 2008](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [programu vs 2008 Web Deployment](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Projekty wdrażania w Internecie](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [Poprzednie](deploying-your-site-using-visual-studio-vb.md)
> [dalej](core-differences-between-iis-and-the-asp-net-development-server-vb.md)

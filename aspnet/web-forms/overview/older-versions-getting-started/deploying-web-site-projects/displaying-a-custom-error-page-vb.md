---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
title: Wyświetlanie niestandardowej strony błędu (VB) | Microsoft Docs
author: rick-anderson
description: Co widzi użytkownik, gdy wystąpi błąd w czasie wykonywania w aplikacji sieci Web ASP.NET? Odpowiedź zależy od konfiguracji&gt; &lt;customErrors witryny sieci Web...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 14873c5d-81a9-455b-bd71-30fb555583e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 33367d5e3c4b5c8fa039ee20704054ba508e717a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614961"
---
# <a name="displaying-a-custom-error-page-vb"></a>Wyświetlanie niestandardowej strony błędu (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_vb.pdf)

> Co widzi użytkownik, gdy wystąpi błąd w czasie wykonywania w aplikacji sieci Web ASP.NET? Odpowiedź zależy od konfiguracji&gt; &lt;customErrors witryny sieci Web. Domyślnie użytkownicy są wyświetlani z nieszczegółowym ekranem, który zażądał błędu w czasie wykonywania. W tym samouczku pokazano, jak dostosować te ustawienia, aby wyświetlić stronę błędu niestandardowego atrakcyjne, która pasuje do wyglądu i działania Twojej witryny.

## <a name="introduction"></a>Wprowadzenie

W idealnym świecie nie będzie żadnych błędów w czasie wykonywania. Programiści mogą napisać kod z nary błędu i z niezawodną walidacją danych wejściowych użytkownika, a zasoby zewnętrzne, takie jak serwery baz danych i serwery poczty e-mail, nigdy nie przechodzą do trybu offline. Oczywiście błędy w rzeczywistości są nieuniknione. Klasy w .NET Framework sygnalizujący błąd przez zgłaszanie wyjątku. Na przykład wywołanie metody Open obiektu SqlConnection nawiązuje połączenie z bazą danych określoną przez parametry połączenia. Jeśli jednak baza danych nie działa lub jeśli poświadczenia w parametrach połączenia są nieprawidłowe, Metoda Otwórz zgłosi `SqlException`. Wyjątki mogą być obsługiwane przez użycie bloków `Try/Catch/Finally`. Jeśli kod w bloku `Try` zgłasza wyjątek, formant zostanie przeniesiony do odpowiedniego bloku catch, w którym deweloper może próbować odzyskać sprawność po błędzie. Jeśli nie ma pasującego bloku catch lub kod, który zgłosił wyjątek, nie znajduje się w bloku try, wyjątek percolates stos wywołań w wyszukiwaniu bloków `Try/Catch/Finally`.

Jeśli wyjątek bąbelkowy cały sposób do środowiska uruchomieniowego ASP.NET bez obsługi, zostanie wywołane [zdarzenie`Error`](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx) [klasy`HttpApplication`](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)i zostanie wyświetlona skonfigurowana *strona błędu* . Domyślnie ASP.NET wyświetla stronę błędu, która jest affectionately, określana jako [żółty ekran zgonu](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Istnieją dwie wersje YSOD: jeden pokazuje szczegóły wyjątku, ślad stosu i inne informacje pomocne dla deweloperów debugujący aplikację (patrz **rysunek 1**); Druga po prostu wskazuje, że wystąpił błąd czasu wykonywania (patrz **rysunek 2**).

Szczegóły wyjątku YSOD są bardzo pomocne w przypadku deweloperów, którzy debugują aplikację, ale przedstawiają YSOD użytkownikom końcowym. Zamiast tego użytkownicy końcowi powinni zostać przekierowani do strony błędu, która utrzymuje wygląd witryny i będzie działać z większą liczbą Prose przyjaznych dla użytkownika opisującą sytuację. Dobrym komunikatem jest to, że tworzenie takiej niestandardowej strony błędu jest bardzo proste. Ten samouczek rozpoczyna się od strony ASP. Różne strony błędów w sieci. Następnie pokazano, jak skonfigurować aplikację sieci Web, aby pokazywała użytkownikom niestandardową stronę błędu w przypadku błędu.

### <a name="examining-the-three-types-of-error-pages"></a>Badanie trzech typów stron błędów

Gdy wystąpi nieobsługiwany wyjątek w aplikacji ASP.NET, zostanie wyświetlony jeden z trzech typów stron błędu:

- Żółty ekran Szczegóły wyjątku strony błędu śmierci,
- Żółty ekran błędu środowiska uruchomieniowego strony błędu śmierci lub
- Niestandardowa strona błędu

Najbardziej popularnym projektantem strony błędów są szczegóły wyjątku YSOD. Domyślnie ta strona jest wyświetlana użytkownikom, którzy znajdują się lokalnie, i w związku z tym jest stroną, która jest wyświetlana, gdy wystąpi błąd podczas testowania lokacji w środowisku deweloperskim. Ponieważ jego nazwa to oznacza, szczegóły wyjątku YSOD zawierają szczegóły dotyczące wyjątku — typu, komunikatu i śladu stosu. Co więcej, jeśli wyjątek został zgłoszony przez kod w klasie z kodem ASP.NET strony i jeśli aplikacja jest skonfigurowana do debugowania, YSOD szczegóły wyjątku również pokażą ten wiersz kodu (oraz kilka wierszy kodu powyżej i poniżej niego).

**Rysunek 1** przedstawia stronę YSOD szczegóły wyjątku. Zanotuj adres URL w oknie adresu przeglądarki: `http://localhost:62275/Genre.aspx?ID=foo`. Odwołaj się, że strona `Genre.aspx` wyświetla przeglądy książki w określonym gatunku. Wymaga, aby wartość `GenreId` (`uniqueidentifier`) była przenoszona przez ciąg QueryString; na przykład odpowiedni adres URL służący do wyświetlania recenzji fikcyjnej jest `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Jeśli wartość nie`uniqueidentifier` jest przenoszona za pomocą ciągu QueryString (na przykład "foo"), zgłaszany jest wyjątek.

> [!NOTE]
> Aby odtworzyć ten błąd w demonstracyjnej aplikacji sieci Web dostępnej do pobrania, możesz albo odwiedzić `Genre.aspx?ID=foo` bezpośrednio, albo kliknąć link "Generuj błąd w czasie wykonywania" w `Default.aspx`.

Zanotuj informacje o wyjątku przedstawione na **rysunku 1**. Komunikat o wyjątku "Konwersja ciągu znaków na unikatowy identyfikator" nie powiodła się w górnej części strony. Typ wyjątku, `System.Data.SqlClient.SqlException`, jest również wyświetlany. Istnieje również ślad stosu.

[![](displaying-a-custom-error-page-vb/_static/image2.png)](displaying-a-custom-error-page-vb/_static/image1.png)

**Rysunek 1**. Szczegóły wyjątku YSOD zawierają informacje o wyjątku  
 ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-a-custom-error-page-vb/_static/image3.png))

Innym typem YSOD jest błąd czasu wykonywania YSOD i przedstawiono na **rysunku 2**. Błąd czasu wykonywania YSOD informuje gościa, że wystąpił błąd w czasie wykonywania, ale nie zawiera żadnych informacji o zgłoszonym wyjątku. (Zawiera jednak instrukcje dotyczące wyświetlania szczegółów błędu poprzez modyfikację pliku `Web.config`, który jest częścią tego, co sprawia, że YSOD wyglądać inaczej).

Domyślnie YSOD błędów środowiska uruchomieniowego jest pokazywany użytkownikom zdalnie (za pomocą http://www.yoursite.com), co zostało potwierdzone przez adres URL na pasku adresu przeglądarki na **rysunku 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Istnieją dwa różne ekrany YSOD, ponieważ deweloperzy chcą znać szczegóły błędu, ale takie informacje nie powinny być wyświetlane w działającej witrynie, ponieważ mogą one ujawnić potencjalne luki w zabezpieczeniach lub inne poufne informacje osobom, które odwiedza lokacji.

> [!NOTE]
> Jeśli korzystasz z usługi DiscountASP.NET jako hosta sieci Web, możesz zauważyć, że błąd czasu wykonywania YSOD nie jest wyświetlany podczas odwiedzania aktywnej witryny. Wynika to z faktu, że serwery DiscountASP.NET są skonfigurowane tak, aby domyślnie pokazywały szczegóły wyjątku YSOD. Dobrymi wiadomościami jest możliwość zastąpienia tego zachowania domyślnego przez dodanie sekcji `<customErrors>` do pliku `Web.config`. Sekcja "Konfigurowanie, która strona błędu jest wyświetlana" — szczegółowe informacje zawiera sekcja `<customErrors>`.

[![](displaying-a-custom-error-page-vb/_static/image5.png)](displaying-a-custom-error-page-vb/_static/image4.png)

**Rysunek 2**: błąd czasu wykonywania YSOD nie zawiera żadnych szczegółów błędu  
 ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-a-custom-error-page-vb/_static/image6.png))

Trzeci typ strony błędu to niestandardowa strona błędu, która jest utworzoną przez Ciebie stroną sieci Web. Zaletą niestandardowej strony błędu jest to, że masz pełną kontrolę nad informacjami wyświetlanymi dla użytkownika wraz z wyglądem i działaniem strony; Strona błędu niestandardowego może używać tej samej strony wzorcowej i stylów co inne strony. Sekcja "Używanie niestandardowej strony błędu" zawiera instrukcje tworzenia niestandardowej strony błędu i konfigurowania jej do wyświetlania w zdarzeniu nieobsłużonego wyjątku. **Rysunek 3** oferuje zobaczyć szczyt tej niestandardowej strony błędu. Jak widać, wygląd i działanie strony błędu jest znacznie bardziej profesjonalne — jest to możliwe, ponieważ nie ma żadnego żółtego ekranu zgonu pokazanego na rysunkach 1 i 2.

[![](displaying-a-custom-error-page-vb/_static/image8.png)](displaying-a-custom-error-page-vb/_static/image7.png)

**Rysunek 3**. niestandardowa strona błędu oferuje bardziej dostosowany wygląd i działanie  
 ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-a-custom-error-page-vb/_static/image9.png))

Poświęć chwilę na sprawdzenie paska adresu w przeglądarce na **rysunku 3**. Należy zauważyć, że na pasku adresu jest wyświetlany adres URL strony błędu niestandardowego (`/ErrorPages/Oops.aspx`). Na rysunkach 1 i 2 żółte ekrany zgonu są wyświetlane na tej samej stronie, z której pochodzi błąd (`Genre.aspx`). Strona błędu niestandardowego jest przenoszona na adres URL strony, na której wystąpił błąd za pośrednictwem parametru `aspxerrorpath` QueryString.

## <a name="configuring-which-error-page-is-displayed"></a>Konfigurowanie, która strona błędu jest wyświetlana

Zostanie wyświetlona, która z trzech możliwych stron błędu jest oparta na dwóch zmiennych:

- Informacje o konfiguracji w sekcji `<customErrors>` i
- Czy użytkownik odwiedzający witrynę lokalnie czy zdalnie.

[Sekcja`<customErrors>`](https://msdn.microsoft.com/library/h0hfz6fc.aspx) w `Web.config` ma dwa atrybuty, które mają wpływ na zawartość wyświetlaną na stronie błędu: `defaultRedirect` i `mode`. Atrybut `defaultRedirect` jest opcjonalny. Jeśli ta wartość jest określona, określa adres URL strony błędu niestandardowego i wskazuje, że zamiast błędu czasu wykonywania należy wyświetlić stronę błędu niestandardowego. Atrybut `mode` jest wymagany i akceptuje jedną z trzech wartości: `On`, `Off`lub `RemoteOnly`. Te wartości mają następujące zachowanie:

- `On` — wskazuje, że strona błędu niestandardowego lub błąd czasu wykonania YSOD jest pokazywana wszystkim odwiedzającym, niezależnie od tego, czy są one lokalne, czy zdalne.
- `Off`-określa, że YSOD szczegóły wyjątku są wyświetlane dla wszystkich odwiedzających, niezależnie od tego, czy są one lokalne, czy zdalne.
- `RemoteOnly` — wskazuje, że niestandardowa strona błędu lub błąd czasu wykonywania YSOD jest pokazywany zdalnym Gościom, podczas gdy szczegóły wyjątku YSOD są widoczne dla Gości lokalnych.

O ile nie określono inaczej, ASP.NET działa tak, jakby atrybut Mode został ustawiony na `RemoteOnly` i nie został określony `defaultRedirect` wartość. Innymi słowy, domyślne zachowanie polega na tym, że szczegóły wyjątku YSOD są wyświetlane odwiedzającym lokalnego, podczas gdy błąd czasu wykonywania YSOD jest pokazywany zdalnym osobom odwiedzającym. To zachowanie domyślne można przesłonić, dodając sekcję `<customErrors>` do `Web.config file.` aplikacji sieci Web

## <a name="using-a-custom-error-page"></a>Używanie niestandardowej strony błędu

Każda aplikacja sieci Web powinna mieć niestandardową stronę błędu. Zapewnia to bardziej profesjonalne, niezawodne rozwiązanie YSOD błędów środowiska uruchomieniowego, można łatwo utworzyć i skonfigurować aplikację do korzystania z niestandardowej strony błędu zajmuje zaledwie kilka minut. Pierwszy krok polega na utworzeniu niestandardowej strony błędu. Dodaliśmy nowy folder do aplikacji przeglądający książki o nazwie `ErrorPages` i dodany do nowej strony ASP.NET o nazwie `Oops.aspx`. Użyj tej samej strony wzorcowej jako reszty stron w witrynie, aby automatycznie dziedziczyć ten sam wygląd i działanie.

[![](displaying-a-custom-error-page-vb/_static/image11.png)](displaying-a-custom-error-page-vb/_static/image10.png)

**Ilustracja 4**. Tworzenie niestandardowej strony błędu

Następnie Poświęć kilka minut na utworzenie zawartości dla strony błędu. Utworzono nieprostą niestandardową stronę błędu z komunikatem informującym o nieoczekiwanym błędzie i linku z powrotem do strony głównej witryny.

[![](displaying-a-custom-error-page-vb/_static/image13.png)](displaying-a-custom-error-page-vb/_static/image12.png)

**Rysunek 5**. Projektowanie niestandardowej strony błędu  
 ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-a-custom-error-page-vb/_static/image14.png))

Gdy strona błędu została ukończona, skonfiguruj aplikację sieci Web tak, aby korzystała ze strony błędu niestandardowego zamiast YSOD błędów środowiska uruchomieniowego. W tym celu należy określić adres URL strony błędu w atrybucie `defaultRedirect` `<customErrors>` sekcji. Dodaj następujący znacznik do pliku `Web.config` aplikacji:

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample1.xml)]

Powyższy znacznik służy do konfigurowania aplikacji do wyświetlania szczegółów wyjątku YSOD użytkownikom lokalnie, podczas gdy przy użyciu niestandardowej strony błędu niestety. aspx dla użytkowników odwiedzanych zdalnie. Aby wyświetlić tę akcję, wdróż witrynę internetową w środowisku produkcyjnym, a następnie odwiedź stronę gatunek. aspx w witrynie na żywo z nieprawidłową wartością QueryString. Powinna zostać wyświetlona strona błędu niestandardowego (zapoznaj się z powrotem do **rysunku 3**).

Aby sprawdzić, czy strona błędu niestandardowego jest wyświetlana tylko dla użytkowników zdalnych, odwiedź stronę `Genre.aspx` z nieprawidłowym wyrażeniem QueryString ze środowiska deweloperskiego. Nadal powinny zostać wyświetlone szczegóły wyjątku YSOD (zapoznaj się z powrotem do **rysunku 1**). Ustawienie `RemoteOnly` gwarantuje, że użytkownicy odwiedzający witrynę w środowisku produkcyjnym zobaczą stronę błędu niestandardowego, podczas gdy deweloperzy pracujący lokalnie nadal widzą szczegóły wyjątku.

## <a name="notifying-developers-and-logging-error-details"></a>Powiadamianie deweloperów i szczegóły błędu rejestrowania

Błędy występujące w środowisku programistycznym były spowodowane przez dewelopera na komputerze. Pokazuje informacje o wyjątku w YSOD szczegóły wyjątku i wie, jakie kroki były wykonywane po wystąpieniu błędu. Ale jeśli wystąpi błąd w środowisku produkcyjnym, Deweloper nie ma informacji o tym, że wystąpił błąd, chyba że użytkownik końcowy przejdzie do witryny, aby zgłosić błąd. Nawet jeśli użytkownik wyjdzie ze swojego sposobu, aby ostrzec zespół programistyczny, że wystąpił błąd, bez znajomości typu wyjątku, komunikatu i śladu stosu, może być trudne zdiagnozowanie przyczyny błędu, samodzielne rozwiązanie tego problemu.

Z tego względu należy zastanowić się, że każdy błąd w środowisku produkcyjnym jest rejestrowany w magazynie trwałym (takim jak baza danych) i że deweloperzy są powiadamiani o tym błędzie. Niestandardowa strona błędu może wydawać się dobrym miejscem do tego rejestrowania i powiadamiania. Niestety, niestandardowa strona błędu nie ma dostępu do szczegółów błędu i dlatego nie można jej użyć do rejestrowania tych informacji. Dobrą wiadomośćą jest to, że istnieją różne sposoby przechwycenia szczegółów błędu i zarejestrowania ich, a następne trzy samouczki szczegółowo poznają ten temat.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>Używanie różnych niestandardowych stron błędów dla różnych stanów błędów HTTP

Gdy wyjątek jest zgłaszany przez stronę ASP.NET i nie jest obsługiwany, wyjątek percolates do środowiska uruchomieniowego ASP.NET, który wyświetla skonfigurowaną stronę błędu. Jeśli żądanie znajduje się w aparacie ASP.NET, ale nie można go przetworzyć z jakiegoś powodu — prawdopodobnie nie znaleziono żądanego pliku lub uprawnienia do odczytu zostały wyłączone dla tego pliku, a następnie aparat ASP.NET wywołuje `HttpException`. Ten wyjątek, taki jak wyjątki wywoływane ze stron ASP.NET, bąbelki do środowiska uruchomieniowego, co powoduje wyświetlenie odpowiedniej strony błędu.

Znaczenie tej aplikacji sieci Web w środowisku produkcyjnym polega na tym, że jeśli użytkownik zażąda strony, która nie została znaleziona, zostanie wyświetlona strona błędu niestandardowego. **Rysunek 6** pokazuje taki przykład. Ponieważ żądanie dotyczy nieistniejącej strony (`NoSuchPage.aspx`), zgłaszany jest `HttpException` i zostanie wyświetlona strona błędu niestandardowego (należy zwrócić uwagę do `NoSuchPage.aspx` w `aspxerrorpath` ciągu QueryString).

[![](displaying-a-custom-error-page-vb/_static/image16.png)](displaying-a-custom-error-page-vb/_static/image15.png) **rysunek 6**: środowisko uruchomieniowe ASP.NET wyświetla skonfigurowaną stronę błędu w odpowiedzi na nieprawidłowe żądanie  
 ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-a-custom-error-page-vb/_static/image17.png)) 

Domyślnie wszystkie typy błędów powodują wyświetlenie tej samej niestandardowej strony błędu. Można jednak określić inną niestandardową stronę błędów dla określonego kodu stanu HTTP przy użyciu elementów podrzędnych `<error>` w sekcji `<customErrors>`. Na przykład aby w zdarzeniu nie znaleziono strony o błędzie o innej stronie, która ma kod stanu HTTP 404, zaktualizuj sekcję `<customErrors>`, aby uwzględnić następujące znaczniki:

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample2.xml)]

Po dokonaniu tej zmiany, za każdym razem, gdy użytkownik odwiedzający zdalnie żąda zasobu ASP.NET, który nie istnieje, zostanie przekierowany `404.aspx` do niestandardowej strony błędu, a nie `Oops.aspx`. Jak pokazano na **rysunku 7** , Strona `404.aspx` może zawierać bardziej konkretny komunikat niż ogólna strona błędu niestandardowego.

> [!NOTE]
> Zapoznaj [się ze stronami błędów 404 jeszcze raz,](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) Aby uzyskać wskazówki dotyczące tworzenia efektywnych stron błędów 404.

[![](displaying-a-custom-error-page-vb/_static/image19.png)](displaying-a-custom-error-page-vb/_static/image18.png) **rysunek 7**: Strona błędu niestandardowego 404 wyświetla bardziej skierowany komunikat niż `Oops.aspx`  
 ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](displaying-a-custom-error-page-vb/_static/image20.png)) 

Ponieważ wiesz, że strona `404.aspx` zostanie osiągnięta tylko wtedy, gdy użytkownik wysyła żądanie dotyczące strony, która nie została znaleziona, można rozszerzyć Tę stronę błędu niestandardowego w celu dołączenia funkcji, aby pomóc użytkownikowi rozwiązać ten konkretny typ błędu. Można na przykład utworzyć tabelę bazy danych, która mapuje znane złe adresy URL na dobre adresy URL, a następnie `404.aspx` stronie błędu niestandardowego uruchom zapytanie względem tej tabeli i Sugeruj strony, do których użytkownik może próbować uzyskać dostęp.

> [!NOTE]
> Strona błędu niestandardowego jest wyświetlana tylko wtedy, gdy żądanie jest wykonywane do zasobu obsługiwanego przez aparat ASP.NET. Zgodnie z opisem w [podstawowych różnicach między usługami IIS a samouczkiem serwera ASP.NET Development](core-differences-between-iis-and-the-asp-net-development-server-vb.md) serwer sieci Web może obsługiwać pewne żądania. Domyślnie serwer sieci Web usług IIS przetwarza żądania dotyczące zawartości statycznej, takiej jak obrazy i pliki HTML bez wywoływania aparatu ASP.NET. W związku z tym, jeśli użytkownik zażąda nieistniejącego pliku obrazu, spowoduje to przywrócenie domyślnego komunikatu o błędzie 404 programu IIS zamiast ASP. Skonfigurowana strona błędu sieci.

## <a name="summary"></a>Podsumowanie

Gdy wystąpił nieobsługiwany wyjątek w aplikacji ASP.NET, użytkownik zostanie pokazany na jednej z trzech stron błędu: Szczegóły wyjątku żółty ekran zgonu; Żółty ekran błędu środowiska uruchomieniowego w czasie wykonywania; lub niestandardową stronę błędu. Wyświetlana strona błędu zależy od konfiguracji `<customErrors>` aplikacji oraz tego, czy użytkownik jest odwiedzany lokalnie, czy zdalnie. Domyślnym zachowaniem jest wyświetlenie szczegółów wyjątku YSOD do lokalnych osób odwiedzających i błędu czasu wykonywania YSOD do zdalnych osób odwiedzających.

Mimo że błąd czasu wykonywania YSOD powoduje ukrycie potencjalnie poufnych informacji o błędach od użytkownika odwiedzającego witrynę, przerwanie od wyglądu i działania witryny oraz sprawia, że aplikacja działa poprawnie. Lepszym rozwiązaniem jest użycie niestandardowej strony błędu, która wiąże się z tworzeniem i projektowaniem niestandardowej strony błędu i określeniu jej adresu URL w atrybucie `defaultRedirect` `<customErrors>` sekcji. Można nawet mieć wiele niestandardowych stron błędów dla różnych stanów błędów HTTP.

Niestandardowa strona błędu to pierwszy krok kompleksowej strategii obsługi błędów dla witryny sieci Web w środowisku produkcyjnym. Należy również pamiętać, aby poinformować dewelopera o błędzie i zalogować się jego szczegóły. Następne trzy samouczki eksplorują techniki powiadamiania o błędach i rejestrowaniu.

Szczęśliwe programowanie!

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Strony błędów, jeszcze raz](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Wyjątki — zalecenia dotyczące projektowania](https://msdn.microsoft.com/library/ms229014.aspx)
- [Strony błędów przyjaznych dla użytkownika](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Obsługa i zgłaszanie wyjątków](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [Poprawne używanie niestandardowych stron błędów w programie ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [Poprzednie](strategies-for-database-development-and-deployment-vb.md)
> [dalej](processing-unhandled-exceptions-vb.md)

---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
title: Konfiguracja uwierzytelniania formularzy i Tematy zaawansowane (VB) | Microsoft Docs
author: rick-anderson
description: W tym samouczku sprawdzimy różne ustawienia uwierzytelniania formularzy i zobaczysz, jak modyfikować je za pomocą elementu Forms. Będzie to wymagać szczegółowego...
ms.author: riande
ms.date: 01/14/2008
ms.assetid: 829d2f56-5c48-445b-b826-3418a450c788
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
msc.type: authoredcontent
ms.openlocfilehash: 4d77816a489a4fa16cd70ec4214cd2f8ee563029
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74632006"
---
# <a name="forms-authentication-configuration-and-advanced-topics-vb"></a>Konfiguracja uwierzytelniania formularzy i tematy zaawansowane (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_vb.pdf)

> W tym samouczku sprawdzimy różne ustawienia uwierzytelniania formularzy i zobaczysz, jak modyfikować je za pomocą elementu Forms. Spowoduje to szczegółowe dostosowanie wartości limitu czasu biletu uwierzytelniania formularzy przy użyciu strony logowania z niestandardowym adresem URL (na przykład Signer. aspx zamiast login. aspx) i biletów uwierzytelniania formularzy bez plików cookie.

## <a name="introduction"></a>Wprowadzenie

W [poprzednim samouczku](an-overview-of-forms-authentication-vb.md) zawarto kroki niezbędne do wdrożenia uwierzytelniania formularzy w aplikacji ASP.NET, od określania ustawień konfiguracji w pliku Web. config w celu utworzenia strony logowania do wyświetlania innej zawartości dla użytkowników uwierzytelnionych i anonimowych. Odwołaj, że skonfigurowano witrynę sieci Web tak, aby korzystała z uwierzytelniania formularzy, ustawiając atrybut Mode elementu &lt;Authentication&gt; na Forms. Element&gt; Authentication &lt;może opcjonalnie zawierać &lt;formularzy&gt; elementu podrzędnego, za pomocą którego można określić asortyment ustawień uwierzytelniania formularzy.

W tym samouczku sprawdzimy różne ustawienia uwierzytelniania formularzy i zobaczysz, jak je modyfikować za pomocą &lt;formularzy&gt; elementu. Spowoduje to szczegółowe dostosowanie wartości limitu czasu biletu uwierzytelniania formularzy przy użyciu strony logowania z niestandardowym adresem URL (na przykład Signer. aspx zamiast login. aspx) i biletów uwierzytelniania formularzy bez plików cookie. Dodatkowo sprawdzimy korzeń biletu uwierzytelniania formularzy i zobaczysz, że ASP.NET wykonuje środki ostrożności, aby upewnić się, że dane biletu są zabezpieczone przed inspekcją i manipulacją. Na koniec dowiesz się, jak przechowywać dodatkowe dane użytkownika w biletach uwierzytelniania formularzy oraz jak modelować te dane za pomocą niestandardowego obiektu podmiotu zabezpieczeń.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Krok 1. Badanie &lt;formularzy&gt; ustawień konfiguracji

System uwierzytelniania formularzy w programie ASP.NET oferuje kilka ustawień konfiguracji, które można dostosować w zależności od aplikacji. Obejmuje to następujące ustawienia: okres istnienia biletu uwierzytelniania formularzy; jakiego rodzaju ochrony dotyczy bilet. w obszarze jakich warunków są używane bilety uwierzytelniania bez plików cookie; ścieżka do strony logowania; i inne informacje. Aby zmodyfikować wartości domyślne, Dodaj [&lt;formularzy&gt; elementu](https://msdn.microsoft.com/library/1d3t3c61.aspx) jako element podrzędny [&gt; uwierzytelniania&lt;elementu](https://msdn.microsoft.com/library/532aee0e.aspx), określając te wartości właściwości, które chcesz dostosować jako atrybuty XML, takie jak:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample1.xml)]

Tabela 1 podsumowuje właściwości, które można dostosować za pomocą &lt;formularzy&gt; elementu. Ponieważ plik Web. config jest plikiem XML, nazwy atrybutów w lewej kolumnie są rozróżniane wielkości liter.

| <strong>Atrybut</strong> |                                                                                                                                                                                                                                     <strong>Opis</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         cookieless         |                                                                                                                Ten atrybut określa, w jakich warunkach bilet uwierzytelniania jest przechowywany w pliku cookie, a w przeciwieństwie do jego osadzenia w adresie URL. Dozwolone wartości to: UseCookies; UseUri; Autowykrywanie i UseDeviceProfile (domyślnie). Krok 2 — sprawdzenie tego ustawienia bardziej szczegółowo.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Wskazuje adres URL, do którego użytkownicy są przekierowywani po zalogowaniu się ze strony logowania, jeśli nie określono wartości RedirectUrl w ciągu QueryString. Wartość domyślna to default. aspx.                                                                                                                                                         |
|           domena           | W przypadku korzystania z biletów uwierzytelniania opartych na plikach cookie to ustawienie określa wartość domeny cookie s. Wartość domyślna to pusty ciąg, który powoduje, że przeglądarka używa domeny, z której został wystawiony (na przykład www.yourdomain.com). W takim przypadku plik cookie <strong>nie</strong> będzie wysyłany podczas przesyłania żądań do poddomen, takich jak admin.yourdomain.com. Jeśli chcesz, aby plik cookie został przesłany do wszystkich poddomen, musisz dostosować atrybut domeny, ustawiając go na yourdomain.com. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Wartość logiczna wskazująca, czy użytkownicy uwierzytelnieni są zapamiętani w przypadku przekierowania do adresów URL w innych aplikacjach sieci Web na tym samym serwerze. Wartość domyślna to false.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      Adres URL strony logowania. Wartość domyślna to login. aspx.                                                                                                                                                                                                                      |
|            {1&gt;nazwa&lt;1}            |                                                                                                                                                                                                   W przypadku używania biletów uwierzytelniania opartych na plikach cookie nazwa pliku cookie. Wartość domyślna to. ASPXAUTH.                                                                                                                                                                                                   |
|            ścieżka            |                                                                             W przypadku korzystania z biletów uwierzytelniania opartych na plikach cookie to ustawienie określa atrybut ścieżki cookie s. Atrybut path umożliwia deweloperowi ograniczenie zakresu pliku cookie do określonej hierarchii katalogów. Wartość domyślna to/, która informuje przeglądarkę o wysłaniu pliku cookie biletu uwierzytelniania do dowolnego żądania wysłanego do domeny.                                                                              |
|         ochrona         |                                                                                                                                            Wskazuje, jakie techniki są używane do ochrony biletu uwierzytelniania formularzy. Dozwolone wartości to: ALL (wartość domyślna); Szyfrowania Dawaj i walidacja. Te ustawienia zostały szczegółowo omówione w kroku 3.                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                Wartość logiczna wskazująca, czy do przesyłania pliku cookie uwierzytelniania jest wymagane połączenie SSL. Wartość domyślna to false.                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 Wartość logiczna wskazująca, czy limit czasu plików cookie uwierzytelniania jest resetowany za każdym razem, gdy użytkownik odwiedzi lokację podczas jednej sesji. Wartość domyślna to true. Zasady limitu czasu biletu uwierzytelniania zostały omówione bardziej szczegółowo w sekcji Określanie wartości limitu czasu biletu s.                                                                                                 |
|          Limit czasu           |                                                                                                                               Określa czas (w minutach), po upływie którego wygasa plik cookie biletu uwierzytelniania. Wartość domyślna to 30. Zasady limitu czasu biletu uwierzytelniania zostały omówione bardziej szczegółowo w sekcji Określanie wartości limitu czasu biletu s.                                                                                                                               |

**Tabela 1**: Podsumowanie &lt;formularzy&gt; atrybutów elementu

W ASP.NET 2,0 i więcej wartości uwierzytelniania formularzy domyślnych są zakodowane w klasie FormsAuthenticationConfiguration w .NET Framework. Wszelkie modyfikacje muszą być stosowane w zależności od aplikacji w pliku Web. config. Różni się to od ASP.NET 1. x, gdzie domyślne wartości uwierzytelniania formularzy były przechowywane w pliku Machine. config (i dlatego mogą być modyfikowane za pomocą edycji Machine. config). W temacie ASP.NET 1. x jest wartościowa, aby wskazać, że wiele ustawień systemowych uwierzytelniania formularzy ma różne wartości domyślne w ASP.NET 2,0 i poza ASP.NET 1. x. Jeśli migrujesz aplikację ze środowiska ASP.NET 1. x, ważne jest, aby mieć świadomość tych różnic. Aby zapoznać się z listą różnic, zapoznaj [się z tematami &lt;forms&gt; dokumentacji technicznej](https://msdn.microsoft.com/library/1d3t3c61.aspx) dotyczącej elementu.

> [!NOTE]
> Kilka ustawień uwierzytelniania formularzy, takich jak limit czasu, domena i ścieżka, określają szczegóły dotyczące powstających plików cookie biletu uwierzytelniania formularzy. Aby uzyskać więcej informacji na temat plików cookie, sposobu ich działania i ich różnych właściwości, Przeczytaj [Samouczek dotyczący plików cookie](http://www.quirksmode.org/js/cookies.html).

### <a name="specifying-the-tickets-timeout-value"></a>Określanie wartości limitu czasu biletu

Bilet uwierzytelniania formularzy jest tokenem, który reprezentuje tożsamość. W przypadku biletów uwierzytelniania opartych na plikach cookie token ten jest przechowywany w postaci pliku cookie i wysyłany do serwera sieci Web na każdym żądaniu. Posiadanie tokenu, w zasadzie, deklaruje *, jestem*zalogowany, i jest używany, aby tożsamość użytkownika mogła być zapamiętywana między wizytami stron.

Bilet uwierzytelniania formularzy nie tylko zawiera tożsamość użytkownika, ale zawiera również informacje pomagające zapewnić integralność i bezpieczeństwo tokenu. Po zakończeniu nie chcemy, aby użytkownik szkodliwa mógł utworzyć token fałszywy, lub zmodyfikować token legit w niedrogi sposób.

Jeden taki bit informacji zawartych w biletze to *wygaśnięcie*, czyli Data i godzina, gdy bilet nie jest już ważny. Za każdym razem, gdy FormsAuthenticationModule bada bilet uwierzytelniania, gwarantuje to, że wygaśnięcie biletu nie zostało jeszcze zakończone. Jeśli ma, oduważa bilet i identyfikuje użytkownika jako anonimowy. Zapewnia to ochronę przed atakami metodą powtórzeń. Bez wygaśnięcia, Jeśli haker mógł uzyskać swoje ręce na prawidłowym biletem uwierzytelniania użytkownika, na przykład przez uzyskanie fizycznego dostępu do komputera i korzenia za pomocą ich plików cookie — może wysłać żądanie do serwera za pomocą tego skradzionego biletu uwierzytelniania i wpis wzmocnienia. Mimo że wygaśnięcie nie przeszkadza w tym scenariuszu, ogranicza okno, w którym atak może się powieść.

> [!NOTE]
> Krok 3. szczegóły dodatkowe techniki używane przez system uwierzytelniania formularzy do ochrony biletu uwierzytelniania.

Podczas tworzenia biletu uwierzytelniania system uwierzytelniania formularzy określa jego wygaśnięcie, sprawdzając ustawienie limitu czasu. Jak wskazano w tabeli 1, ustawienie limitu czasu domyślnie przyjmuje wartość 30 minut, co oznacza, że po utworzeniu biletu uwierzytelniania formularzy jego wygaśnięcie ma ustawioną datę i godzinę 30 minut w przyszłości.

Wygaśnięcie określa czas bezwzględny w przyszłości w przypadku wygaśnięcia biletu uwierzytelniania formularzy. Ale zazwyczaj deweloperzy chcą zaimplementować przewinięcie, który jest resetowany za każdym razem, gdy użytkownik ponownie odwiedza lokację. To zachowanie zależy od ustawień slidingExpiration. Jeśli wartość jest równa true (domyślnie), za każdym razem, gdy FormsAuthenticationModule uwierzytelnia użytkownika, aktualizuje ważność biletu. Jeśli zostanie ustawiona na wartość false, wygaśnięcie nie zostanie zaktualizowane dla każdego żądania, co spowoduje, że bilet wygaśnie dokładnie określoną liczbę minut, po której bilet został po raz pierwszy utworzony.

> [!NOTE]
> Wygaśnięcie przechowywane w biletu uwierzytelniania to bezwzględna wartość daty i godziny, na przykład 2 sierpnia 2008 11:34 AM. Ponadto Data i godzina są względne w stosunku do czasu lokalnego serwera sieci Web. Takie decyzje projektowe mogą mieć pewne interesujące skutki uboczne wokół czasu letniego (DST), który jest używany podczas przenoszenia zegarów w Stany Zjednoczone w ciągu jednej godziny (przy założeniu, że serwer sieci Web jest hostowany w ustawieniach regionalnych w przypadku zaobserwowania czasu letniego). Zastanów się, co się stanie w witrynie sieci Web ASP.NET z 30-minutowym wygaśnięciem w czasie, gdy zaczyna się od momentu rozpoczęcia (w 2:00 AM). Wyobraź sobie, że odwiedzający loguje się do witryny, 11 marca 2008 o 1:55 AM. Spowoduje to wygenerowanie biletu uwierzytelniania formularzy, który wygaśnie 11 marca 2008 o godz. 2:25 AM (30 minut w przyszłości). Jednak po rozliczeniu na 2:00, zegar zostanie przeskoczy do 3:00 AM z powodu czasu letniego. Gdy użytkownik ładuje nową stronę sześć minut po zalogowaniu się (o godzinie 3:01 AM), FormsAuthenticationModule, że bilet wygasł i przekierowuje użytkownika na stronę logowania. Aby uzyskać bardziej szczegółowe omówienie tego i innego limitu czasu biletu uwierzytelniania oddities, a także obejść, należy pobrać kopię firmy Stefan Schackow *Professional ASP.NET 2,0 Security, Membership i Management role* (ISBN: 978-0-7645-9698-8).

Rysunek 1 ilustruje przepływ pracy, gdy slidingExpiration jest ustawiona na false, a limit czasu jest ustawiony na 30. Należy zauważyć, że bilet uwierzytelniania wygenerowany przy logowaniu zawiera datę wygaśnięcia, a ta wartość nie jest aktualizowana w kolejnych żądaniach. Jeśli FormsAuthenticationModule stwierdzi, że bilet wygasł, odrzuci ją i traktuje żądanie jako anonimowe.

[![graficzną reprezentację wygaśnięcia biletu uwierzytelniania formularzy, gdy slidingExpiration ma wartość false](forms-authentication-configuration-and-advanced-topics-vb/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image1.png)

**Ilustracja 01**. Reprezentacja czasu wygaśnięcia biletu uwierzytelniania formularzy, gdy slidingExpiration ma wartość FAŁSZ ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-vb/_static/image3.png))

Rysunek 2 pokazuje przepływ pracy, gdy slidingExpiration ma wartość true, a limit czasu jest ustawiony na 30. Po otrzymaniu uwierzytelnionego żądania (z niewygasłym biletem) jego wygaśnięcie zostanie zaktualizowane do limitu czasu w minutach w przyszłości.

[![graficzną reprezentację biletu uwierzytelniania formularzy, gdy slidingExpiration ma wartość true](forms-authentication-configuration-and-advanced-topics-vb/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image4.png)

**Ilustracja 02**: graficzna reprezentacja biletu uwierzytelniania formularzy, gdy slidingExpiration ma wartość true ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-vb/_static/image6.png))

W przypadku korzystania z biletów uwierzytelniania opartych na plikach cookie (domyślnie) ta dyskusja stanie się nieco bardziej myląca, ponieważ pliki cookie mogą również mieć określone własne wygaśnięcie. Wygaśnięcie pliku cookie (lub jego brak) instruuje przeglądarkę, gdy plik cookie powinien zostać zniszczony. Jeśli plik cookie nie ma wygaśnięcia, zostanie zniszczony, gdy przeglądarka zostanie zamknięta. Jeśli jednak wygasa, plik cookie pozostaje przechowywany na komputerze użytkownika do momentu, gdy data i godzina określona w wygaśnięciu zakończyła się niepowodzenie. Gdy plik cookie zostanie zniszczony przez przeglądarkę, nie jest już wysyłany do serwera sieci Web. W związku z tym zniszczenie pliku cookie jest analogiczne do wylogowania użytkownika z lokacji.

> [!NOTE]
> Oczywiście użytkownik może aktywnie usunąć wszystkie pliki cookie przechowywane na komputerze. W programie Internet Explorer 7 przejdź do pozycji narzędzia, opcje i kliknij przycisk Usuń w sekcji Historia przeglądania. Następnie kliknij przycisk Usuń pliki cookie.

System uwierzytelniania formularzy tworzy pliki cookie oparte na sesji lub wygaśnięcia w zależności od wartości przesyłanej do parametru *persistCookie* . Odwołaj, że metody GetAuthCookie, SetAuthCookie i RedirectFromLoginPage klasy FormsAuthentication przyjmują dwa parametry wejściowe: *username* i *persistCookie*. Strona logowania utworzona w poprzednim samouczku zawiera pole wyboru Zapamiętaj mnie, które określa, czy został utworzony trwały plik cookie. Trwałe pliki cookie są oparte na wygaśnięciu; pliki cookie nietrwałe są oparte na sesji.

Omawiane pojęcia dotyczące przekroczenia limitu czasu i slidingExpiration stosują te same zarówno dla plików cookie opartych na sesji, jak i wygaśnięcia. Wykonywanie jest tylko jedną małą różnicą: w przypadku korzystania z plików cookie opartych na wygaśnięciu z slidingTimeout ustawionym na wartość true ważność pliku cookie jest aktualizowana tylko wtedy, gdy upłynęło więcej niż połowę określonego czasu.

Zaktualizujmy zasady limitu czasu biletu uwierzytelniania naszej witryny sieci Web, aby bilety przekroczą limit czasu po upływie 1 godziny (60 minut) przy użyciu przesuwanego okresu ważności. Aby zmienić tę zmianę, zaktualizuj plik Web. config, dodając element&gt; &lt;Forms do elementu &lt;Authentication&gt; z następującym znacznikiem:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Przy użyciu adresu URL strony logowania innego niż login. aspx

Ponieważ FormsAuthenticationModule automatycznie przekierowuje nieautoryzowanych użytkowników do strony logowania, musi znać adres URL strony logowania. Ten adres URL jest określany przez atrybut loginUrl w elemencie&gt; Forms &lt;, a wartość domyślna to login. aspx. W przypadku przenoszenia za pośrednictwem istniejącej witryny sieci Web może już istnieć strona logowania z innym adresem URL, taką, która została już zakładką i indeksowana przez aparaty wyszukiwania. Zamiast zmieniać nazwę istniejącej strony logowania na nazwę logowania. aspx i przerywać łącza oraz zakładki użytkowników, możesz zamiast tego zmodyfikować atrybut loginUrl, aby wskazywał na stronę logowania.

Na przykład jeśli strona logowania ma nazwę Signer. aspx i została umieszczona w katalogu Users, można wskazać ustawienie konfiguracji loginUrl na ~/Users/SignIn.aspx:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample3.xml)]

Ponieważ nasza bieżąca aplikacja ma już stronę logowania o nazwie login. aspx, nie ma potrzeby określania wartości niestandardowej w elemencie &lt;Forms&gt;.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Krok 2. Korzystanie z biletów uwierzytelniania formularzy bez plików cookie

Domyślnie system uwierzytelniania formularzy określa, czy mają być przechowywane bilety uwierzytelniania w kolekcji plików cookie, czy też osadzane w adresie URL na podstawie agenta użytkownika odwiedzającego witrynę. Wszystkie podstawowe przeglądarki pulpitu, takie jak Internet Explorer, Firefox, Opera i Safari, obsługują pliki cookie, ale nie wszystkie urządzenia przenośne.

Zasady dotyczące plików cookie używane przez system uwierzytelniania formularzy zależą od ustawienia bez plików cookie w &lt;&gt; formularzy elementu, do którego można przypisać jedną z czterech wartości:

- UseCookies — określa, że będą zawsze używane bilety uwierzytelniania oparte na plikach cookie.
- UseUri — wskazuje, że bilety uwierzytelniania oparte na plikach cookie nigdy nie będą używane.
- Autowykrywanie — Jeśli profil urządzenia nie obsługuje plików cookie, nie są używane bilety uwierzytelniania oparte na plikach cookie. Jeśli profil urządzenia obsługuje pliki cookie, mechanizm sondowania służy do określenia, czy pliki cookie są włączone.
- UseDeviceProfile — domyślne; używa biletów uwierzytelniania opartych na plikach cookie tylko wtedy, gdy profil urządzenia obsługuje pliki cookie. Nie jest używany żaden mechanizm sondowania.

Ustawienia autowykrywania i UseDeviceProfile polegają na *profilu urządzenia* w sprawdzaniu, czy należy używać biletów uwierzytelniania opartych na plikach cookie i plików cookie. ASP.NET obsługuje bazę danych różnych urządzeń i ich możliwości, takie jak obsługa plików cookie, obsługiwana przez nie wersja języka JavaScript itd. Za każdym razem, gdy urządzenie żąda strony sieci Web od serwera sieci Web wysyłanej przy użyciu nagłówka HTTP *User-Agent* , który identyfikuje typ urządzenia. ASP.NET automatycznie pasuje do podanego ciągu agenta użytkownika z odpowiednim profilem określonym w jego bazie danych.

> [!NOTE]
> Ta baza danych możliwości urządzenia jest przechowywana w wielu plikach XML, które są zgodne ze [schematem pliku definicji przeglądarki](https://msdn.microsoft.com/library/ms228122.aspx). Domyślne pliki profilów urządzeń znajdują się w%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. Możesz również dodawać niestandardowe pliki do aplikacji\_przeglądarki aplikacji. Aby uzyskać więcej informacji, zobacz [How to: Detect Types Browser in in ASP.NET Web Pages](https://msdn.microsoft.com/library/3yekbd5b.aspx).

Ponieważ ustawieniem domyślnym jest UseDeviceProfile, bilety uwierzytelniania formularzy bez plików cookie będą używane podczas odwiedzania lokacji przez urządzenie, którego profil zgłasza, że nie obsługuje plików cookie.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Kodowanie biletu uwierzytelniania w adresie URL

Pliki cookie są naturalnym nośnikiem w celu dołączenia informacji z przeglądarki w poszczególnych żądaniach do określonej witryny sieci Web, dlatego domyślne ustawienia uwierzytelniania formularzy używają plików cookie, jeśli urządzenie odwiedzające je obsługuje. Jeśli pliki cookie nie są obsługiwane, należy zastosować alternatywną metodę przekazywania biletu uwierzytelniania z klienta do serwera. Typowym obejściem używanym w środowiskach bez plików cookie jest kodowanie danych plików cookie w adresie URL.

Najlepszym sposobem, aby zobaczyć, jak takie informacje mogą zostać osadzone w adresie URL, jest wymuszenie użycia biletów uwierzytelniania bez plików cookie. Można to osiągnąć przez ustawienie ustawienia konfiguracja bezcookie na UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample4.xml)]

Po wprowadzeniu tej zmiany odwiedź witrynę za pomocą przeglądarki. Podczas odwiedzania jako użytkownik anonimowy adresy URL będą wyglądały tak samo jak wcześniej. Na przykład podczas odwiedzania strony Default. aspx na pasku adresu przeglądarki wyświetlany jest następujący adres URL:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Jednak po zalogowaniu bilet uwierzytelniania formularzy jest osadzony w adresie URL. Na przykład po odwiedzeniu strony logowania i zalogowaniu się jako sam zwracana jest strona default. aspx, ale jej adres URL jest następujący:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

Bilet uwierzytelniania formularzy został osadzony w adresie URL. Ciąg (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2) reprezentuje informacje o biletach uwierzytelniania zakodowane szesnastkowo i jest tymi samymi danymi, które zwykle są przechowywane w pliku cookie.

Aby bilety uwierzytelniania bez plików cookie działały, system musi kodować wszystkie adresy URL na stronie, aby uwzględnić dane biletu uwierzytelniania. w przeciwnym razie bilet uwierzytelniania zostanie utracony, gdy użytkownik kliknie link. Thankfully, ta logika osadzania jest wykonywana automatycznie. Aby zademonstrować tę funkcję, Otwórz stronę Default. aspx i Dodaj kontrolkę hiperłącze, ustawiając jej właściwości text i NavigateUrl odpowiednio do linku test i SomePage. aspx. Nie ma znaczenia, że w naszym projekcie nie ma żadnej strony o nazwie SomePage. aspx.

Zapisz zmiany w default. aspx, a następnie odwiedź je za pomocą przeglądarki. Zaloguj się do witryny, aby bilet uwierzytelniania formularzy został osadzony w adresie URL. Następnie z poziomu default. aspx kliknij link testowy link. Co się stało? Jeśli nie istnieje strona o nazwie SomePage. aspx, wystąpił błąd 404, ale nie jest to istotne w tym miejscu. Zamiast tego należy skoncentrować się na pasku adresu w przeglądarce. Należy pamiętać, że zawiera on bilet uwierzytelniania formularzy w adresie URL.

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

Adres URL SomePage. aspx w łącz został automatycznie przekonwertowany na adres URL, który zawierał bilet uwierzytelniania — nie musimy pisać Lick kodu! Bilet uwierzytelniania formularza zostanie automatycznie osadzony w adresie URL dla wszystkich hiperłączy, które nie zaczynają się od http://lub/. Nie ma znaczenia, czy hiperłącze pojawia się w wywołaniu metody Response. Redirect, w kontrolce HyperLink lub w zakotwiczonym elemencie HTML (tj. &lt;a href = "..."&gt;...&lt;/a&gt;). Pod warunkiem, że adres URL nie jest taki sam jak http://www.someserver.com/SomePage.aspx lub/SomePage.aspx, bilet uwierzytelniania formularzy zostanie osadzony dla nas.

> [!NOTE]
> Bilety uwierzytelniania formularzy bez plików cookie są zgodne z tymi samymi zasadami limitu czasu, co w przypadku biletów uwierzytelniania opartych na plikach cookie. Jednak bilety uwierzytelniania bez plików cookie są bardziej podatne na ataki metodą powtórzeń, ponieważ bilet uwierzytelniania jest osadzony bezpośrednio w adresie URL. Wyobraź sobie, że użytkownik odwiedzi witrynę internetową, zaloguje się, a następnie wklei adres URL w wiadomości e-mail do współpracownika. Jeśli współpracownik kliknie ten link przed osiągnięciem wygaśnięcia, zostanie zalogowany jako użytkownik, który wysłał wiadomość e-mail.

## <a name="step-3-securing-the-authentication-ticket"></a>Krok 3. Zabezpieczanie biletu uwierzytelniania

Bilet uwierzytelniania formularzy jest przesyłany przez sieć w pliku cookie lub osadzony bezpośrednio w adresie URL. Oprócz informacji o tożsamości bilet uwierzytelniania może również zawierać dane użytkownika (w kroku 4). W związku z tym ważne jest, aby dane biletu były szyfrowane z prying oczu i (jeszcze bardziej ważne), aby system uwierzytelniania formularzy mógł zagwarantować, że bilet nie został naruszony.

W celu zapewnienia prywatności danych biletu system uwierzytelniania formularzy może zaszyfrować dane biletów. Niepowodzenie szyfrowania danych biletu wysyła potencjalnie poufne informacje w postaci zwykłego tekstu.

W celu zagwarantowania autentyczności biletu system uwierzytelniania formularzy musi *zweryfikować* bilet. Weryfikacja polega na tym, że określona część danych nie została zmodyfikowana i jest realizowana za pośrednictwem *[kodu uwierzytelniania wiadomości (Mac)](http://en.wikipedia.org/wiki/Message_authentication_code)* . W Nutshell, MAC to niewielka część informacji, która identyfikuje dane, które muszą zostać zweryfikowane (w tym przypadku bilet). Jeśli dane reprezentowane przez komputer MAC są modyfikowane, komputer MAC i dane nie będą zgodne. Co więcej, trudne jest, aby haker mógł modyfikować dane i generować własne komputery MAC odpowiadające modyfikowanym danym.

Podczas tworzenia (lub modyfikowania) biletu system uwierzytelniania formularzy tworzy komputer MAC i dołącza go do danych biletu. Po nadejściu kolejnego żądania system uwierzytelniania formularzy porównuje dane dotyczące adresów MAC i biletów w celu zweryfikowania autentyczności danych biletu. Rysunek 3 ilustruje ten przepływ pracy graficznie.

[![autentyczność biletu jest zapewniana przez komputer MAC](forms-authentication-configuration-and-advanced-topics-vb/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image7.png)

**Ilustracja 03**: autentyczność biletu jest zapewniana przez Mac ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-vb/_static/image9.png))

Jakie miary zabezpieczeń są stosowane do biletu uwierzytelniania, zależy od ustawienia ochrony w &lt;&gt; formularzy elementu. Ustawienie ochrony może być przypisane do jednej z następujących trzech wartości:

- Wszystko — bilet jest szyfrowany i podpisany cyfrowo (domyślnie).
- Zastosowano szyfrowanie tylko do szyfrowania — nie jest generowany żaden adres MAC.
- Brak — bilet nie jest szyfrowany ani podpisany cyfrowo.
- Sprawdzanie poprawności — generowany jest komputer MAC, ale dane biletu są wysyłane przez sieć w postaci zwykłego tekstu.

Firma Microsoft zdecydowanie zaleca korzystanie z ustawień wszystkie.

### <a name="setting-the-validation-and-decryption-keys"></a>Ustawianie weryfikacji i kluczy odszyfrowywania

Algorytmy szyfrowania i wyznaczania wartości skrótu używane przez system uwierzytelniania formularzy do szyfrowania i weryfikowania biletu uwierzytelniania można dostosowywać za pomocą [elementu&lt;machineKey&gt;](https://msdn.microsoft.com/library/w8h3skw9.aspx) w pliku Web. config. W tabeli 2 przedstawiono atrybuty elementu &lt;machineKey&gt; i ich możliwe wartości.

| **Atrybut** | **Opis** |
| --- | --- |
| odszyfrowywanie | Wskazuje algorytm używany do szyfrowania. Ten atrybut może mieć jedną z następujących czterech wartości:-autodefault. określa algorytm na podstawie długości atrybutu decryptionKey. -AES-używa algorytmu [Advanced Encryption Standard (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) . -DES — używa algorytmu [des (Data Encryption Standard)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) jest uznawany za niesłaby i nie należy jej używać. -3DES — używa algorytmu [Triple DES](http://en.wikipedia.org/wiki/Triple_DES) , który działa przez zastosowanie algorytmu des trzykrotnie. |
| decryptionKey | Klucz tajny używany przez algorytm szyfrowania. Ta wartość musi być ciągiem szesnastkowym o odpowiedniej długości (na podstawie wartości w odszyfrowywaniu), autogeneratu lub dowolnej wartości dołączonej do IsolateApps. Dodanie IsolateApps instruuje ASP.NET o użycie unikatowej wartości dla każdej aplikacji. Wartość domyślna to AutoGenerate, IsolateApps. |
| weryfikacja | Wskazuje algorytm używany do walidacji. Ten atrybut może mieć jedną z następujących czterech wartości:-AES-używa algorytmu Advanced Encryption Standard (AES). -MD5-używa algorytmu [Message-Digest 5 (MD5)](http://en.wikipedia.org/wiki/MD5) . -SHA1-używa algorytmu [SHA1](http://en.wikipedia.org/wiki/Sha1) (wartość domyślna). -3DES — używa algorytmu Triple DES. |
| validationKey | Klucz tajny używany przez algorytm walidacji. Ta wartość musi być ciągiem szesnastkowym o odpowiedniej długości (na podstawie wartości w walidacji), autogeneratu lub dowolnej wartości dołączonej do IsolateApps. Dodanie IsolateApps instruuje ASP.NET o użycie unikatowej wartości dla każdej aplikacji. Wartość domyślna to AutoGenerate, IsolateApps. |

**Tabela 2**: atrybuty elementu &lt;machineKey&gt;

Szczegółowe omówienie tych opcji szyfrowania i walidacji oraz specjalistów i wad różnych algorytmów wykracza poza zakres tego samouczka. Aby uzyskać szczegółowe informacje na temat tych problemów, w tym wskazówki dotyczące tego, jakie algorytmy szyfrowania i walidacji mają być używane, jakie długości kluczy należy używać oraz jak najlepiej generować te klucze, zapoznaj się z artykułem *Professional ASP.NET 2,0 Security, Membership i role*.

Domyślnie klucze używane do szyfrowania i walidacji są generowane automatycznie dla każdej aplikacji, a klucze te są przechowywane w lokalnym urzędzie zabezpieczeń (LSA). W skrócie ustawienia domyślne gwarantują unikatowe klucze na serwerze sieci Web i aplikacji sieci Web. W związku z tym domyślne zachowanie nie będzie działało w przypadku dwóch następujących scenariuszy:

- Farmy **sieci** Web — w scenariuszu [farmy sieci](http://en.wikipedia.org/wiki/Web_farm) Web pojedyncza aplikacja sieci Web jest hostowana na wielu serwerach sieci Web na potrzeby skalowalności i nadmiarowości. Każde żądanie przychodzące jest wysyłane do serwera w farmie, co oznacza, że w okresie istnienia sesji użytkownika różne serwery mogą być używane do obsługi różnych żądań. W związku z tym każdy serwer musi używać tych samych kluczy szyfrowania i sprawdzania poprawności, aby bilet uwierzytelniania formularzy utworzony, zaszyfrowany i sprawdzony na jednym serwerze mógł zostać odszyfrowany i sprawdzony na innym serwerze w farmie.
- **Udostępnianie biletów między aplikacjami** — pojedynczy serwer sieci Web może obsługiwać wiele aplikacji ASP.NET. Jeśli chcesz, aby te różne aplikacje współdzielą bilet uwierzytelniania jednego formularza, konieczna jest zgodność ich kluczy szyfrowania i walidacji.

Podczas pracy w ustawieniach kolektywu serwerów sieci Web lub udostępniania biletów uwierzytelniania między aplikacjami na tym samym serwerze, należy skonfigurować &lt;machineKey&gt; elementu w aplikacjach, których dotyczy ten stan, tak aby ich wartości decryptionKey i validationKey były zgodne.

Chociaż żaden z powyższych scenariuszy nie ma zastosowania do naszej przykładowej aplikacji, nadal możemy określić jawne wartości decryptionKey i validationKey oraz zdefiniować algorytmy, które mają być używane. Dodaj ustawienie &lt;machineKey&gt; do pliku Web. config:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample5.xml)]

Więcej informacji [można znaleźć w temacie How to: Configure MachineKey in ASP.NET 2,0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> Wartości decryptionKey i validationKey zostały wykonane ze [strony sieci Web idealne hasła](https://www.grc.com/passwords.htm) [Gibson](http://www.grc.com/stevegibson.htm), która generuje 64 losowe znaki szesnastkowe na każdej stronie. Aby zmniejszyć prawdopodobieństwo, że te klucze są używane w aplikacjach produkcyjnych, zaleca się zamienienie powyższych kluczy z losowo generowanymi nimi na stronie idealne hasła.

## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Krok 4. przechowywanie dodatkowych danych użytkownika w bilet

Wiele aplikacji sieci Web Wyświetla informacje na temat lub na podstawie wyświetlania strony na bieżąco zalogowanego użytkownika. Na przykład na stronie sieci Web może zostać wyświetlona nazwa użytkownika i Data ostatniego logowania w górnym rogu każdej strony. Bilet uwierzytelniania formularzy przechowuje nazwę użytkownika zalogowanego użytkownika, ale jeśli są wymagane inne informacje, strona musi przejść do magazynu użytkownika — zwykle jest to baza danych — wyszukiwanie informacji nieprzechowywanych w bilet uwierzytelniania.

W przypadku małej ilości kodu możemy przechowywać dodatkowe informacje o użytkowniku w biletu uwierzytelniania formularzy. Takie dane można wyrazić za pomocą [Właściwości UserData](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx) [klasy FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx). Jest to przydatne miejsce do umieszczania niewielkich ilości informacji o użytkowniku, który jest często wymagany. Wartość określona we właściwości UserData jest uwzględniana jako część pliku cookie biletu uwierzytelniania, podobnie jak w przypadku innych pól biletu, jest zaszyfrowana i weryfikowana na podstawie konfiguracji systemu uwierzytelniania formularzy. Domyślnie, UserData jest pustym ciągiem.

Aby można było przechowywać dane użytkownika w biletu uwierzytelniania, musimy napisać bit kodu na stronie logowania, który poprowadzi informacje specyficzne dla użytkownika i zapisze go w bilet. Ponieważ UserData jest właściwością typu String, dane przechowywane w niej muszą być poprawnie serializowane jako ciąg. Załóżmy na przykład, że nasz magazyn użytkowników uwzględniał datę urodzenia każdego użytkownika i jego nazwę pracodawcy, a chcemy przechowywać te dwie wartości właściwości w biletach uwierzytelniania. Możemy serializować te wartości do ciągu przez połączenie daty urodzenia użytkownika z potoku (|), po którym następuje nazwa pracodawcy. Dla użytkownika urodzonego 15 sierpnia 1974, który działa w przypadku Northwind handlowców, przypiszemy Właściwość UserData ciąg: 1974-08-15 | Northwind Traders.

Zawsze, gdy będziemy musieli uzyskać dostęp do danych przechowywanych w biletach, możemy to zrobić, pobierając bieżące żądanie FormsAuthenticationTicket i deserializacji właściwości UserData. W przypadku daty urodzenia i nazwy pracodawcy, możemy podzielić ciąg UserData na dwa podciągi na podstawie ogranicznika (|).

[![informacje o dodatkowych użytkownikach mogą być przechowywane w biletach uwierzytelniania](forms-authentication-configuration-and-advanced-topics-vb/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image10.png)

**Rysunek 04**: dodatkowe informacje o użytkowniku mogą być przechowywane w biletu uwierzytelniania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-vb/_static/image12.png))

### <a name="writing-information-to-userdata"></a>Zapisywanie informacji do UserData

Niestety, Dodawanie informacji specyficznych dla użytkownika do biletu uwierzytelniania formularzy nie jest tak proste, jak może się spodziewać. Właściwość UserData klasy FormsAuthenticationTicket jest tylko do odczytu i może być określona tylko za pomocą konstruktora klasy FormsAuthenticationTicket. Podczas określania właściwości UserData w Konstruktorze należy również podać inne wartości biletu: nazwę użytkownika, datę wydania, wygaśnięcie i tak dalej. Po utworzeniu strony logowania w poprzednim samouczku wszystkie te dane były obsługiwane przez klasę FormsAuthentication. Podczas dodawania elementu UserData do FormsAuthenticationTicket musimy napisać kod, aby replikować większość funkcji już dostarczonych przez klasę FormsAuthentication.

Zapoznaj się z kodem wymaganym do pracy z usługą UserData przez zaktualizowanie strony Login. aspx, aby zarejestrować dodatkowe informacje o użytkowniku do biletu uwierzytelniania. Poudawać, że nasz magazyn użytkowników zawiera informacje o firmie, w której pracuje użytkownik, oraz o ich tytule oraz o tym, że chcemy przechwycić te informacje w biletu uwierzytelniania. Zaktualizuj procedurę obsługi zdarzeń LoginButton strony Login. aspx, aby kod wyglądał następująco:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample6.vb)]

Przejdźmy do tego kodu po jednym wierszu naraz. Metoda zaczyna się od zdefiniowania czterech tablic ciągów: Użytkownicy, hasła, NazwaFirmy i titleAtCompany. W tych tablicach znajdują się nazwy użytkowników, hasła, nazwy firm i tytuły dla kont użytkownika w systemie, dla których istnieją trzy: Scott, Jisun i sam. W prawdziwej aplikacji te wartości byłyby wysyłane z magazynu użytkownika, a nie są twarde w kodzie źródłowym strony.

W poprzednim samouczku, jeśli podane poświadczenia były prawidłowe, po prostu nazywamy FormsAuthentication. RedirectFromLoginPage (UserName. text, kontrolka rememberMe. Checked), który wykonał następujące czynności:

1. Utworzono bilet uwierzytelniania formularzy
2. Zapisał bilet do odpowiedniego magazynu. W przypadku biletów uwierzytelniania opartych na plikach cookie jest używana Kolekcja plików cookie przeglądarki. w przypadku biletów uwierzytelniania bez plików cookie dane biletów są serializowane do adresu URL
3. Przekierowanie użytkownika do odpowiedniej strony

Te kroki są replikowane w powyższym kodzie. Najpierw ciąg, który zostanie ostatecznie zapisany we właściwości UserData, jest tworzony przez połączenie nazwy firmy i tytułu, ograniczając te dwie wartości przy użyciu znaku potoku (|).

Dim userDataString jako ciąg = String. Concat (NazwaFirmy (i), "|", titleAtCompany (i))

Następnie wywoływana jest metoda FormsAuthentication. GetAuthCookie, która tworzy bilet uwierzytelniania, szyfruje i sprawdza poprawność zgodnie z ustawieniami konfiguracji i umieszcza je w obiekcie HttpCookie.

Dim authCookie jako HttpCookie = FormsAuthentication. GetAuthCookie (UserName. text, kontrolka rememberMe. Checked)

Aby można było współpracować z FormAuthenticationTicket osadzonym w pliku cookie, musimy wywołać [metodę odszyfrowania](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx)klasy FormAuthentication, przekazując wartość cookie.

Dim — bilet as FormsAuthenticationTicket = FormsAuthentication. Odszyfruj (authCookie. Value)

Następnie utworzymy *nowe* wystąpienie FormsAuthenticationTicket na podstawie wartości istniejących FormsAuthenticationTicket. Jednak ten nowy bilet zawiera informacje specyficzne dla użytkownika (userDataString).

Dim newTicket jako FormsAuthenticationTicket = New FormsAuthenticationTicket (bilet. Wersja, bilet. Nazwa, bilet. IssueDate, bilet. Wygaśnięcie, bilet. IsPersistent, userDataString)

Następnie szyfrujemy (i weryfikuje) nowe wystąpienie FormsAuthenticationTicket przez wywołanie [metody szyfrowania](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)i umieszczenie tych zaszyfrowanych (i zweryfikowanych) danych z powrotem w authCookie.

authCookie. Value = FormsAuthentication. Szyfruj (newTicket)

Na koniec authCookie jest dodawany do kolekcji Response. cookies, a metoda GetRedirectUrl jest wywoływana w celu określenia odpowiedniej strony do wysłania użytkownika.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample7.vb)]

Cały ten kod jest wymagany, ponieważ Właściwość UserData jest tylko do odczytu, a Klasa FormsAuthentication nie udostępnia żadnych metod określania informacji UserData w metodach GetAuthCookie, SetAuthCookie lub RedirectFromLoginPage.

> [!NOTE]
> Właśnie sprawdzony kod przechowuje informacje specyficzne dla użytkownika w biletach uwierzytelniania opartych na plikach cookie. Klasy odpowiedzialne za Serializowanie biletu uwierzytelniania formularzy do adresu URL są wewnętrzne dla .NET Framework. Długie scenariusze krótko nie można przechowywać danych użytkownika w biletach uwierzytelniania formularzy bez plików cookie.

### <a name="accessing-the-userdata-information"></a>Uzyskiwanie dostępu do informacji o UserData

W tym momencie nazwa i tytuł firmy każdego użytkownika są przechowywane we właściwości UserData biletu uwierzytelniania formularzy po zalogowaniu się. Dostęp do tych informacji można uzyskać, korzystając z biletu uwierzytelniania na dowolnej stronie, bez konieczności wyjazdu do magazynu użytkowników. Aby zilustrować, jak te informacje można pobrać z właściwości UserData, zaktualizujmy default. aspx, tak aby komunikat powitalny zawierał nie tylko nazwę użytkownika, ale również firmę, dla której pracują i ich tytułem.

Obecnie default. aspx zawiera panel AuthenticatedMessagePanel z kontrolką etykieta o nazwie WelcomeBackMessage. Ten panel jest wyświetlany tylko dla uwierzytelnionych użytkowników. Zaktualizuj kod na stronie Default. aspx\_Załaduj procedurę obsługi zdarzeń, aby wyglądać następująco:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample8.vb)]

Jeśli żądanie. IsAuthenticated ma wartość true, właściwość Text WelcomeBackMessage jest najpierw ustawiona na Witamy z powrotem, *username*. Następnie Właściwość User. Identity jest rzutowana na obiekt FormsIdentity, aby można było uzyskać dostęp do bazowego FormsAuthenticationTicketu. Gdy mamy FormsAuthenticationTicket, deserializowaćmy właściwości UserData do nazwy i tytułu firmy. W tym celu należy podzielić ciąg na znak potoku. Nazwa firmy i tytuł są następnie wyświetlane w etykiecie WelcomeBackMessage.

Rysunek 5 pokazuje zrzut ekranu przedstawiający ten ekran w akcji. Zalogowanie się jako Scott wyświetla komunikat powitalny, który obejmuje firmę i tytuł Scotta.

[![jest wyświetlana nazwa aktualnie zalogowanego użytkownika i tytuł](forms-authentication-configuration-and-advanced-topics-vb/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image13.png)

**Ilustracja 05**. są wyświetlane aktualnie zalogowanego użytkownika i tytuł ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-vb/_static/image15.png))

> [!NOTE]
> Właściwość UserData biletu uwierzytelniania służy jako pamięć podręczna magazynu użytkownika. Podobnie jak w przypadku każdej pamięci podręcznej, należy ją zaktualizować, gdy dane podstawowe są modyfikowane. Na przykład jeśli istnieje strona sieci Web, z której użytkownicy mogą zaktualizować swój profil, pola buforowane we właściwości UserData muszą zostać odświeżone w celu odzwierciedlenia zmian wprowadzonych przez użytkownika.

## <a name="step-5-using-a-custom-principal"></a>Krok 5. Korzystanie z niestandardowego podmiotu zabezpieczeń

Na każdym żądaniu przychodzącym FormsAuthenticationModule próbuje uwierzytelnić użytkownika. Jeśli istnieje bilet niewygasłego uwierzytelniania, FormsAuthenticationModule przypisze Właściwość HttpContext. User do nowego obiektu GenericPrincipal —. Ten obiekt GenericPrincipal — ma tożsamość typu FormsIdentity, która zawiera odwołanie do biletu uwierzytelniania formularzy. Klasa GenericPrincipal — zawiera minimalne funkcje niezbędne przez klasę, która implementuje IPrincipal — tylko ma właściwość Identity i metodę IsInRole.

Obiekt Principal ma dwie obowiązki: aby wskazać, do jakich ról należy użytkownik i podać informacje o tożsamości. Jest to realizowane za pomocą odpowiednio metody IsInRole (*rolename*) interfejsu IPrincipal. Klasa GenericPrincipal — umożliwia określenie tablicy ciągów nazw ról do określenia za pośrednictwem jego konstruktora; jego Metoda IsInRole (*rolename*) sprawdza tylko, *czy w tablicy ciągów istnieje* przekazanie. Gdy FormsAuthenticationModule tworzy GenericPrincipal —, przekazuje pustą tablicę ciągów do konstruktora GenericPrincipal —. W związku z tym każde wywołanie metody IsInRole zawsze zwróci wartość false.

Klasa GenericPrincipal — spełnia potrzeby większości scenariuszy uwierzytelniania opartych na formularzach, w których role nie są używane. W przypadku sytuacji, w których domyślna obsługa ról jest niewystarczająca lub gdy konieczne jest skojarzenie niestandardowego obiektu IIdentity z użytkownikiem, można utworzyć niestandardowy obiekt IPrincipal podczas przepływu pracy uwierzytelniania i przypisać go do właściwości HttpContext. User.

> [!NOTE]
> Jak zobaczymy w przyszłych samouczkach, gdy ASP. Struktura ról w sieci jest włączona, tworzy niestandardowy obiekt podmiotu zabezpieczeń typu [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) i zastępuje obiekt GenericPrincipal — utworzony przez funkcję uwierzytelniania formularzy. Wykonuje to w celu dostosowania metody IsInRole podmiotu zabezpieczeń do interfejsu za pomocą interfejsu API struktury role.

Ponieważ nie mamy jeszcze żadnych wypróbujemy z rolami, jedyną przyczyną, którą chcemy utworzyć niestandardowy podmiot zabezpieczeń w tym miejscu, byłoby skojarzenie niestandardowego obiektu IIdentity z podmiotem zabezpieczeń. W kroku 4 zawarto przechowywanie dodatkowych informacji o użytkowniku we właściwości UserData biletu uwierzytelniania, w szczególności nazwy firmy użytkownika i ich tytułu. Informacje o postawce UserData są jednak dostępne tylko za pomocą biletu uwierzytelniania, a następnie jako ciąg serializowany, co oznacza, że w dowolnym momencie chcemy wyświetlić informacje o użytkowniku przechowywane w biletze, musimy przeanalizować Właściwość UserData.

Możemy ulepszyć środowisko programistyczne, tworząc klasę implementującą IIdentity i zawierającą właściwości NazwaFirmy i title. Dzięki temu deweloper może uzyskać dostęp do obecnie zalogowanej nazwy i tytułu firmy użytkownika bezpośrednio za pomocą właściwości NazwaFirmy i title bez konieczności, aby wiedzieć, jak przeanalizować Właściwość UserData.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Tworzenie tożsamości niestandardowej i klas głównych

Na potrzeby tego samouczka utworzysz niestandardowe obiekty Principal i Identity w folderze\_aplikacji. Zacznij od dodania\_aplikacji folder kodu do projektu — kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, wybierz opcję Dodaj folder ASP.NET, a następnie wybierz pozycję Aplikacja\_kod. Folder Code\_App jest specjalnym folderem ASP.NET, który zawiera pliki klas specyficzne dla witryny sieci Web.

> [!NOTE]
> Folderu\_aplikacji należy używać tylko w przypadku zarządzania projektem za pomocą modelu projektu witryny sieci Web. Jeśli używasz [modelu projektu aplikacji sieci Web](https://msdn.microsoft.com/asp.net/Aa336618.aspx), Utwórz folder standardowy i Dodaj do niego klasy. Można na przykład dodać nowy folder o nazwie Classes i umieścić w nim kod.

Następnie Dodaj dwa nowe pliki klasy do folderu\_aplikacji, jeden o nazwie CustomIdentity. vb i jeden o nazwie CustomPrincipal. vb.

[![dodać klasy CustomIdentity i CustomPrincipal do projektu](forms-authentication-configuration-and-advanced-topics-vb/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image16.png)

**Ilustracja 06**. Dodawanie klas CustomIdentity i CustomPrincipal do projektu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-vb/_static/image18.png))

Klasa CustomIdentity jest odpowiedzialna za implementację interfejsu IIdentity, który definiuje właściwości AuthenticationType, IsAuthenticated i Name. Oprócz tych wymaganych właściwości chcemy ujawniać odpowiedni bilet uwierzytelniania formularzy oraz właściwości nazwy i tytułu firmy użytkownika. Wprowadź następujący kod do klasy CustomIdentity.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample9.vb)]

Należy zauważyć, że Klasa zawiera zmienną członkowską FormsAuthenticationTicket (bilet\_) i że informacje o biletach muszą być dostarczone przez Konstruktor. Dane biletów są używane w zwracaniu nazwy tożsamości; Właściwość UserData jest analizowana w celu zwrócenia wartości właściwości NazwaFirmy i title.

Następnie Utwórz klasę CustomPrincipal. Ponieważ nie są one objęte rolami w tym miejscu, Konstruktor klasy CustomPrincipal akceptuje tylko obiekt CustomIdentity; jego Metoda IsInRole zawsze zwraca wartość false.

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample10.vb)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Przypisywanie obiektu CustomPrincipal do kontekstu zabezpieczeń żądania przychodzącego

Mamy teraz klasę, która rozszerza domyślną specyfikację IIdentity w taki sposób, aby obejmowała właściwości NazwaFirmy i title oraz niestandardową klasę podmiotu, która używa tożsamości niestandardowej. Jesteśmy gotowi do przechodzenia do potoku ASP.NET i przypisywania naszego niestandardowego obiektu głównego do kontekstu zabezpieczeń żądania przychodzącego.

Potok ASP.NET przyjmuje przychodzące żądanie i przetwarza go za pomocą kilku kroków. W każdym kroku zostanie zgłoszone konkretne zdarzenie, dzięki czemu deweloperzy mogą wybierać potok ASP.NET i modyfikować żądanie w określonych punktach w jego cyklu życia. FormsAuthenticationModule, na przykład, czeka na wystąpienie [zdarzenia AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)przez ASP.NET, co wskazuje na to, że sprawdza przychodzące żądanie biletu uwierzytelniania. Jeśli zostanie znaleziony bilet uwierzytelniania, obiekt GenericPrincipal — zostanie utworzony i przypisany do właściwości HttpContext. User.

Po zdarzeniu AuthenticateRequest potok ASP.NET wywołuje [zdarzenie PostAuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), w którym można zastąpić obiekt GenericPrincipal — utworzony przez FormsAuthenticationModule z wystąpieniem naszego obiektu CustomPrincipal. Rysunek 7 przedstawia ten przepływ pracy.

[![GenericPrincipal — jest zastępowana przez CustomPrincipal w zdarzeniu PostAuthenticationRequest](forms-authentication-configuration-and-advanced-topics-vb/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image19.png)

**Ilustracja 07**: GenericPrincipal — jest zastępowana przez CustomPrincipal w zdarzeniu PostAuthenticationRequest ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-vb/_static/image21.png))

Aby można było wykonać kod w odpowiedzi na zdarzenie potoku ASP.NET, możemy utworzyć odpowiedni program obsługi zdarzeń w Global. asax lub utworzyć własny moduł HTTP. Na potrzeby tego samouczka utworzymy procedurę obsługi zdarzeń w Global. asax. Zacznij od dodania szablonu Global. asax do witryny sieci Web. Kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań i Dodaj element typu globalnej klasy aplikacji o nazwie Global. asax.

[![dodać pliku Global. asax do witryny sieci Web](forms-authentication-configuration-and-advanced-topics-vb/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image22.png)

**Ilustracja 08**: Dodawanie pliku Global. asax do witryny sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-vb/_static/image24.png))

Domyślny szablon Global. asax zawiera programy obsługi zdarzeń dla wielu zdarzeń potoku ASP.NET, w tym zdarzenia początkowego, końcowego i [błędu](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), między innymi. Możesz usunąć te programy obsługi zdarzeń, ponieważ nie są one potrzebne dla tej aplikacji. Zainteresowane wydarzenie jest PostAuthenticateRequest. Zaktualizuj plik Global. asax, aby jego znaczniki wyglądały podobnie do następujących:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample11.aspx)]

Aplikacja\_Metoda OnPostAuthenticateRequest jest wykonywana za każdym razem, gdy środowisko uruchomieniowe ASP.NET wywołuje zdarzenie PostAuthenticateRequest, które występuje raz na każdym żądaniu strony przychodzącej. Program obsługi zdarzeń jest uruchamiany przez sprawdzenie, czy użytkownik jest uwierzytelniany i został uwierzytelniony za pomocą uwierzytelniania formularzy. W takim przypadku tworzony jest nowy obiekt CustomIdentity i przeszedł bilet uwierzytelniania bieżącego żądania w konstruktorze. Po wykonaniu tej czynności obiekt CustomPrincipal jest tworzony i przeszedł właśnie utworzony obiekt CustomIdentity w konstruktorze. Na koniec kontekst zabezpieczeń bieżącego żądania jest przypisywany do nowo utworzonego obiektu CustomPrincipal.

Należy zauważyć, że ostatnim krokiem jest skojarzenie obiektu CustomPrincipal z kontekstem zabezpieczeń żądania — przypisuje podmiot zabezpieczeń do dwóch właściwości: HttpContext. User i Thread. CurrentPrincipal. Te dwa przypisania są niezbędne ze względu na sposób, w jaki konteksty zabezpieczeń są obsługiwane w ASP.NET. .NET Framework kojarzy kontekst zabezpieczeń z każdym uruchomionym wątkiem; te informacje są dostępne jako obiekt IPrincipal za pomocą [Właściwości CurrentPrincipal](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx) [obiektu wątku](https://msdn.microsoft.com/library/system.threading.thread.aspx). To, co jest mylące, jest to, że ASP.NET ma własne informacje kontekstu zabezpieczeń (HttpContext. user).

W niektórych scenariuszach Właściwość Thread. CurrentPrincipal jest sprawdzana podczas określania kontekstu zabezpieczeń. w innych scenariuszach element HttpContext. user jest używany. Na przykład istnieją funkcje zabezpieczeń w programie .NET, które umożliwiają deweloperom deklaratywne określenie, które użytkownicy lub role mogą tworzyć wystąpienie klasy lub wywoływać konkretne metody (zobacz [Dodawanie reguł autoryzacji do warstw firmy i danych za pomocą PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Te metody deklaracyjne określają kontekst zabezpieczeń za pośrednictwem właściwości Thread. CurrentPrincipal.

W innych scenariuszach używana jest Właściwość HttpContext. User. Na przykład w poprzednim samouczku ta właściwość została użyta w celu wyświetlenia aktualnie zalogowanego użytkownika. Jasno, jest to konieczne, aby informacje kontekstu zabezpieczeń w wątkach. CurrentPrincipal oraz właściwości HttpContext. User były zgodne.

Środowisko uruchomieniowe ASP.NET automatycznie synchronizuje te wartości właściwości dla nas. Jednak taka synchronizacja występuje po zdarzeniu AuthenticateRequest, ale *przed* zdarzeniem PostAuthenticateRequest. W związku z tym podczas dodawania niestandardowego podmiotu zabezpieczeń w zdarzeniu PostAuthenticateRequest musi być to konieczne do ręcznego przypisania wątku. CurrentPrincipal lub else wątku. CurrentPrincipal i HttpContext. użytkownik nie będzie zsynchronizowany. Aby zapoznać się z bardziej szczegółowym omówieniem tego problemu, zobacz [Context. User vs. Thread. CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) .

### <a name="accessing-the-companyname-and-title-properties"></a>Uzyskiwanie dostępu do właściwości NazwaFirmy i title

Za każdym razem, gdy żądanie dociera i jest wysyłane do aparatu ASP.NET, aplikacja\_OnPostAuthenticateRequest programu obsługi zdarzeń w programie Global. asax zostanie wyzwolona. Jeśli żądanie zostało pomyślnie uwierzytelnione przez FormsAuthenticationModule, program obsługi zdarzeń utworzy nowy obiekt CustomPrincipal z obiektem CustomIdentity na podstawie biletu uwierzytelniania formularzy. W przypadku tej logiki dostęp do informacji o nazwie i tytule aktualnie zalogowanego użytkownika jest niezwykle prosty.

Wróć do strony\_Załaduj procedurę obsługi zdarzeń w default. aspx, gdzie w kroku 4 został napisany kod umożliwiający pobranie biletu uwierzytelniania formularza i przeanalizowanie właściwości UserData w celu wyświetlenia nazwy i tytułu firmy użytkownika. Gdy obiekty CustomPrincipal i CustomIdentity są obecnie używane, nie trzeba analizować wartości z właściwości UserData biletu. Zamiast tego wystarczy uzyskać odwołanie do obiektu CustomIdentity i użyć jego właściwości NazwaFirmy i title:

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample12.vb)]

## <a name="summary"></a>Podsumowanie

W tym samouczku sprawdzono, jak dostosować ustawienia systemu uwierzytelniania formularzy za pośrednictwem pliku Web. config. Zawarto informacje na temat sposobu obsługi ważności biletu uwierzytelniania oraz sposobu, w jaki zabezpieczenia dotyczące szyfrowania i weryfikacji są używane do ochrony biletu przed inspekcją i modyfikacją. Na koniec omówiono użycie właściwości UserData biletu uwierzytelniania w celu przechowywania dodatkowych informacji o użytkownikach w ramach biletu, a także sposobu używania niestandardowych obiektów Principal i Identity w celu udostępnienia tych informacji w bardziej przyjazny dla deweloperów sposób.

W tym samouczku zakończymy badanie uwierzytelniania formularzy w ASP.NET. W następnym samouczku rozpocznie się kurs do struktury członkostwa.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Uwierzytelnianie formularzy odizolowanych](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Wyjaśniono: uwierzytelnianie formularzy w ASP.NET 2,0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Instrukcje: ochrona uwierzytelniania formularzy w ASP.NET 2,0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Professional ASP.NET 2,0 — zabezpieczenia, członkostwo i zarządzanie rolami](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Zabezpieczanie formantów logowania](https://msdn.microsoft.com/library/ms178346.aspx)
- [Element&gt; &lt;Authentication](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;formularzy&gt; elementu do uwierzytelniania &lt;&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [Element &lt;machineKey&gt;](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Informacje o biletach i pliku cookie uwierzytelniania formularzy](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Szkolenia wideo dotyczące tematów zawartych w tym samouczku

- [Jak zmienić właściwości uwierzytelniania formularzy](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Jak skonfigurować uwierzytelnianie bez plików cookie i korzystać z nich w aplikacji ASP.NET](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Relokacja logowania formularzy stron ASP](../../../videos/authentication/asp-forms-login-relocation.md)
- [Konfiguracja niestandardowa kluczy logowania formularzy](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Dodawanie niestandardowych danych do metody uwierzytelniania](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Używanie niestandardowych obiektów głównych](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Informacje o autorze

Scott Mitchell, autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to *[Sams ASP.NET 2,0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott można uzyskać w [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Alicja Maziarz. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Ubiegł](an-overview-of-forms-authentication-vb.md)

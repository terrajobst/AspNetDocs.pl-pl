---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
title: Ochrona parametrów połączeń i innych informacji konfiguracyjnych (VB) | Microsoft Docs
author: rick-anderson
description: Aplikacja ASP.NET zazwyczaj przechowuje informacje o konfiguracji w pliku Web. config. Niektóre z tych informacji są poufne i gwarantują ochronę. Według def...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: cd17dbe1-c5e1-4be8-ad3d-57233d52cef1
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 070e1dccb80ef9af21ea621357c5b23e2ada6f9f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607703"
---
# <a name="protecting-connection-strings-and-other-configuration-information-vb"></a>Ochrona parametrów połączeń i innych informacji o konfiguracji (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_VB.zip) lub [Pobierz plik PDF](protecting-connection-strings-and-other-configuration-information-vb/_static/datatutorial73vb1.pdf)

> Aplikacja ASP.NET zazwyczaj przechowuje informacje o konfiguracji w pliku Web. config. Niektóre z tych informacji są poufne i gwarantują ochronę. Domyślnie ten plik nie będzie obsługiwany przez Gościa witryny sieci Web, ale administrator lub haker może uzyskać dostęp do systemu plików serwera sieci Web i wyświetlić zawartość pliku. W tym samouczku dowiesz się, że ASP.NET 2,0 pozwala nam chronić poufne informacje przez szyfrowanie sekcji pliku Web. config.

## <a name="introduction"></a>Wprowadzenie

Informacje o konfiguracji aplikacji ASP.NET są zwykle przechowywane w pliku XML o nazwie `Web.config`. W ramach tych samouczków Zaktualizowaliśmy `Web.config` kilku razy. Podczas tworzenia zestawu danych `Northwind` określonym w [pierwszym samouczku](../introduction/creating-a-data-access-layer-vb.md), na przykład informacje o parametrach połączenia zostały automatycznie dodane do `Web.config` w sekcji `<connectionStrings>`. Później, na [stronach wzorcowych i](../introduction/master-pages-and-site-navigation-vb.md) w samouczku nawigacji po witrynie, firma Microsoft ręcznie zaktualizowała `Web.config`, dodając `<pages>` element wskazujący, że wszystkie strony ASP.NET w naszym projekcie powinny używać motywu `DataWebControls`.

Ponieważ `Web.config` mogą zawierać dane poufne, takie jak parametry połączenia, ważne jest, aby zawartość `Web.config` była bezpieczna i ukryta przed nieautoryzowanymi przeglądarkami. Domyślnie wszystkie żądania HTTP do pliku z rozszerzeniem `.config` są obsługiwane przez aparat ASP.NET, który zwraca *ten typ strony nie jest obsługiwany* komunikat pokazywany na rysunku 1. Oznacza to, że odwiedzający nie mogą wyświetlić zawartości pliku `Web.config`. wystarczy wprowadzić http://www.YourServer.com/Web.config do paska adresu przeglądarki.

[![odwiedzania pliku Web. config za pomocą przeglądarki zwraca komunikat tego typu nie jest obsługiwany](protecting-connection-strings-and-other-configuration-information-vb/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image1.png)

**Rysunek 1**. odwiedzenie `Web.config` za pomocą przeglądarki zwraca komunikat tego typu nie jest obsługiwany ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](protecting-connection-strings-and-other-configuration-information-vb/_static/image3.png))

Ale co zrobić, jeśli osoba atakująca będzie mogła znaleźć inne wykorzystanie, który umożliwia wyświetlanie zawartości `Web.config` pliku? Co może zrobić osoba atakująca z tymi informacjami i jakie czynności można podjąć, aby dodatkowo chronić poufne informacje w ramach `Web.config`? Na szczęście większość sekcji w `Web.config` nie zawiera informacji poufnych. Jakie szkody może wykorzystać osoba atakująca, jeśli znają nazwę motywu domyślnego używanego przez strony ASP.NET?

Niektóre `Web.config` sekcje zawierają jednak informacje poufne, które mogą obejmować parametry połączeń, nazwy użytkowników, hasła, nazwy serwerów, klucze szyfrowania i tak dalej. Te informacje są zazwyczaj dostępne w następujących `Web.config` sekcjach:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

W tym samouczku Przyjrzyjmy się technikom ochrony poufnych informacji o konfiguracji. Jak zobaczymy, .NET Framework wersja 2,0 zawiera system konfiguracji chronionej, który umożliwia programistyczne szyfrowanie i odszyfrowywanie wybranych sekcji konfiguracji w Breeze.

> [!NOTE]
> Ten samouczek zawiera zalecenia firmy Microsoft dotyczące łączenia się z bazą danych z poziomu aplikacji ASP.NET. Oprócz szyfrowania parametrów połączenia można pomóc w zabezpieczaniu systemu przez zapewnienie, że łączysz się z bazą danych w bezpieczny sposób.

## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Krok 1. Eksplorowanie opcji konfiguracji chronionych ASP.NET 2,0 s

ASP.NET 2,0 zawiera chroniony System konfiguracyjny służący do szyfrowania i odszyfrowywania informacji o konfiguracji. Obejmuje to metody w .NET Framework, których można użyć do programistycznego szyfrowania lub odszyfrowywania informacji o konfiguracji. Chroniony system konfiguracji korzysta z [modelu dostawcy](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), który umożliwia deweloperom wybór zastosowania implementacji kryptograficznej.

.NET Framework są dostarczane z dwoma chronionymi dostawcami konfiguracji:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) — używa algorytmu asymetrycznego [RSA](http://en.wikipedia.org/wiki/Rsa) do szyfrowania i odszyfrowywania.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) — używa [interfejsu API ochrony danych systemu Windows (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) do szyfrowania i odszyfrowywania.

Ponieważ chroniony system konfiguracji implementuje Wzorzec projektowy dostawcy, można utworzyć własnego, chronionego dostawcę konfiguracji i podłączyć go do aplikacji. Aby uzyskać więcej informacji na temat tego procesu, zobacz temat [implementowanie chronionego dostawcy konfiguracji](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) .

Dostawcy RSA i DPAPI używają kluczy do procedur szyfrowania i odszyfrowywania, a te klucze mogą być przechowywane na poziomie komputera lub użytkownika. Klucze na poziomie komputera są idealne dla scenariuszy, w których aplikacja sieci Web działa na własnym dedykowanym serwerze lub jeśli na serwerze istnieje wiele aplikacji, które muszą udostępniać zaszyfrowane informacje. Klucze na poziomie użytkownika są bezpieczniejszym rozwiązaniem w środowiskach hostingu udostępnionego, w których inne aplikacje na tym samym serwerze nie mogą odszyfrować sekcji konfiguracji chronionej przez aplikację.

W tym samouczku nasze przykłady spowodują użycie dostawcy DPAPI i kluczy na poziomie komputera. W szczególności zapoznaj się z sekcją szyfrowanie `<connectionStrings>` w programie `Web.config`, mimo że chroniony system konfiguracji może służyć do szyfrowania większości `Web.config` sekcji. Aby uzyskać informacje na temat korzystania z kluczy na poziomie użytkownika lub za pomocą dostawcy RSA, zapoznaj się z zasobami w sekcji dalsze informacje o odczycie na końcu tego samouczka.

> [!NOTE]
> Dostawcy `RSAProtectedConfigurationProvider` i `DPAPIProtectedConfigurationProvider` są zarejestrowani w pliku `machine.config` z nazwami dostawców `RsaProtectedConfigurationProvider` i `DataProtectionConfigurationProvider`. Podczas szyfrowania lub odszyfrowywania informacji o konfiguracji konieczne będzie podanie odpowiedniej nazwy dostawcy (`RsaProtectedConfigurationProvider` lub `DataProtectionConfigurationProvider`) zamiast rzeczywistej nazwy typu (`RSAProtectedConfigurationProvider` i `DPAPIProtectedConfigurationProvider`). Plik `machine.config` można znaleźć w folderze `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG`.

## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Krok 2. programowane szyfrowanie i odszyfrowywanie sekcji konfiguracyjnych

Za pomocą kilku wierszy kodu możemy zaszyfrować lub odszyfrować określoną sekcję konfiguracyjną przy użyciu określonego dostawcy. Kod, który będzie widoczny wkrótce, po prostu musi programistycznie odwoływać się do odpowiedniej sekcji konfiguracji, wywołać jej `ProtectSection` lub `UnprotectSection` metodę, a następnie wywołać metodę `Save`, aby zachować zmiany. Ponadto .NET Framework obejmuje pomocne narzędzie wiersza polecenia, które może szyfrować i odszyfrowywać informacje o konfiguracji. To narzędzie wiersza polecenia zostanie omówione w kroku 3.

Aby zilustrować programistyczną ochronę informacji o konfiguracji, pozwól s utworzyć stronę ASP.NET zawierającą przyciski służące do szyfrowania i odszyfrowywania sekcji `<connectionStrings>` w `Web.config`.

Zacznij od otwarcia strony `EncryptingConfigSections.aspx` w folderze `AdvancedDAL`. Przeciągnij kontrolkę TextBox z przybornika do projektanta, ustawiając jej Właściwość `ID` na `WebConfigContents`, jej Właściwość `TextMode` na `MultiLine`, a jej `Width` i `Rows` właściwości odpowiednio do 95% i 15. W tej kontrolce TextBox zostanie wyświetlona zawartość `Web.config` pozwalające szybko sprawdzić, czy zawartość jest zaszyfrowana. Oczywiście w prawdziwej aplikacji nie ma potrzeby wyświetlania zawartości `Web.config`.

Poniżej pola tekstowego Dodaj dwa kontrolki przycisku o nazwie `EncryptConnStrings` i `DecryptConnStrings`. Ustaw właściwości tekstu w taki sposób, aby szyfrować parametry połączenia i odszyfrowywać parametry połączenia.

W tym momencie ekran powinien wyglądać podobnie do rysunku 2.

[![dodać pole tekstowe i dwa kontrolki sieci Web Button do strony](protecting-connection-strings-and-other-configuration-information-vb/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image4.png)

**Rysunek 2**. Dodawanie pola tekstowego i dwóch kontrolek sieci Web na stronie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](protecting-connection-strings-and-other-configuration-information-vb/_static/image6.png))

Następnie musimy napisać kod, który ładuje i wyświetla zawartość `Web.config` w polu tekstowym `WebConfigContents` podczas pierwszego ładowania strony. Dodaj następujący kod do klasy s powiązanej z kodem. Ten kod dodaje metodę o nazwie `DisplayWebConfig` i wywołuje ją z programu obsługi zdarzeń `Page_Load`, gdy `Page.IsPostBack` jest `False`:

[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample1.vb)]

Metoda `DisplayWebConfig` używa [klasy`File`](https://msdn.microsoft.com/library/system.io.file.aspx) do otwierania pliku `Web.config` aplikacji, [klasy`StreamReader`](https://msdn.microsoft.com/library/system.io.streamreader.aspx) do odczytywania zawartości w postaci ciągu oraz [klasy`Path`](https://msdn.microsoft.com/library/system.io.path.aspx) w celu wygenerowania ścieżki fizycznej do pliku `Web.config`. Wszystkie te trzy klasy zostały znalezione w [przestrzeni nazw`System.IO`](https://msdn.microsoft.com/library/system.io.aspx). W związku z tym należy dodać instrukcję `Imports``System.IO` na początku klasy związanej z kodem lub, Alternatywnie, prefiks tych nazw klas z `System.IO.`

Następnie musimy dodać procedury obsługi zdarzeń dla dwóch formantów Button `Click` zdarzenia i dodać kod niezbędny do szyfrowania i odszyfrowywania sekcji `<connectionStrings>` przy użyciu klucza na poziomie komputera z dostawcą DPAPI. W projektancie kliknij dwukrotnie każdy z przycisków, aby dodać program obsługi zdarzeń `Click` w klasie powiązanej z kodem, a następnie Dodaj następujący kod:

[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample2.vb)]

Kod używany w dwóch programach obsługi zdarzeń jest niemal identyczny. Oba te elementy są uruchamiane przez pobranie informacji o bieżącym pliku `Web.config` aplikacji za pośrednictwem metody [`WebConfigurationManager` klasy](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [`OpenWebConfiguration`](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Ta metoda zwraca plik konfiguracji sieci Web dla określonej ścieżki wirtualnej. Następnie do sekcji `Web.config` pliku s `<connectionStrings>` można uzyskać dostęp za pośrednictwem metody [`Configuration` Class](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [`GetSection(sectionName)`](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), która zwraca obiekt [`ConfigurationSection`](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) .

Obiekt `ConfigurationSection` zawiera [właściwość`SectionInformation`](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) , która zawiera dodatkowe informacje i funkcje dotyczące sekcji konfiguracji. Jak przedstawiono powyżej kod, możemy określić, czy sekcja konfiguracji jest zaszyfrowana, sprawdzając Właściwość `SectionInformation` `IsProtected` właściwości. Ponadto sekcję można zaszyfrować lub odszyfrować za pomocą `SectionInformation` właściwości s `ProtectSection(provider)` i `UnprotectSection`.

Metoda `ProtectSection(provider)` akceptuje jako wejściowy ciąg określający nazwę chronionego dostawcy konfiguracji do użycia podczas szyfrowania. W `EncryptConnString` obsługi zdarzeń przycisku s przeszedł DataProtectionConfigurationProvider do metody `ProtectSection(provider)`, tak aby był używany dostawca DPAPI. Metoda `UnprotectSection` może określić dostawcę, który został użyty do zaszyfrowania sekcji konfiguracji i w związku z tym nie wymaga żadnych parametrów wejściowych.

Po wywołaniu metody `ProtectSection(provider)` lub `UnprotectSection`, należy wywołać [metodę`Save`](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) `Configuration` Object s, aby zachować zmiany. Gdy informacje o konfiguracji zostały zaszyfrowane lub odszyfrowane i zapisane zmiany, wywołamy `DisplayWebConfig` do załadowania zaktualizowanej zawartości `Web.config` do kontrolki TextBox.

Po wprowadzeniu powyższego kodu przetestuj go, odwiedzając stronę `EncryptingConfigSections.aspx` za pomocą przeglądarki. Początkowo powinna zostać wyświetlona strona zawierająca listę zawartości `Web.config` z sekcją `<connectionStrings>` wyświetlaną w postaci zwykłego tekstu (patrz rysunek 3).

[![dodać pole tekstowe i dwa kontrolki sieci Web Button do strony](protecting-connection-strings-and-other-configuration-information-vb/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image7.png)

**Rysunek 3**. Dodawanie pola tekstowego i dwóch kontrolek sieci Web na stronie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](protecting-connection-strings-and-other-configuration-information-vb/_static/image9.png))

Teraz kliknij przycisk Szyfruj parametry połączenia. Jeśli sprawdzanie poprawności żądania jest włączone, znacznik ogłoszony z powrotem z `WebConfigContents` TextBox spowoduje wygenerowanie `HttpRequestValidationException`, który wyświetla komunikat, dla klienta wykryto potencjalnie niebezpieczną `Request.Form` wartość. Zażądaj weryfikacji, która jest domyślnie włączona w ASP.NET 2,0, zabrania ogłaszania zwrotnego, który zawiera niezakodowany kod HTML i ma na celu zapobieganie atakom z użyciem skryptów. Ten test można wyłączyć na poziomie strony lub aplikacji. Aby wyłączyć tę stronę, należy ustawić ustawienie `ValidateRequest` na `False` w dyrektywie `@Page`. Dyrektywa `@Page` znajduje się w górnej części znacznika deklaracyjnego strony.

[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample3.aspx)]

Aby uzyskać więcej informacji na temat weryfikacji żądań, jej przeznaczenia, sposobu wyłączania na poziomie strony i aplikacji, a także sposobu kodowania kodu HTML, zobacz [Sprawdzanie poprawności żądań — zapobieganie atakom skryptów](../../../../whitepapers/request-validation.md).

Po wyłączeniu weryfikacji żądań dla strony spróbuj ponownie kliknąć przycisk Szyfruj parametry połączenia. W przypadku ogłaszania zwrotnego plik konfiguracyjny zostanie uzyskany, a jego sekcja `<connectionStrings>` zaszyfrowana przy użyciu dostawcy DPAPI. Pole tekstowe zostanie następnie zaktualizowane w celu wyświetlenia nowej zawartości `Web.config`. Jak pokazano na rysunku 4, `<connectionStrings>` informacje są teraz szyfrowane.

[![kliknięcie przycisku Szyfruj parametry połączenia szyfruje sekcję &lt;connectionString&gt;](protecting-connection-strings-and-other-configuration-information-vb/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image10.png)

**Ilustracja 4**. kliknięcie przycisku Szyfruj parametry połączenia szyfruje sekcję `<connectionString>` ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](protecting-connection-strings-and-other-configuration-information-vb/_static/image12.png))

Sekcja zaszyfrowana `<connectionStrings>` wygenerowana na komputerze jest następująca, chociaż część zawartości elementu `<CipherData>` została usunięta dla zwięzłości:

[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample4.xml)]

> [!NOTE]
> Element `<connectionStrings>` określa dostawcę używanego do przeprowadzania szyfrowania (`DataProtectionConfigurationProvider`). Te informacje są używane przez metodę `UnprotectSection`, gdy zostanie kliknięty przycisk Odszyfruj parametry połączenia.

Po uzyskaniu dostępu do informacji o parametrach połączenia z `Web.config`-przez kod, który piszesz, z formantu kontrolki SqlDataSource lub z automatycznie generowanego kodu z TableAdapters w naszych typach danych, zostanie on automatycznie odszyfrowany. W krótkim przypadku nie trzeba dodawać żadnych dodatkowych kodów ani logiki, aby odszyfrować zaszyfrowaną sekcję `<connectionString>`. Aby to zademonstrować, w tej chwili odwiedź jedną z wcześniejszych samouczków, na przykład prosty samouczek wyświetlania z sekcji podstawowe Raportowanie (`~/BasicReporting/SimpleDisplay.aspx`). Jak pokazano na rysunku 5, samouczek działa dokładnie tak, jak oczekujemy, wskazując, że informacje o zaszyfrowanych parametrach połączenia są automatycznie odszyfrowywane przez stronę ASP.NET.

[![Warstwa dostępu do danych automatycznie odszyfrowuje informacje o parametrach połączenia](protecting-connection-strings-and-other-configuration-information-vb/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image13.png)

**Rysunek 5**. Warstwa dostępu do danych automatycznie odszyfrowuje informacje o parametrach połączenia ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](protecting-connection-strings-and-other-configuration-information-vb/_static/image15.png))

Aby przywrócić sekcję `<connectionStrings>` z powrotem do swojej reprezentacji w postaci zwykłego tekstu, kliknij przycisk Odszyfruj parametry połączenia. Na stronie ogłaszania zwrotnego należy zobaczyć parametry połączenia w `Web.config` w postaci zwykłego tekstu. W tym momencie ekran powinien wyglądać tak, jak podczas pierwszej wizyty na tej stronie (zobacz rysunek 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnet_regiisexe"></a>Krok 3. szyfrowanie sekcji konfiguracji za pomocą`aspnet_regiis.exe`

.NET Framework obejmuje wiele narzędzi wiersza polecenia w folderze `$WINDOWS$\Microsoft.NET\Framework\version\`. W samouczku [Używanie zależności pamięci podręcznej SQL](../caching-data/using-sql-cache-dependencies-vb.md) można na przykład korzystać z narzędzia wiersza polecenia `aspnet_regsql.exe`, aby dodać infrastrukturę niezbędną dla zależności pamięci podręcznej SQL. Innym przydatnym narzędziem wiersza polecenia w tym folderze jest [Narzędzie rejestracji ASP.NET IIS (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Ponieważ jego nazwa to oznacza, Narzędzie rejestracji ASP.NET IIS jest używane przede wszystkim do zarejestrowania aplikacji ASP.NET 2,0 z serwerem sieci Web Microsoft s Professional klasy usług IIS. Oprócz funkcji związanych z usługami IIS można także używać narzędzia rejestracji ASP.NET IIS do szyfrowania lub odszyfrowywania określonych sekcji konfiguracyjnych w `Web.config`.

Poniższa instrukcja pokazuje ogólną składnię używaną do szyfrowania sekcji konfiguracji za pomocą narzędzia wiersza polecenia `aspnet_regiis.exe`:

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample5.cmd)]

*sekcja* to sekcja konfiguracyjna do zaszyfrowania (na przykład connectionStrings), *katalog fizyczny\_* jest pełną ścieżką fizyczną do katalogu głównego aplikacji sieci Web, a *dostawcą* jest nazwa chronionego dostawcy konfiguracji do użycia (na przykład DataProtectionConfigurationProvider). Alternatywnie, jeśli aplikacja sieci Web jest zarejestrowana w usługach IIS, można wprowadzić ścieżkę wirtualną zamiast ścieżki fizycznej przy użyciu następującej składni:

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample6.cmd)]

Poniższy `aspnet_regiis.exe` przykład szyfruje sekcję `<connectionStrings>` przy użyciu dostawcy DPAPI z kluczem na poziomie komputera:

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample7.cmd)]

Podobnie narzędzie wiersza polecenia `aspnet_regiis.exe` może służyć do odszyfrowywania sekcji konfiguracyjnych. Zamiast używać przełącznika `-pef`, użyj `-pdf` (lub zamiast `-pe`, użyj `-pd`). Należy również pamiętać, że nazwa dostawcy nie jest konieczna podczas odszyfrowywania.

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample8.cmd)]

> [!NOTE]
> Ponieważ korzystamy z dostawcy DPAPI, który używa kluczy specyficznych dla komputera, musisz uruchomić `aspnet_regiis.exe` z tego samego komputera, z którego są obsługiwane strony sieci Web. Jeśli na przykład uruchomisz ten program wiersza polecenia z lokalnej maszyny deweloperskiej, a następnie przekażesz zaszyfrowany plik Web. config na serwer produkcyjny, serwer produkcyjny nie będzie w stanie odszyfrować informacji o parametrach połączenia, ponieważ zostały one zaszyfrowane Korzystanie z kluczy specyficznych dla komputera deweloperskiego. Dostawca RSA nie ma tego ograniczenia, ponieważ możliwe jest wyeksportowanie kluczy RSA do innej maszyny.

## <a name="understanding-database-authentication-options"></a>Informacje na temat opcji uwierzytelniania bazy danych

Zanim aplikacja będzie mogła wystawić `SELECT`, `INSERT`, `UPDATE`lub `DELETE` zapytań do bazy danych Microsoft SQL Server, najpierw należy zidentyfikować żądaną bazę danych. Ten proces jest znany jako *uwierzytelnianie* , a SQL Server zapewnia dwie metody uwierzytelniania:

- **Uwierzytelnianie systemu Windows** — proces, w którym uruchomiona jest aplikacja, jest używany do komunikacji z bazą danych. Podczas uruchamiania aplikacji ASP.NET za pomocą programu Visual Studio 2005 s ASP.NET Development Server aplikacja ASP.NET zakłada tożsamość aktualnie zalogowanego użytkownika. W przypadku aplikacji ASP.NET na serwerze Microsoft Internet Information Server (IIS) aplikacje ASP.NET zazwyczaj zakładają tożsamość `domainName``\MachineName` lub `domainName``\NETWORK SERVICE`, chociaż można je dostosować.
- **Uwierzytelnianie SQL** — wartości identyfikatora użytkownika i hasła są podawane jako poświadczenia na potrzeby uwierzytelniania. W przypadku uwierzytelniania SQL identyfikator użytkownika i hasło są podawane w parametrach połączenia.

Uwierzytelnianie systemu Windows jest preferowane w porównaniu z uwierzytelnianiem SQL, ponieważ jest bezpieczniejsze. Przy użyciu uwierzytelniania systemu Windows parametry połączenia są bezpłatne z nazwy użytkownika i hasła, a jeśli serwer sieci Web i serwer bazy danych znajdują się na dwóch różnych komputerach, poświadczenia nie są wysyłane przez sieć w postaci zwykłego tekstu. W przypadku uwierzytelniania SQL poświadczenia uwierzytelniania są jednak trwale kodowane w parametrach połączenia i są przesyłane z serwera sieci Web do serwera bazy danych w postaci zwykłego tekstu.

Te samouczki używały uwierzytelniania systemu Windows. Możesz określić, jaki tryb uwierzytelniania jest używany, sprawdzając parametry połączenia. Parametry połączenia w `Web.config` dla naszych samouczków:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Zintegrowane zabezpieczenia = true i brak nazwy użytkownika i hasła wskazują, że jest używane uwierzytelnianie systemu Windows. W niektórych parametrach połączenia jest używany termin zaufane połączenie = tak lub Integrated Security = SSPI zamiast zintegrowanych zabezpieczeń = true, ale wszystkie trzy wskazują użycie uwierzytelniania systemu Windows.

Poniższy przykład przedstawia parametry połączenia używające uwierzytelniania SQL. Zanotuj poświadczenia osadzone w parametrach połączenia:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Załóżmy, że osoba atakująca może wyświetlić plik `Web.config` aplikacji. W przypadku korzystania z uwierzytelniania SQL w celu nawiązania połączenia z bazą danych, która jest dostępna za pośrednictwem Internetu, osoba atakująca może użyć tych parametrów połączenia w celu nawiązania połączenia z bazą danych za pośrednictwem programu SQL Management Studio lub ze stron ASP.NET w ich własnej witrynie sieci Web. Aby pomóc w ograniczeniu tego zagrożenia, Zaszyfruj informacje o parametrach połączenia w `Web.config` przy użyciu chronionego systemu konfiguracji.

> [!NOTE]
> Aby uzyskać więcej informacji na temat różnych typów uwierzytelniania dostępnych w SQL Server, zobacz [Tworzenie bezpiecznych aplikacji ASP.NET: uwierzytelnianie, autoryzacja i Bezpieczna komunikacja](https://msdn.microsoft.com/library/aa302392.aspx). Aby uzyskać dalsze przykłady parametrów połączenia ilustrujące różnice między składnią uwierzytelniania systemu Windows i programu SQL Server, zobacz [connectionStrings.com](http://www.connectionstrings.com/).

## <a name="summary"></a>Podsumowanie

Domyślnie nie można uzyskać dostępu do plików z rozszerzeniem `.config` w aplikacji ASP.NET za pomocą przeglądarki. Te typy plików nie są zwracane, ponieważ mogą zawierać informacje poufne, takie jak parametry połączenia bazy danych, nazwy użytkowników i hasła itd. Chroniony system konfiguracji w programie .NET 2,0 ułatwia ochronę poufnych informacji przez umożliwienie szyfrowania określonych sekcji konfiguracyjnych. Istnieją dwa wbudowane dostawcy konfiguracji chronionej: jeden, który używa algorytmu RSA i jednego, który używa interfejsu API ochrony danych systemu Windows (DPAPI).

W tym samouczku pokazano, jak szyfrować i odszyfrowywać ustawienia konfiguracji przy użyciu dostawcy DPAPI. Można to zrobić programowo, jak pokazano w kroku 2, a także za pomocą narzędzia wiersza polecenia `aspnet_regiis.exe`, które zostało omówione w kroku 3. Aby uzyskać więcej informacji na temat używania kluczy na poziomie użytkownika lub zamiast nich przy użyciu dostawcy RSA, zapoznaj się z tematem zasoby w dalszej części odczytywania.

Szczęśliwe programowanie!

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Tworzenie bezpiecznej aplikacji ASP.NET: uwierzytelnianie, autoryzacja i Bezpieczna komunikacja](https://msdn.microsoft.com/library/aa302392.aspx)
- [Szyfrowanie informacji o konfiguracji w aplikacjach ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Szyfrowanie wartości `Web.config` w ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Instrukcje: szyfrowanie sekcji konfiguracyjnych w ASP.NET 2,0 przy użyciu funkcji DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Instrukcje: szyfrowanie sekcji konfiguracyjnych w ASP.NET 2,0 przy użyciu RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [Interfejs API konfiguracji w programie .NET 2,0](http://www.odetocode.com/Articles/418.aspx)
- [Ochrona danych systemu Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzenci liderzy dla tego samouczka to Teresa Murphy i Randy Schmidt. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
> [dalej](debugging-stored-procedures-vb.md)

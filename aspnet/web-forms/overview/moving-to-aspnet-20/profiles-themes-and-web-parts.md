---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Profile, motywy i składniki Web Part | Microsoft Docs
author: microsoft
description: Istnieją istotne zmiany w konfiguracji i Instrumentacji w programie ASP.NET 2,0. Nowy interfejs API konfiguracji ASP.NET umożliwia zmianę konfiguracji żądania ściągnięcia...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: cf5c45781be6d003d28c6aa27efa08032579a6dd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587235"
---
# <a name="profiles-themes-and-web-parts"></a>Profile, motywy i składniki Web Part

przez [firmę Microsoft](https://github.com/microsoft)

> Istnieją istotne zmiany w konfiguracji i Instrumentacji w programie ASP.NET 2,0. Nowy interfejs API konfiguracji ASP.NET umożliwia programistyczne wprowadzanie zmian w konfiguracji. Ponadto istnieją liczne nowe ustawienia konfiguracji, które pozwalają na nowe konfiguracje i instrumentację.

ASP.NET 2,0 reprezentuje znaczną poprawę w obszarze spersonalizowanych witryn sieci Web. Oprócz funkcji członkostwa, które zostały już omówione, ASP.NET Profile, motywy i składniki Web Part znacząco ulepszają personalizację w witrynach sieci Web.

## <a name="aspnet-profiles"></a>Profile ASP.NET

Profile ASP.NET są podobne do sesji. Różnica polega na tym, że profil jest trwały, podczas gdy przeglądarka zostanie utracona. Kolejną dużą różnicą między sesjami i profilami jest to, że profile są silnie wpisywane, dlatego udostępniasz IntelliSense podczas procesu tworzenia.

Profil jest zdefiniowany w pliku konfiguracyjnym maszyny lub w pliku Web. config aplikacji. (Nie można zdefiniować profilu w pliku Web. config podfolderów.) Poniższy kod definiuje profil do przechowywania odwiedzających witrynę sieci Web imię i nazwisko.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Domyślnym typem danych dla właściwości profilu jest system. String. W powyższym przykładzie nie określono żadnego typu danych. W związku z tym właściwości FirstName i LastName są typu String. Jak wspomniano wcześniej, właściwości profilu są jednoznacznie wpisane. Poniższy kod dodaje nową właściwość dla wieku typu Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Profile są zwykle używane z uwierzytelnianiem formularzy ASP.NET. W połączeniu z uwierzytelnianiem formularzy każdy użytkownik ma oddzielny profil skojarzony z ich IDENTYFIKATORem użytkownika. Można jednak zezwolić na używanie profilów w aplikacji anonimowej przy użyciu &lt;anonymousIdentification&gt; elementu w pliku konfiguracji wraz z atrybutem **AllowAnonymous** , jak pokazano poniżej:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Gdy użytkownik anonimowy przegląda witrynę, ASP.NET tworzy wystąpienie **ProfileCommon** dla użytkownika. Ten profil używa unikatowego identyfikatora przechowywanego w pliku cookie w przeglądarce w celu zidentyfikowania użytkownika jako unikatowego gościa. W ten sposób można przechowywać informacje o profilu dla użytkowników, którzy przeglądają anonimowo.

## <a name="profile-groups"></a>Grupy profilów

Istnieje możliwość pogrupowania właściwości profilów. Grupowanie właściwości umożliwia symulowanie wielu profilów dla określonej aplikacji.

Poniższa konfiguracja umożliwia skonfigurowanie właściwości FirstName i LastName dla dwóch grup; Nabywcy i potencjalni klienci.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Następnie można ustawić właściwości dla określonej grupy w następujący sposób:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Przechowywanie obiektów złożonych

Do tej pory pokryte przykłady zawierają proste typy danych w profilu. Istnieje również możliwość przechowywania złożonych typów danych w profilu przez określenie metody serializacji przy użyciu atrybutu **SerializeAs** w następujący sposób:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

W tym przypadku typem jest PurchaseInvoice. Klasa PurchaseInvoice musi być oznaczona jako SERIALIZABLE i może zawierać dowolną liczbę właściwości. Na przykład jeśli PurchaseInvoice ma właściwość o nazwie **NumItemsPurchased**, można odwołać się do tej właściwości w kodzie w następujący sposób:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Dziedziczenie profilu

Istnieje możliwość utworzenia profilu do użycia w wielu aplikacjach. Tworząc klasę profilu, która pochodzi od ProfileBase, można ponownie użyć profilu w kilku aplikacjach przy użyciu atrybutu **Inherits** , jak pokazano poniżej:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

W takim przypadku Klasa **PurchasingProfile** będzie wyglądać następująco:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Dostawcy profilów

Profile ASP.NET używają modelu dostawcy. Dostawca domyślny przechowuje informacje w bazie danych SQL Server Express w folderze\_danych aplikacji sieci Web przy użyciu dostawcy SqlProfileProvider. Jeśli baza danych nie istnieje, ASP.NET automatycznie utworzy ją, gdy profil próbuje zapisać informacje.

Jednak w niektórych przypadkach warto opracować własnego dostawcę profilów. Funkcja profilu ASP.NET umożliwia łatwe korzystanie z różnych dostawców.

Niestandardowy dostawca profilu jest tworzony, gdy:

- Należy przechowywać informacje o profilu w źródle danych, na przykład w bazie danych programu FoxPro lub w bazie danych Oracle, która nie jest obsługiwana przez dostawców profilów uwzględnionych w .NET Framework.
- Należy zarządzać informacjami o profilu przy użyciu schematu bazy danych innego niż schemat bazy danych używany przez dostawców dołączonych do .NET Framework. Typowym przykładem jest to, że chcesz zintegrować informacje o profilu z danymi użytkowników w istniejącej bazie danych SQL Server.

### <a name="required-classes"></a>Wymagane klasy

Aby zaimplementować dostawcę profilu, należy utworzyć klasę, która dziedziczy klasę abstrakcyjną system. Web. profile. ProfileProvider. Klasa abstrakcyjna **ProfileProvider** z kolei dziedziczy klasę abstrakcyjną system. Configuration. SettingsProvider, która dziedziczy klasę abstrakcyjną system. Configuration. Provider. ProviderBase. Ze względu na ten łańcuch dziedziczenia, oprócz wymaganych elementów członkowskich klasy **ProfileProvider** , należy zaimplementować wymagane elementy członkowskie klas **SettingsProvider** i **ProviderBase** .

W poniższych tabelach opisano właściwości i metody, które należy zaimplementować z klas abstrakcyjnych **ProviderBase**, **SettingsProvider**i **ProfileProvider** .

### <a name="providerbase-members"></a>ProviderBase członkowie

| **Członkiem** | **Opis** |
| --- | --- |
| Initialize — Metoda | Przyjmuje jako dane wejściowe nazwę wystąpienia dostawcy i NameValueCollection ustawień konfiguracji. Służy do ustawiania opcji i wartości właściwości dla wystąpienia dostawcy, w tym wartości specyficznych dla implementacji i opcji określonych w pliku konfiguracji komputera lub Web. config. |

### <a name="settingsprovider-members"></a>SettingsProvider członkowie

| **Członkiem** | **Opis** |
| --- | --- |
| Właściwość ApplicationName | Nazwa aplikacji, która jest przechowywana z każdym profilem. Dostawca profilu używa nazwy aplikacji do przechowywania informacji o profilu osobno dla każdej aplikacji. Dzięki temu wiele aplikacji ASP.NET może używać tego samego źródła danych bez konfliktu, jeśli ta sama nazwa użytkownika zostanie utworzona w różnych aplikacjach. Alternatywnie wiele aplikacji ASP.NET może współużytkować źródło danych profilu, określając tę samą nazwę aplikacji. |
| GetPropertyValues — Metoda | Przyjmuje jako dane wejściowe SettingsContext i obiekt SettingsPropertyCollection. **SettingsContext** zawiera informacje o użytkowniku. Możesz użyć informacji jako klucza podstawowego do pobrania informacji o właściwościach profilu dla użytkownika. Użyj obiektu **SettingsContext** , aby uzyskać nazwę użytkownika i określić, czy użytkownik jest uwierzytelniany, czy anonimowy. **SettingsPropertyCollection** zawiera kolekcję obiektów SettingsProperty. Każdy obiekt **SettingsProperty** zawiera nazwę i typ właściwości oraz informacje dodatkowe, takie jak wartość domyślna właściwości i czy właściwość jest tylko do odczytu. Metoda **GetPropertyValues** wypełnia SettingsPropertyValueCollection z obiektami SettingsPropertyValue na podstawie obiektów **SettingsProperty** dostarczonych jako dane wejściowe. Wartości ze źródła danych dla określonego użytkownika są przypisywane do właściwości PropertyValue dla każdego obiektu **SettingsPropertyValue** i zwracana jest cała kolekcja. Wywołanie metody powoduje także zaktualizowanie wartości LastActivityDate dla określonego profilu użytkownika do bieżącej daty i godziny. |
| SetPropertyValues — Metoda | Przyjmuje jako dane wejściowe **SettingsContext** i obiekt **SettingsPropertyValueCollection** . **SettingsContext** zawiera informacje o użytkowniku. Możesz użyć informacji jako klucza podstawowego do pobrania informacji o właściwościach profilu dla użytkownika. Użyj obiektu **SettingsContext** , aby uzyskać nazwę użytkownika i określić, czy użytkownik jest uwierzytelniany, czy anonimowy. **SettingsPropertyValueCollection** zawiera kolekcję obiektów **SettingsPropertyValue** . Każdy obiekt **SettingsPropertyValue** zawiera nazwę, typ i wartość właściwości oraz informacje dodatkowe, takie jak wartość domyślna właściwości i czy właściwość jest tylko do odczytu. Metoda **SetPropertyValues** aktualizuje wartości właściwości profilu w źródle danych dla określonego użytkownika. Wywołanie metody także aktualizuje wartości **LastActivityDate** i LastUpdatedDate dla określonego profilu użytkownika do bieżącej daty i godziny. |

### <a name="profileprovider-members"></a>ProfileProvider członkowie

| **Członkiem** | **Opis** |
| --- | --- |
| DeleteProfiles, Metoda | Przyjmuje jako dane wejściowe tablicę nazw użytkowników i usuwa ze źródła danych wszystkie informacje o profilu i wartości właściwości dla określonych nazw, gdzie nazwa aplikacji jest zgodna z wartością właściwości **ApplicationName** . Jeśli źródło danych obsługuje transakcje, zaleca się, aby uwzględnić wszystkie operacje usuwania w transakcji i wycofać transakcję i zgłosić wyjątek w przypadku niepowodzenia operacji usuwania. |
| DeleteProfiles, Metoda | Przyjmuje jako dane wejściowe kolekcja obiektów ProfileInfo i usuwa ze źródła danych wszystkie informacje o profilu i wartości właściwości dla każdego profilu, gdzie nazwa aplikacji jest zgodna z wartością właściwości **ApplicationName** . Jeśli źródło danych obsługuje transakcje, zaleca się, aby uwzględnić wszystkie operacje usuwania w transakcji i wycofać transakcję i zgłosić wyjątek w przypadku niepowodzenia operacji usuwania. |
| DeleteInactiveProfiles, Metoda | Przyjmuje jako dane wejściowe wartość ProfileAuthenticationOption i obiekt DateTime i usuwa ze źródła danych wszystkie informacje o profilu i wartości właściwości, w których Data ostatniego działania jest mniejsza lub równa określonej dacie i godzinie oraz gdzie nazwa aplikacji jest zgodna z wartością właściwości **ApplicationName** . Parametr **ProfileAuthenticationOption** określa, czy mają być usuwane tylko profile anonimowe, profile uwierzytelnione lub wszystkie profile. Jeśli źródło danych obsługuje transakcje, zaleca się, aby uwzględnić wszystkie operacje usuwania w transakcji i wycofać transakcję i zgłosić wyjątek w przypadku niepowodzenia operacji usuwania. |
| GetAllProfiles, Metoda | Przyjmuje jako dane wejściowe wartość **ProfileAuthenticationOption** , liczbę całkowitą określającą indeks strony, liczbę całkowitą, która określa rozmiar strony i odwołanie do liczby całkowitej, która zostanie ustawiona na łączną liczbę profilów. Zwraca ProfileInfoCollection, który zawiera obiekty **ProfileInfo** dla wszystkich profilów w źródle danych, gdzie nazwa aplikacji jest zgodna z wartością właściwości **ApplicationName** . Parametr **ProfileAuthenticationOption** określa, czy mają być zwracane tylko profile anonimowe, profile uwierzytelnione lub wszystkie profile. Wyniki zwrócone przez metodę **GetAllProfiles** są ograniczone przez wartości indeksu strony i rozmiaru strony. Wartość rozmiaru strony określa maksymalną liczbę obiektów **ProfileInfo** do zwrócenia w **ProfileInfoCollection**. Wartość indeksu strony określa, która strona wyników ma zostać zwrócona, gdzie 1 identyfikuje pierwszą stronę. Parametr łącznego rekordu jest parametrem out (można użyć elementu **ByRef** w Visual Basic), który jest ustawiony na łączną liczbę profilów. Na przykład jeśli magazyn danych zawiera 13 profilów dla aplikacji, a wartość indeksu strony to 2 z rozmiarem strony wynoszącym 5, zwracana **ProfileInfoCollection** zawiera szóste profile. Całkowita wartość rekordów jest ustawiana na 13, gdy metoda zwraca. |
| GetAllInactiveProfiles, Metoda | Przyjmuje jako dane wejściowe wartość **ProfileAuthenticationOption** , obiekt **DateTime** , liczbę całkowitą określającą indeks strony, liczbę całkowitą określającą rozmiar strony i odwołanie do liczby całkowitej, która zostanie ustawiona na łączną liczbę profilów. Zwraca element **ProfileInfoCollection** , który zawiera obiekty **ProfileInfo** dla wszystkich profilów w źródle danych, w których Data ostatniego działania jest mniejsza lub równa określonej wartości **daty** i lokalizacji, w której nazwa aplikacji jest zgodna z wartością właściwości **ApplicationName** . Parametr **ProfileAuthenticationOption** określa, czy mają być zwracane tylko profile anonimowe, profile uwierzytelnione lub wszystkie profile. Wyniki zwrócone przez metodę **GetAllInactiveProfiles** są ograniczone przez wartości indeksu strony i rozmiaru strony. Wartość rozmiaru strony określa maksymalną liczbę obiektów **ProfileInfo** do zwrócenia w **ProfileInfoCollection**. Wartość indeksu strony określa, która strona wyników ma zostać zwrócona, gdzie 1 identyfikuje pierwszą stronę. Parametr łącznego rekordu jest parametrem out (można użyć elementu **ByRef** w Visual Basic), który jest ustawiony na łączną liczbę profilów. Na przykład jeśli magazyn danych zawiera 13 profilów dla aplikacji, a wartość indeksu strony to 2 z rozmiarem strony wynoszącym 5, zwracana **ProfileInfoCollection** zawiera szóste profile. Całkowita wartość rekordów jest ustawiana na 13, gdy metoda zwraca. |
| FindProfilesByUserName, Metoda | Przyjmuje jako dane wejściowe **ProfileAuthenticationOption** wartość, ciąg zawierający nazwę użytkownika, liczbę całkowitą, która określa indeks strony, liczbę całkowitą, która określa rozmiar strony i odwołanie do liczby całkowitej, która zostanie ustawiona na łączną liczbę profilów. Zwraca **ProfileInfoCollection** , który zawiera obiekty **ProfileInfo** dla wszystkich profilów w źródle danych, gdzie nazwa użytkownika jest zgodna z określoną nazwą użytkownika i gdzie nazwa aplikacji jest zgodna z wartością właściwości **ApplicationName** . Parametr **ProfileAuthenticationOption** określa, czy mają być zwracane tylko profile anonimowe, profile uwierzytelnione lub wszystkie profile. Jeśli źródło danych obsługuje dodatkowe możliwości wyszukiwania, takie jak symbole wieloznaczne, można zapewnić bardziej rozbudowane możliwości wyszukiwania nazw użytkowników. Wyniki zwrócone przez metodę **FindProfilesByUserName** są ograniczone przez wartości indeksu strony i rozmiaru strony. Wartość rozmiaru strony określa maksymalną liczbę obiektów **ProfileInfo** do zwrócenia w **ProfileInfoCollection**. Wartość indeksu strony określa, która strona wyników ma zostać zwrócona, gdzie 1 identyfikuje pierwszą stronę. Parametr łącznego rekordu jest parametrem out (można użyć elementu **ByRef** w Visual Basic), który jest ustawiony na łączną liczbę profilów. Na przykład jeśli magazyn danych zawiera 13 profilów dla aplikacji, a wartość indeksu strony to 2 z rozmiarem strony wynoszącym 5, zwracana **ProfileInfoCollection** zawiera szóste profile. Całkowita wartość rekordów jest ustawiana na 13, gdy metoda zwraca. |
| FindInactiveProfilesByUserName method | Przyjmuje jako dane wejściowe **ProfileAuthenticationOption** wartość, ciąg zawierający nazwę użytkownika, obiekt **DateTime** , liczbę całkowitą określającą indeks strony, liczbę całkowitą, która określa rozmiar strony i odwołanie do liczby całkowitej, która zostanie ustawiona na łączną liczbę profilów. Zwraca element **ProfileInfoCollection** , który zawiera obiekty **ProfileInfo** dla wszystkich profilów w źródle danych, w których nazwa użytkownika jest zgodna z określoną nazwą użytkownika, gdzie Data ostatniego działania jest mniejsza lub równa określonej wartości **daty**i gdzie nazwa aplikacji jest zgodna z wartością właściwości **ApplicationName** . Parametr **ProfileAuthenticationOption** określa, czy mają być zwracane tylko profile anonimowe, profile uwierzytelnione lub wszystkie profile. Jeśli źródło danych obsługuje dodatkowe możliwości wyszukiwania, takie jak symbole wieloznaczne, można zapewnić bardziej rozbudowane możliwości wyszukiwania nazw użytkowników. Wyniki zwrócone przez metodę **FindInactiveProfilesByUserName** są ograniczone przez wartości indeksu strony i rozmiaru strony. Wartość rozmiaru strony określa maksymalną liczbę obiektów **ProfileInfo** do zwrócenia w **ProfileInfoCollection**. Wartość indeksu strony określa, która strona wyników ma zostać zwrócona, gdzie 1 identyfikuje pierwszą stronę. Parametr łącznego rekordu jest parametrem out (można użyć elementu **ByRef** w Visual Basic), który jest ustawiony na łączną liczbę profilów. Na przykład jeśli magazyn danych zawiera 13 profilów dla aplikacji, a wartość indeksu strony to 2 z rozmiarem strony wynoszącym 5, zwracana **ProfileInfoCollection** zawiera szóste profile. Całkowita wartość rekordów jest ustawiana na 13, gdy metoda zwraca. |
| GetNumberOfInActiveProfiles, Metoda | Przyjmuje jako dane wejściowe wartość **ProfileAuthenticationOption** i obiekt **DateTime** oraz zwraca liczbę wszystkich profilów w źródle danych, w których Data ostatniego działania jest mniejsza lub równa określonej wartości **DateTime** i gdzie nazwa aplikacji jest zgodna z wartością właściwości **ApplicationName** . Parametr **ProfileAuthenticationOption** określa, czy mają być zliczane tylko profile anonimowe, profile uwierzytelnione lub wszystkie profile. |

### <a name="applicationname"></a>ApplicationName

Ponieważ dostawcy profilów przechowują informacje o profilu osobno dla każdej aplikacji, należy się upewnić, że schemat danych zawiera nazwę aplikacji oraz że zapytania i aktualizacje zawierają również nazwę aplikacji. Na przykład następujące polecenie służy do pobierania wartości właściwości z bazy danych na podstawie nazwy użytkownika i tego, czy profil jest anonimowy, i zapewnia, że wartość **ApplicationName** jest uwzględniona w zapytaniu.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>Motywy ASP.NET

## <a name="what-are-aspnet-20-themes"></a>Co to są motywy ASP.NET 2,0?

Jednym z najważniejszych aspektów aplikacji sieci Web jest spójny wygląd i działanie w całej lokacji. ASP.NET 1. x deweloperzy zazwyczaj używają kaskadowe arkusze stylów (CSS) do implementowania spójnego wyglądu i działania. Motywy ASP.NET 2,0 znacząco poprawiają się w języku CSS, ponieważ dają deweloperom ASP.NET możliwość definiowania wyglądu formantów serwera ASP.NET oraz elementów HTML. Motywy ASP.NET można zastosować do poszczególnych kontrolek, konkretnej strony sieci Web lub całej aplikacji sieci Web. Motywy używają kombinacji plików CSS, opcjonalnego pliku skórki i katalogu obrazów opcjonalnych, jeśli są one zbędne. Plik skórki steruje wyglądem kontrolek serwera ASP.NET.

## <a name="where-are-themes-stored"></a>Gdzie są przechowywane motywy?

Lokalizacja, w której są przechowywane motywy, różni się w zależności od ich zakresu. Motywy, które można zastosować do dowolnej aplikacji, są przechowywane w następującym folderze:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Motyw, który jest specyficzny dla konkretnej aplikacji, jest przechowywany w katalogu `App\_Themes\<Theme\_Name>` w katalogu głównym witryny sieci Web.

> [!NOTE]
> Plik skórki powinien modyfikować tylko właściwości kontrolki serwera, które mają wpływ na wygląd.

Motyw globalny to motyw, który można zastosować do dowolnej aplikacji lub witryny sieci Web działającej na serwerze sieci Web. Te motywy są domyślnie przechowywane w katalogu ASP. NETClientfiles\Themes, który znajduje się w katalogu v2. x. xxxxx. Alternatywnie możesz przenieść pliki motywu do ASPNET\_Client/System\_Web/[Version]/Themes/[Theme\_Name] w folderze głównym witryny sieci Web.

Motywy specyficzne dla aplikacji mogą być stosowane tylko do aplikacji, w której znajdują się pliki. Te pliki są przechowywane w katalogu `App\_Themes/<theme\_name>` w folderze głównym witryny sieci Web.

## <a name="the-components-of-a-theme"></a>Składniki motywu

Kompozycja składa się z jednego lub więcej plików CSS, opcjonalnego pliku skórki i folderu opcjonalne obrazy. Pliki CSS mogą mieć dowolną nazwę (np. default. CSS lub Theme. css itp.) i muszą znajdować się w folderze głównym folderu motywy. Pliki CSS są używane do definiowania zwykłych klas CSS i atrybutów dla określonych selektorów. Aby zastosować jedną z klas CSS do elementu Page, używana jest właściwość **CSSClass** .

Plik skórki jest plikiem XML zawierającym definicje właściwości dla kontrolek serwera ASP.NET. Kod wymieniony poniżej to przykładowy plik skórki.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

Na **rysunku 1** poniżej zostanie wyświetlona mała strona ASP.neta bez zastosowanego motywu. **Rysunek 2** przedstawia ten sam plik z zastosowanym motywem. Kolor tła i kolor tekstu są konfigurowane za pośrednictwem pliku CSS. Wygląd przycisku i pola tekstowego są konfigurowane przy użyciu pliku skórki wymienionego powyżej.

![Brak motywu](profiles-themes-and-web-parts/_static/image1.gif)

**Rysunek 1**. Brak motywu

![Zastosowano motyw](profiles-themes-and-web-parts/_static/image2.gif)

**Rysunek 2**. zastosowano motyw

Wymieniony powyżej plik skórki definiuje domyślną skórkę dla wszystkich formantów TextBox i kontrolek Button. Oznacza to, że każda kontrolka TextBox i formant Button wstawiony na stronie zajmie się tym wyglądem. Można również zdefiniować skórkę, która może być stosowana do określonych wystąpień tych formantów przy użyciu właściwości **SkinID** formantu.

Poniższy kod definiuje skórkę kontrolki Button. Tylko kontrolki przycisku z właściwością SkinID **goButton** będą miały wpływ na wygląd karnacji.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Dla każdego typu formantu serwera można mieć tylko jedną domyślną skórkę. Jeśli potrzebujesz dodatkowych karnacji, należy użyć właściwości SkinID.

## <a name="applying-themes-to-pages"></a>Stosowanie motywów do stron

Motyw można zastosować przy użyciu jednej z następujących metod:

- Na stronie &lt;&gt; elementu pliku Web. config
- W @Page dyrektywie strony
- Programistyczne

## <a name="applying-a-theme-in-the-configuration-file"></a>Stosowanie motywu w pliku konfiguracji

Aby zastosować motyw w pliku konfiguracyjnym aplikacji, należy użyć następującej składni:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Nazwa motywu określona w tym miejscu musi być zgodna z nazwą folderu motywów. Ten folder może istnieć w jednej z lokalizacji wymienionych wcześniej w tym kursie. Jeśli spróbujesz zastosować nieistniejący motyw, wystąpi błąd konfiguracji.

## <a name="applying-a-theme-in-the-page-directive"></a>Stosowanie motywu w dyrektywie page

Możesz również zastosować motyw w dyrektywie @ page. Ta metoda umożliwia użycie motywu dla określonej strony.

Aby zastosować motyw w dyrektywie @Page, należy użyć następującej składni:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Motyw określony w tym miejscu musi być zgodny z folderem motywu, jak wspomniano wcześniej. Jeśli spróbujesz zastosować nieistniejący motyw, wystąpi błąd kompilacji. Program Visual Studio wyróżni również atrybut i powiadomi użytkownika, że nie istnieje taki motyw.

## <a name="applying-a-theme-programmatically"></a>Programistyczne stosowanie motywu

Aby programowo zastosować motyw, należy określić właściwość **motyw** dla strony na **stronie\_metodę preinicjowania** .

Aby programowo zastosować motyw, należy użyć następującej składni:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Należy zastosować motyw w metodzie preinicjowania ze względu na cykl życia strony. Jeśli zastosujesz go po tym momencie, kompozycja stron zostanie już zastosowana przez środowisko uruchomieniowe, a zmiana w tym punkcie zostanie zbyt opóźniona w cyklu życia. Jeśli zastosujesz nieistniejący motyw, wystąpi wyjątek **HttpException** . Gdy motyw jest stosowany programowo, pojawi się ostrzeżenie kompilacji, jeśli wszystkie kontrolki serwera mają określoną właściwość SkinID. To ostrzeżenie jest przeznaczone do informowania o tym, że żaden motyw nie został deklaratywnie zastosowany i można go zignorować.

## <a name="exercise-1--applying-a-theme"></a>Ćwiczenie 1: stosowanie motywu

W tym ćwiczeniu utworzysz motyw ASP.NET do witryny sieci Web.

> [!IMPORTANT]
> Jeśli używasz programu Microsoft Word do wprowadzania informacji do pliku skórki, pamiętaj, aby nie zamieniać zwykłych cudzysłowów z inteligentnymi cudzysłowami. Inteligentne cudzysłowy spowodują problemy z plikami karnacji.

1. Utwórz nową witrynę sieci Web ASP.NET.
2. Kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań i wybierz polecenie Dodaj nowy element.
3. Wybierz z listy plików plik konfiguracja sieci Web i kliknij przycisk Dodaj.
4. Kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań i wybierz polecenie Dodaj nowy element.
5. Wybierz plik skórki, a następnie kliknij przycisk Dodaj.
6. Kliknij przycisk tak po wyświetleniu monitu, jeśli chcesz umieścić plik w folderze motywy\_aplikacji.
7. Kliknij prawym przyciskiem myszy folder SkinFile w folderze motywy\_aplikacji w Eksplorator rozwiązań a następnie wybierz pozycję Dodaj nowy element.
8. Wybierz arkusz stylów z listy plików i kliknij przycisk Dodaj. Masz teraz wszystkie pliki niezbędne do zaimplementowania nowego motywu. Jednak program Visual Studio ma nazwę folder motywów SkinFile. Kliknij prawym przyciskiem myszy ten folder i Zmień nazwę na CoolTheme.
9. Otwórz plik SkinFile. Skin i Dodaj następujący kod na końcu pliku: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Zapisz plik SkinFile. skórki.
11. Otwórz arkusz stylów. css.
12. Zastąp cały tekst w nim następującym: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Zapisz plik StyleSheet. css.
14. Otwórz stronę Default. aspx.
15. Dodaj kontrolkę TextBox i kontrolkę Button.
16. Zapisz stronę. Teraz Przeglądaj domyślną stronę. aspx. Powinien być wyświetlany jako normalny formularz sieci Web.
17. Otwórz plik Web. config.
18. Dodaj następujący bezpośrednio poniżej tagu otwierającego `<system.web>`: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Zapisz plik Web. config. Teraz Przeglądaj domyślną stronę. aspx. Powinien zostać wyświetlony wraz z zastosowanym motywem.
20. Jeśli nie jest jeszcze otwarty, Otwórz stronę Default. aspx w programie Visual Studio.
21. Wybierz przycisk.
22. Zmień właściwość **SkinID** na goButton. Zauważ, że program Visual Studio udostępnia listę rozwijaną z prawidłowymi wartościami SkinID dla kontrolki Button.
23. Zapisz stronę. Teraz ponownie Wyświetl podgląd strony w przeglądarce. Przycisk powinien teraz powiedzieć "Przejdź" i powinien być szerszy od wyglądu.

Za pomocą właściwości **SkinID** można łatwo skonfigurować różne karnacje dla różnych wystąpień określonego typu formantu serwera.

## <a name="the-stylesheettheme-property"></a>Właściwość StyleSheetTheme

Do tej pory porozmawiamy tylko o stosowaniu motywów przy użyciu właściwości motywu. W przypadku użycia właściwości Theme plik skórki przesłoni wszystkie ustawienia deklaracyjne dla formantów serwera. Na przykład w ćwiczeniu 1 określono SkinID "goButton" dla kontrolki przycisk, a tekst przycisku został zmieniony na "go". Być może zauważono, że właściwość Text przycisku w projektancie została ustawiona na "Button", ale motyw overrode. Motyw zawsze będzie przesłaniał wszystkie ustawienia właściwości w projektancie.

Jeśli chcesz mieć możliwość przesłaniania właściwości zdefiniowanych w pliku skór motywu o właściwościach określonych w projektancie, możesz użyć właściwości **StyleSheetTheme** zamiast właściwości motyw. Właściwość StyleSheetTheme jest taka sama jak Właściwość motywu, z tą różnicą, że nie przesłania wszystkich jawnych ustawień właściwości, takich jak Właściwość motywu.

Aby wyświetlić tę akcję, Otwórz plik Web. config z projektu w ćwiczeniu 1 i Zmień element `<pages>` na następujący:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Teraz Przeglądaj domyślną stronę. aspx i zobaczysz, że Kontrolka przycisku ma ponownie właściwość Text przycisku. Wynika to z faktu, że jawne ustawienie właściwości w projektancie zastępuje właściwość Text ustawioną przez goButton SkinID.

## <a name="overriding-themes"></a>Zastępowanie motywów

Motyw globalny można zastąpić, stosując motyw o tej samej nazwie w folderze motywy\_aplikacji. Jednak motyw nie jest stosowany w scenariuszu prawdziwych zastąpień. Jeśli środowisko uruchomieniowe napotyka pliki motywu w folderze motywy\_aplikacji, zastosuje motyw przy użyciu tych plików i zignoruje motyw globalny.

Właściwość StyleSheetTheme jest możliwa do zastąpienia i może zostać przesłonięta w kodzie w następujący sposób:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Części sieci Web

ASP.NET składniki Web Part to zintegrowany zestaw kontrolek służący do tworzenia witryn sieci Web, które umożliwiają użytkownikom końcowym modyfikowanie zawartości, wyglądu i zachowania stron sieci Web bezpośrednio z przeglądarki. Modyfikacje mogą być stosowane do wszystkich użytkowników w danej lokacji lub do poszczególnych użytkowników. Gdy użytkownicy modyfikują strony i kontrolki, można zapisać ustawienia, aby zachować osobiste preferencje użytkownika w przyszłych sesjach przeglądarki, a funkcja o nazwie Personalizacja. Te składniki Web Part możliwości oznaczają, że deweloperzy mogą umożliwić użytkownikom końcowym dynamiczne Personalizowanie aplikacji sieci Web bez udziału dewelopera lub administratora.

Korzystając z zestawu kontrolek składniki Web Part, programista może umożliwić użytkownikom końcowym:

- Personalizowanie zawartości strony. Użytkownicy mogą dodawać nowe kontrolki składniki Web Part do strony, usuwać je, ukrywać lub minimalizować, jak zwykłe okna.
- Personalizowanie układu strony. Użytkownicy mogą przeciągać formant składniki Web Part do innej strefy na stronie lub zmieniać jego wygląd, właściwości i zachowanie.
- Kontrolki eksportu i importu. Użytkownicy mogą importować lub eksportować składniki Web Part ustawienia kontroli do użycia na innych stronach lub w innych witrynach, zachowując właściwości, wygląd, a nawet dane w kontrolkach. Pozwala to ograniczyć żądania związane z wprowadzaniem danych i konfiguracją użytkowników końcowych.
- Tworzenie połączeń. Użytkownicy mogą nawiązywać połączenia między kontrolkami, tak aby na przykład formant wykresu mógł wyświetlić wykres dla danych w kontrolce giełdowej. Użytkownicy mogą spersonalizować nie tylko połączenie, ale wygląd i szczegóły sposobu wyświetlania danych przez kontrolkę wykres.
- Zarządzanie ustawieniami poziomu lokacji i ich Personalizowanie. Autoryzowani użytkownicy mogą konfigurować ustawienia na poziomie witryny, określać, kto może uzyskać dostęp do witryny lub strony, ustawiać dostęp oparty na rolach do kontrolek i tak dalej. Na przykład użytkownik w roli administracyjnej może ustawić formant składniki Web Part, który ma być współużytkowany przez wszystkich użytkowników, i uniemożliwić użytkownikom, którzy nie są administratorami, Personalizowanie kontroli udostępnionej.

Zwykle pracujesz z składniki Web Part na jeden z trzech sposobów: Tworzenie stron, które używają kontrolek składniki Web Part, tworzenia indywidualnych kontrolek składniki Web Part lub tworzenia kompletnych, Personalizable aplikacji sieci Web, takich jak portal.

## <a name="page-development"></a>Programowanie stron

Deweloperzy stron mogą używać narzędzi do projektowania wizualnego, takich jak Microsoft Visual Studio 2005, do tworzenia stron, które używają składniki Web Part. Jedną z zalet korzystania z narzędzia, takiego jak Visual Studio, jest to, że zestaw kontrolek składniki Web Part udostępnia funkcje do tworzenia i konfigurowania kontrolek składniki Web Part w projektancie wizualnym. Można na przykład użyć projektanta do przeciągnięcia strefy składniki Web Part lub kontrolki edytora składniki Web Part do powierzchni projektowej, a następnie skonfigurować formant bezpośrednio w projektancie przy użyciu interfejsu użytkownika dostarczonego przez zestaw kontrolek składniki Web Part. Umożliwia to przyspieszenie opracowywania aplikacji składniki Web Part i zmniejszenie ilości kodu, który trzeba napisać.

## <a name="control-development"></a>Programowanie kontrolek

Można użyć dowolnej istniejącej kontrolki ASP.NET jako formantu składniki Web Part, w tym standardowych kontrolek serwera sieci Web, niestandardowych kontrolek serwera i kontrolek użytkownika. W celu uzyskania maksymalnej programowalnej kontroli środowiska można także utworzyć niestandardowe kontrolki składniki Web Part pochodne od klasy WebPart. W przypadku poszczególnych rozwiązań w zakresie kontroli składniki Web Part zwykle można utworzyć kontrolkę użytkownika i użyć jej jako formantu składniki Web Part lub opracować niestandardową kontrolkę składniki Web Part.

Przykładem opracowywania niestandardowej kontrolki składniki Web Part można utworzyć kontrolkę, aby zapewnić dowolne funkcje udostępniane przez inne kontrolki serwera ASP.NET, które mogą być przydatne do pakowania jako Personalizable składniki Web Part kontrolki: kalendarze, listy, informacje finansowe, wiadomości, kalkulatory, kontrolki tekstu sformatowanego do aktualizowania zawartości, siatki edytowalne, które łączą się z bazami danych, wykresy, które umożliwiają dynamiczne aktualizowanie ich wyświetlania lub Pogoda i informacje o podróży. Jeśli postanowisz projektanta wizualnego z kontrolką, każdy deweloper strony korzystający z programu Visual Studio może po prostu przeciągnąć formant do strefy składniki Web Part i skonfigurować go w czasie projektowania bez konieczności pisania dodatkowego kodu.

Personalizacja jest podstawą funkcji składniki Web Part. Pozwala ona użytkownikom na modyfikowanie lub Personalizowanie — układ, wygląd i zachowanie formantów składniki Web Part na stronie. Spersonalizowane ustawienia są długotrwałe: są utrwalane nie tylko podczas bieżącej sesji przeglądarki (podobnie jak w przypadku stanu widoku), ale również w przypadku magazynu długoterminowego, dzięki czemu ustawienia użytkownika są również zapisywane w przyszłych sesjach przeglądarki. Personalizacja jest domyślnie włączona dla stron składniki Web Part.

Składniki strukturalne interfejsu użytkownika korzystają z personalizacji i zapewniają podstawową strukturę i usługi, które są obsługiwane przez wszystkie formanty składniki Web Part. Jeden składnik strukturalny interfejsu użytkownika wymagany na każdej składniki Web Part stronie to formant WebPartManager. Mimo że nie jest to nigdy widoczne, ta kontrolka ma krytyczne zadanie koordynujące wszystkie kontrolki składniki Web Part na stronie. Na przykład śledzi wszystkie poszczególne kontrolki składniki Web Part. Zarządza ona strefami składniki Web Part (regiony, które zawierają kontrolki składniki Web Part na stronie), a następnie formanty, w których znajdują się strefy. Ponadto śledzi i kontroluje różne tryby wyświetlania, które mogą znajdować się na stronie, takie jak przeglądanie, łączenie, edytowanie i tryb katalogowania, a także czy zmiany personalizacji mają zastosowanie do wszystkich użytkowników, czy do poszczególnych użytkowników. Na koniec inicjuje i śledzi połączenia i komunikację między kontrolkami składniki Web Part.

Drugim rodzajem składnika strukturalnego interfejsu użytkownika jest strefa. Strefy działają jako menedżerowie układu na stronie składniki Web Part. Zawierają one i organizują kontrolki, które pochodzą z klasy części (formanty części) i umożliwiają modularny układ strony w orientacji poziomej lub pionowej. Strefy oferują również wspólne i spójne elementy interfejsu użytkownika (takie jak styl nagłówka i stopki, tytuł, styl obramowania, przyciski akcji itd.) dla każdej kontrolki, która zawiera; te typowe elementy są znane jako Chrome formantu. Kilka wyspecjalizowanych typów stref są używane w różnych trybach wyświetlania i z różnymi kontrolkami. Różne typy stref są opisane w sekcji składniki Web Part podstawowe kontrolki poniżej.

Kontrolki interfejsu użytkownika składniki Web Part, z których wszystkie pochodzą z klasy **części** , tworzą podstawowy interfejs użytkownika na stronie składniki Web Part. Zestaw kontrolek składniki Web Part jest elastyczny i włączny w opcjach umożliwiających tworzenie formantów częściowych. Oprócz tworzenia własnych niestandardowych kontrolek składniki Web Part można również użyć istniejących kontrolek serwera ASP.NET, kontrolek użytkownika lub niestandardowych formantów serwera jako formantów składniki Web Part. Podstawowe formanty, które są najczęściej używane do tworzenia stron składniki Web Part są opisane w następnej sekcji.

## <a name="web-parts-essential-controls"></a>składniki Web Part podstawowe kontrole

Zestaw kontrolek składniki Web Part jest obszerny, ale niektóre kontrolki są niezbędne, ponieważ są one wymagane do pracy składniki Web Part lub ponieważ są kontrolkami najczęściej używanymi na składniki Web Part stronach. Po rozpoczęciu korzystania z składniki Web Part i tworzenia podstawowych stron składniki Web Part, warto zapoznać się z podstawowymi kontrolkami składniki Web Part opisanymi w poniższej tabeli.

| **Kontrolka składniki Web Part** | **Opis** |
| --- | --- |
| WebPartManager | Zarządza wszystkimi kontrolkami składniki Web Part na stronie. Jeden (i tylko jeden) formant **WebPartManager** jest wymagany dla każdej strony składniki Web Part. |
| CatalogZone | Zawiera kontrolki CatalogPart. Ta strefa służy do tworzenia wykazu formantów składniki Web Part, z których użytkownicy mogą wybierać kontrolki do dodania do strony. |
| Edytora EditorZone | Zawiera kontrolki EditorPart. Ta strefa umożliwia użytkownikom edytowanie i personalizowanie formantów składniki Web Part na stronie. |
| WebPartZone | Zawiera i zapewnia ogólny układ formantów WebPart, które tworzą główny interfejs użytkownika strony. Tej strefy należy używać przy każdej tworzeniu stron z kontrolkami składniki Web Part. Strony mogą zawierać co najmniej jedną strefę. |
| ConnectionsZone | Zawiera kontrolki WebPartConnection i udostępnia interfejs użytkownika do zarządzania połączeniami. |
| Składnik WebPart (GenericWebPart) | Renderuje podstawowy interfejs użytkownika; Większość formantów interfejsu użytkownika składniki Web Part należy do tej kategorii. Aby uzyskać maksymalną kontrolę programistyczną, można utworzyć niestandardowe kontrolki składniki Web Part pochodne od podstawowej kontrolki **WebPart** . Można również użyć istniejących kontrolek serwera, kontrolek użytkownika lub formantów niestandardowych jako składniki Web Part kontrolek. Za każdym razem, gdy którykolwiek z tych kontrolek znajduje się w strefie, formant **WebPartManager** automatycznie zawija je z kontrolkami **GenericWebPart** w czasie wykonywania, aby można było korzystać z nich przy użyciu funkcji składniki Web Part. |
| CatalogPart | Zawiera listę dostępnych składniki Web Part formantów, które użytkownicy mogą dodawać do strony. |
| WebPartConnection | Tworzy połączenie między dwoma składniki Web Part kontrolkami na stronie. Połączenie definiuje jedną z składniki Web Part formantów jako dostawcę (dane), a druga jako odbiorca. |
| EditorPart | Służy jako klasa bazowa dla wyspecjalizowanych kontrolek edytora. |
| Kontrolki EditorPart (AppearanceEditorPart, LayoutEditorPart, BehaviorEditorPart i PropertyGridEditorPart) | Zezwól użytkownikom na personalizowanie różnych aspektów składniki Web Part formantów interfejsu użytkownika na stronie |

## <a name="lab-create-a-web-part-page"></a>Lab: Tworzenie strony składnika Web Part

W tym laboratorium utworzysz stronę części sieci Web, która będzie utrzymywać informacje za pośrednictwem profilów ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Tworzenie prostej strony z składniki Web Part

W tej części przewodnika utworzysz stronę, która używa kontrolek składniki Web Part do wyświetlania zawartości statycznej. Pierwszym krokiem w pracy z składniki Web Partem jest utworzenie strony z dwoma wymaganymi elementami strukturalnymi. Najpierw strona składników Web Part wymaga kontrolki WebPartManager do śledzenia i koordynowania wszystkich formantów składniki Web Part. Druga strona składniki Web Part wymaga co najmniej jednej strefy, które są kontrolkami złożonymi, które zawierają składnik WebPart lub inne kontrolki serwera i zajmują określony region strony.

> [!NOTE]
> Aby włączyć personalizację składniki Web Part, nie trzeba wykonywać żadnych czynności. jest on domyślnie włączony dla zestawu kontrolek składniki Web Part. Po pierwszym uruchomieniu składniki Web Part stronie w witrynie program ASP.NET konfiguruje domyślnego dostawcę personalizacji do przechowywania ustawień personalizacji użytkownika. Aby uzyskać więcej informacji na temat personalizacji, zobacz Omówienie personalizacji składniki Web Part.

### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Aby utworzyć stronę zawierającą kontrolki składniki Web Part

1. Zamknij stronę domyślną i Dodaj nową stronę do witryny o nazwie WebPartsDemo. aspx.
2. Przejdź do widoku **projektu** .
3. W menu **Widok** upewnij się, że opcje formanty i **szczegóły** **inne niż wizualne** są zaznaczone, aby zobaczyć znaczniki układu i kontrolki, które nie mają interfejsu użytkownika.
4. Umieść punkt wstawiania przed tagami `<div>` na powierzchni projektowej i naciśnij klawisz ENTER, aby dodać nowy wiersz. Umieść punkt wstawiania przed znakiem nowego wiersza, kliknij kontrolkę listy rozwijanej **Format bloku** w menu, a następnie wybierz opcję **Nagłówek 1** . W nagłówku Dodaj **stronę demonstracyjną składniki Web Part**tekst.
5. Na karcie **WebParts** przybornika przeciągnij kontrolkę **WebPartManager** na stronę, umieszczając ją tuż po nowym znaku wiersza i przed tagami `<div>`.   
  
   Formant **WebPartManager** nie renderuje żadnych danych wyjściowych, więc pojawia się jako szare pole na powierzchni projektanta.
6. Umieść punkt wstawiania w tagach `<div>`.
7. W menu **Układ** kliknij polecenie **Wstaw tabelę**i Utwórz nową tabelę, która ma jeden wiersz i trzy kolumny. Kliknij przycisk **właściwości komórki** , wybierz pozycję **Góra** z listy rozwijanej **Wyrównaj w pionie** , kliknij przycisk **OK**, a następnie ponownie kliknij przycisk **OK** , aby utworzyć tabelę.
8. Przeciągnij formant WebPartZone do lewej kolumny tabeli. Kliknij prawym przyciskiem myszy kontrolkę **WebPartZone** , wybierz polecenie **Właściwości**i ustaw następujące właściwości:   
  
   Identyfikator: SidebarZone   
  
   HeaderText: pasek boczny
9. Przeciągnij drugi formant **WebPartZone** do kolumny środkowej tabeli i ustaw następujące właściwości:   
  
   Identyfikator: MainZone   
  
   HeaderText: Main
10. Zapisz plik.

Na stronie znajdują się teraz dwie odrębne strefy, które można kontrolować osobno. Jednak żadna strefa nie ma żadnej zawartości, więc tworzenie zawartości jest następnym krokiem. W tym instruktażu pracujesz z kontrolkami składniki Web Part, które wyświetlają tylko zawartość statyczną.

Układ strefy składniki Web Part jest określany za pomocą &lt;szablon ZoneTemplate&gt; elementu. Wewnątrz szablonu strefy można dodać dowolną kontrolkę ASP.NET, niezależnie od tego, czy jest to niestandardowa kontrolka składniki Web Part, kontrolka użytkownika czy istniejący formant serwera. Zwróć uwagę, że w tym miejscu używasz kontrolki etykieta i po prostu dodajesz tekst statyczny. Po umieszczeniu zwykłej kontroli serwera w strefie **WebPartZone** ASP.NET traktuje formant jako kontrolkę składniki Web Part w czasie wykonywania, co umożliwia składniki Web Part funkcji w formancie.

**Aby utworzyć zawartość dla strefy głównej**

1. W widoku **projektu** przeciągnij kontrolkę **etykieta** z karty **standardowa** przybornika do obszaru zawartość strefy, której właściwość **ID** ma wartość MainZone.
2. Przejdź do widoku **źródła** . Zwróć uwagę, że element &lt;szablon ZoneTemplate&gt; został dodany, aby otoczyć formant **etykiety** w MainZone.
3. Dodaj atrybut o nazwie **title** do &lt;ASP: Label&gt; element i ustaw jego wartość na Content. Usuń atrybut text = "label" z elementu &lt;ASP: Label&gt;. Między tagiem otwierającym i zamykającym elementu &lt;ASP: Label&gt;, Dodaj jakiś tekst, taki jak **Witamy w mojej stronie głównej** , w ramach pary &lt;H2&gt; tagów elementu. Kod powinien wyglądać w następujący sposób. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Zapisz plik.

Następnie utwórz kontrolkę użytkownika, którą można także dodać do strony jako kontrolkę składniki Web Part.

### <a name="to-create-a-user-control"></a>Aby utworzyć kontrolkę użytkownika

1. Dodaj nową kontrolkę użytkownika sieci Web do swojej witryny, która będzie działać jako kontrolka wyszukiwania. Usuń zaznaczenie opcji **umieszczenia kodu źródłowego w osobnym pliku**. Dodaj ją w tym samym katalogu, w którym znajduje się Strona WebPartsDemo. aspx, i nadaj jej nazwę SearchUserControl. ascx.   
  
    > [!NOTE]
    > Kontrolka użytkownika dla tego przewodnika nie implementuje rzeczywistej funkcji wyszukiwania. jest on używany tylko do zademonstrowania składniki Web Part funkcji.
2. Przejdź do widoku **projektu** . Na karcie **standardowe** przybornika przeciągnij kontrolkę TextBox na stronę.
3. Umieść punkt wstawiania po właśnie dodanym polu tekstowym, a następnie naciśnij klawisz ENTER, aby dodać nowy wiersz.
4. Przeciągnij kontrolkę Button na stronę w nowym wierszu poniżej dodawanego pola tekstowego.
5. Przejdź do widoku **źródła** . Upewnij się, że kod źródłowy kontrolki użytkownika wygląda jak w poniższym przykładzie. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Zapisz i zamknij plik.

Teraz można dodać kontrolki składniki Web Part do strefy paska bocznego. Do strefy paska bocznego dodawane są dwie kontrolki, z których jedna zawiera listę linków i inną, która jest kontrolką użytkownika utworzoną w poprzedniej procedurze. Linki są dodawane jako kontrolki serwera **etykiet** standardowych, podobnie jak w przypadku utworzonego tekstu statycznego dla strefy głównej. Jednak mimo że poszczególne kontrolki serwera zawarte w kontrolce użytkownika mogą być zawarte bezpośrednio w strefie (podobnie jak kontrolka etykieta), w tym przypadku nie są. Zamiast tego są one częścią kontrolki użytkownika utworzonej w poprzedniej procedurze. Przedstawia to typowy sposób pakowania wszelkich kontrolek i dodatkowych funkcji w kontrolce użytkownika, a następnie odwoływania się do tej kontrolki w strefie jako kontrolka składniki Web Part.

W czasie wykonywania, składniki Web Part zestaw kontrolny otacza obydwa formanty formantem GenericWebPart. Gdy formant **GenericWebPart** otacza formant serwera sieci Web, formant części ogólnej jest formantem nadrzędnym i można uzyskać dostęp do formantu serwera za pomocą właściwości ChildControl kontrolki nadrzędnej. To użycie ogólnych formantów częściowych pozwala formantom serwera sieci Web korzystać z tego samego podstawowego zachowania i atrybutów jako składniki Web Part formantów, które pochodzą od klasy **WebPart** .

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Aby dodać kontrolki składniki Web Part do strefy paska bocznego

1. Otwórz stronę WebPartsDemo. aspx.
2. Przejdź do widoku **projektu** .
3. Przeciągnij utworzoną przez siebie stronę kontrolki użytkownika SearchUserControl. ascx, z **Eksplorator rozwiązań** do strefy, której właściwość **ID** ma wartość SidebarZone, i upuść ją tam.
4. Zapisz stronę WebPartsDemo. aspx.
5. Przejdź do widoku **źródła** .
6. Wewnątrz &lt;ASP: WebPartZone&gt; elementu SidebarZone, tuż nad odwołaniem do kontrolki użytkownika, Dodaj element &lt;ASP: Label&gt; z zawartymi łączami, jak pokazano w poniższym przykładzie. Ponadto należy dodać atrybut **tytułu** do tagu kontrolki użytkownika, korzystając z wartości **Wyszukiwanie**, jak pokazano. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Zapisz i zamknij plik.

Teraz możesz przetestować stronę, przechodząc do jej w przeglądarce. Na stronie zostaną wyświetlone dwie strefy. Na poniższym zrzucie ekranu zostanie wyświetlona strona.

**składniki Web Part stronę demonstracyjną z dwiema strefami**

![Zrzut ekranu składniki Web Part VS — Przewodnik 1](profiles-themes-and-web-parts/_static/image3.gif)

**Rysunek 3**. zrzut ekranu składniki Web Part vs Instruktaż 1

Na pasku tytułu każdej kontrolki znajduje się strzałka w dół, która zapewnia dostęp do menu zleceń dostępnych akcji, które można wykonać na formancie. Kliknij menu zlecenia dla jednej z kontrolek, a następnie kliknij czasownik **minimalizowania** i pamiętaj, że formant jest zminimalizowany. Z menu zlecenia kliknij polecenie **Przywróć**, a kontrolka powróci do normalnego rozmiaru.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Umożliwienie użytkownikom edytowania stron i zmiany układu

Składniki Web Part zapewnia użytkownikom możliwość zmiany układu kontrolek składniki Web Part, przeciągając je z jednej strefy do innej. Oprócz umożliwienia użytkownikom przenoszenia formantów **WebPart** z jednej strefy do innej, można zezwolić użytkownikom na edytowanie różnych cech formantów, w tym ich wyglądu, układu i zachowań. Składniki Web Part zestaw kontrolny zawiera podstawowe funkcje edycji dla formantów **WebPart** . Chociaż nie zostanie to zrobione w tym instruktażu, można również utworzyć niestandardowe formanty edytora umożliwiające użytkownikom edytowanie funkcji formantów **WebPart** . Podobnie jak w przypadku zmiany lokalizacji kontrolki **WebPart** , edytowanie właściwości kontrolki opiera się na ASP.NET personalizacji, aby zapisać zmiany wprowadzane przez użytkowników.

W tej części przewodnika dodano możliwość, aby użytkownicy mogli edytować podstawowe cechy każdej kontrolki **WebPart** na stronie. Aby włączyć te funkcje, należy dodać kolejną niestandardową kontrolkę użytkownika do strony wraz z &lt;ASP: edytora EditorZone&gt; i dwoma kontrolkami edycji.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Aby utworzyć kontrolkę użytkownika, która umożliwia zmianę układu strony

1. W programie Visual Studio w menu **plik** wybierz polecenie **Nowy** podmenu, a następnie kliknij opcję **plik** .
2. W oknie dialogowym **Dodaj nowy element** wybierz pozycję **kontrolka użytkownika sieci Web**. Nazwij nowy plik DisplayModeMenu. ascx. Usuń zaznaczenie opcji **umieszczenia kodu źródłowego w osobnym pliku**.
3. Kliknij przycisk Dodaj, aby utworzyć nową kontrolkę.
4. Przejdź do widoku **źródła** .
5. Usuń cały istniejący kod w nowym pliku i wklej go w poniższym kodzie. Ten kod kontrolny użytkownika korzysta z funkcji zestawu kontrolek składniki Web Part, który umożliwia stronie zmianę jego widoku lub trybu wyświetlania, a także pozwala zmieniać wygląd i układ strony w pewnych trybach wyświetlania. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Zapisz plik, klikając ikonę Zapisz na pasku narzędzi lub wybierając pozycję **Zapisz** w menu **plik** .

### <a name="to-enable-users-to-change-the-layout"></a>Aby umożliwić użytkownikom zmianę układu

1. Otwórz stronę WebPartsDemo. aspx i przejdź do widoku **projektu** .
2. Umieść punkt wstawiania w widoku **projektu** tuż po dodanym wcześniej formancie **WebPartManager** . Dodaj twarde Return po tekście, tak aby po kontrolce **WebPartManager** był pusty wiersz. Umieść punkt wstawiania w pustym wierszu.
3. Przeciągnij właśnie utworzoną kontrolkę użytkownika (plik o nazwie DisplayModeMenu. ascx) na stronę WebPartsDemo. aspx i upuść ją w pustym wierszu.
4. Przeciągnij formant edytora EditorZone z sekcji **WebParts** przybornika do pozostałej otwartej komórki tabeli na stronie WebPartsDemo. aspx.
5. W sekcji **WebParts** przybornika przeciągnij kontrolkę AppearanceEditorPart i kontrolkę LayoutEditorPart do kontrolki **edytora EditorZone** .
6. Przejdź do widoku **źródła** . Kod wynikający z komórki tabeli powinien wyglądać podobnie do poniższego kodu. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Zapisz plik WebPartsDemo. aspx. Utworzono kontrolkę użytkownika, która pozwala na zmianę trybów wyświetlania i zmianę układu strony, oraz przywoływany formant na podstawowej stronie sieci Web.

Teraz można testować możliwość edytowania stron i zmiany układu.

### <a name="to-test-layout-changes"></a>Aby przetestować zmiany układu

1. Załaduj stronę w przeglądarce.
2. Kliknij menu rozwijane **tryb wyświetlania** , a następnie wybierz pozycję **Edytuj**. Wyświetlane są tytuły stref.
3. Przeciągnij kontrolkę **moje linki** na pasek tytułu ze strefy paska bocznego na dolną część strefy głównej. Strona powinna wyglądać podobnie do poniższego zrzutu ekranu.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Strona demonstracyjna składniki Web Part z przeniesionym formantem moje łącza

![Zrzut ekranu składniki Web Part VS — Instruktaż 2](profiles-themes-and-web-parts/_static/image4.gif)

**Rysunek 4**. zrzut ekranu składniki Web Part vs Instruktaż 2

1. Kliknij menu rozwijane **tryb wyświetlania** , a następnie wybierz pozycję **Przeglądaj**. Strona zostanie odświeżona, nazwy stref znikają, a kontrolka **moje linki** pozostaje w miejscu, w którym została umieszczona.
2. Aby zademonstrować, że Personalizacja działa, zamknij przeglądarkę, a następnie ponownie Załaduj stronę. Wprowadzone zmiany są zapisywane w przyszłych sesjach przeglądarki.
3. W menu **tryb wyświetlania** wybierz pozycję **Edytuj**.   
  
   Każda kontrolka na stronie jest teraz wyświetlana ze strzałką w dół na pasku tytułu, która zawiera menu rozwijane zlecenia.
4. Kliknij strzałkę, aby wyświetlić menu czasowniki w kontrolce **moje linki** . Kliknij pozycję **Edytuj** zlecenie.   
  
   Zostanie wyświetlona kontrolka **edytora EditorZone** wyświetlająca dodane kontrolki EditorPart.
5. W sekcji **wygląd** kontrolki Edycja Zmień **tytuł** na moje ulubione, Użyj listy rozwijanej **Typ Chrome** , aby zaznaczyć pozycję **tylko tytuł**, a następnie kliknij przycisk **Zastosuj**. Poniższy zrzut ekranu przedstawia stronę w trybie edycji.

### <a name="web-parts-demo-page-in-edit-mode"></a>Strona demonstracyjna składniki Web Part w trybie edycji

![Zrzut ekranu składniki Web Part VS Instruktaż 3](profiles-themes-and-web-parts/_static/image5.gif)

**Rysunek 5**. zrzut ekranu składniki Web Part vs Instruktaż 3

1. Kliknij menu **tryb wyświetlania** , a następnie wybierz polecenie **Przeglądaj** , aby powrócić do trybu przeglądania.
2. Kontrolka ma teraz zaktualizowany tytuł i brak obramowania, jak pokazano na poniższym zrzucie ekranu.

### <a name="edited-web-parts-demo-page"></a>Edytowanie składniki Web Part stronie demonstracyjnej

![Zrzut ekranu składniki Web Part VS Instruktaż 4](profiles-themes-and-web-parts/_static/image6.gif)

**Rysunek 4**. zrzut ekranu składniki Web Part vs Instruktaż 4

### <a name="adding-web-parts-at-run-time"></a>Dodawanie składniki Web Part w czasie wykonywania

Możesz również pozwolić użytkownikom na dodawanie składniki Web Part formantów do ich strony w czasie wykonywania. W tym celu należy skonfigurować stronę z wykazem składniki Web Part, który zawiera listę formantów składniki Web Part, które mają być dostępne dla użytkowników.

**Aby umożliwić użytkownikom dodawanie składniki Web Part w czasie wykonywania**

1. Otwórz stronę WebPartsDemo. aspx i przejdź do widoku **projektu** .
2. Na karcie **WebParts** przybornika przeciągnij formant CatalogZone do prawej kolumny tabeli poniżej kontrolki **edytora EditorZone** .   
  
   Obie kontrolki mogą znajdować się w tej samej komórce tabeli, ponieważ nie będą wyświetlane w tym samym czasie.
3. W okienku właściwości Przypisz ciąg **dodaj składniki Web Part** do właściwości HeaderText kontrolki **CatalogZone** .
4. W sekcji **WebParts** przybornika przeciągnij kontrolkę DeclarativeCatalogPart do obszaru zawartość kontrolki **CatalogZone** .
5. Kliknij strzałkę w prawym górnym rogu kontrolki **DeclarativeCatalogPart** , aby uwidocznić jej menu zadania, a następnie wybierz pozycję **Edytuj szablony**.
6. W sekcji **standardowa** przybornika przeciągnij kontrolkę **FileUpload** i kontrolkę **Calendar** do sekcji **WebPartsTemplate** kontrolki **DeclarativeCatalogPart** .
7. Przejdź do widoku **źródła** . Sprawdź kod źródłowy elementu &lt;ASP: CatalogZone&gt;. Należy zauważyć, że formant **DeclarativeCatalogPart** zawiera element&gt; &lt;WebPartsTemplate z dwoma załączonymi kontrolkami serwera, które będą mogły zostać dodane do strony z wykazu.
8. Dodaj właściwość **title** do każdego z formantów, które zostały dodane do wykazu, przy użyciu wartości ciągu podanej dla każdego tytułu w poniższym przykładzie kodu. Mimo że tytuł nie jest właściwością, można normalnie ustawić na tych dwóch kontrolkach serwera w czasie projektowania, gdy użytkownik doda te kontrolki do strefy **WebPartZone** z wykazu w czasie wykonywania, są one opakowane przy użyciu kontrolki **GenericWebPart** . Dzięki temu mogą one działać jako kontrolki składniki Web Part, dzięki czemu będą mogli wyświetlać tytuły.   
  
   Kod dla dwóch kontrolek zawartych w kontrolce **DeclarativeCatalogPart** powinien wyglądać w następujący sposób. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Zapisz stronę.

Teraz można testować wykaz.

### <a name="to-test-the-web-parts-catalog"></a>Aby przetestować katalog składniki Web Part

1. Załaduj stronę w przeglądarce.
2. Kliknij menu rozwijane **tryb wyświetlania** , a następnie wybierz pozycję **katalog**.   
  
   Zostanie wyświetlony wykaz zatytułowany **dodaj składniki Web Part** .
3. Przeciągnij kontrolkę **Moje ulubione** z strefy głównej z powrotem na górną część strefy paska bocznego i upuść ją tam.
4. W oknie **dodawanie składniki Web Part** wykazu zaznacz oba pola wyboru, a następnie wybierz pozycję **główny** z listy rozwijanej zawierającej dostępne strefy.
5. Kliknij przycisk **Dodaj** w wykazie. Formanty są dodawane do strefy głównej. Jeśli chcesz, możesz dodać wiele wystąpień kontrolek z wykazu do strony.   
  
   Poniższy zrzut ekranu przedstawia stronę z formantem przekazywania plików i kalendarzem w strefie głównej. 

![Kontrolki dodane do strefy głównej z wykazu](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Kliknij menu rozwijane **tryb wyświetlania** , a następnie wybierz pozycję **Przeglądaj**. Wykaz znika i strona zostanie odświeżona.
7. Zamknij okno przeglądarki. Załaduj ponownie stronę. Wprowadzone zmiany są utrwalane.

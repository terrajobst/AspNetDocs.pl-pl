---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Buforowanie | Microsoft Docs
author: microsoft
description: Zrozumienie buforowania jest ważne dla dobrze wykonywanej aplikacji ASP.NET. ASP.NET 1. x oferuje trzy różne opcje buforowania; buforowanie danych wyjściowych,...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 4f0b021ca6ca151544dd9fb0587ed9e0cf14ff65
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575937"
---
# <a name="caching"></a>Buforowanie

przez [firmę Microsoft](https://github.com/microsoft)

> Zrozumienie buforowania jest ważne dla dobrze wykonywanej aplikacji ASP.NET. ASP.NET 1. x oferuje trzy różne opcje buforowania; buforowanie danych wyjściowych, buforowanie fragmentów i interfejs API pamięci podręcznej.

Zrozumienie buforowania jest ważne dla dobrze wykonywanej aplikacji ASP.NET. ASP.NET 1. x oferuje trzy różne opcje buforowania; buforowanie danych wyjściowych, buforowanie fragmentów i interfejs API pamięci podręcznej. ASP.NET 2,0 oferuje wszystkie trzy te metody, ale dodaje kilka znaczących dodatkowych funkcji. Istnieje kilka nowych zależności pamięci podręcznej, a deweloperzy mają teraz możliwość tworzenia niestandardowych zależności pamięci podręcznej. Konfiguracja buforowania została również znacznie ulepszona w ASP.NET 2,0.

## <a name="new-features"></a>Nowe funkcje

## <a name="cache-profiles"></a>Profile pamięci podręcznej

Profile pamięci podręcznej umożliwiają deweloperom Definiowanie określonych ustawień pamięci podręcznej, które można następnie zastosować do poszczególnych stron. Na przykład jeśli masz pewne strony, które powinny wygasnąć z pamięci podręcznej po upływie 12 godzin, możesz łatwo utworzyć profil pamięci podręcznej, który będzie można zastosować do tych stron. Aby dodać nowy profil pamięci podręcznej, należy użyć sekcji &lt;outputCacheSettings&gt; w pliku konfiguracji. Na przykład poniżej przedstawiono konfigurację profilu pamięci podręcznej o nazwie *twoday* , który konfiguruje czas trwania pamięci podręcznej wynoszący 12 godzin.

[!code-xml[Main](caching/samples/sample1.xml)]

Aby zastosować ten profil pamięci podręcznej do określonej strony, należy użyć atrybutu CacheProfile dyrektywy @ OutputCache, jak pokazano poniżej:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Niestandardowe zależności pamięci podręcznej

ASP.NET 1. x deweloperzy Cried z niestandardowymi zależnościami pamięci podręcznej. W ASP.NET 1. x Klasa CacheDependency została zapieczętowana, która uniemożliwiła deweloperom wyprowadzanie ich własnych klas. W ASP.NET 2,0 ograniczenie zostało usunięte, a deweloperzy mogą opracowywać własne niestandardowe zależności pamięci podręcznej. Klasa CacheDependency umożliwia tworzenie niestandardowej zależności pamięci podręcznej na podstawie plików, katalogów lub kluczy pamięci podręcznej.

Na przykład poniższy kod tworzy nową niestandardową zależność pamięci podręcznej na podstawie pliku o nazwie repliks. XML znajdującego się w katalogu głównym aplikacji sieci Web:

[!code-csharp[Main](caching/samples/sample3.cs)]

W tym scenariuszu, gdy plik. XML zmieni się, buforowany element jest unieważniony.

Można również utworzyć niestandardową zależność pamięci podręcznej przy użyciu kluczy pamięci podręcznej. Korzystając z tej metody, usunięcie klucza pamięci podręcznej spowoduje unieważnienie danych z pamięci. Zostało to przedstawione w poniższym przykładzie:

[!code-csharp[Main](caching/samples/sample4.cs)]

Aby unieważnić element, który został wstawiony powyżej, po prostu Usuń element, który został wstawiony do pamięci podręcznej, aby działać jako klucz pamięci podręcznej.

[!code-csharp[Main](caching/samples/sample5.cs)]

Należy zauważyć, że klucz elementu, który działa jako klucz pamięci podręcznej, musi być taki sam jak wartość dodana do tablicy kluczy pamięci podręcznej.

## <a name="polling-based-sql-cache-dependenciesalso-called-table-based-dependencies"></a>Sondy pamięci podręcznej SQL oparte na sondowaniu (nazywane również zależnościami opartymi na tabelach)

SQL Server 7 i 2000 używają modelu sondowania dla zależności pamięci podręcznej SQL. Model oparty na sondowaniu używa wyzwalacza w tabeli bazy danych, która jest wyzwalana, gdy dane w tabeli zmieniają się. Ten wyzwalacz aktualizuje pole **changeId** w tabeli powiadomień, które okresowo sprawdza ASP.NET. Jeśli pole **changeId** zostało zaktualizowane, ASP.NET wie, że dane uległy zmianie i unieważnią dane w pamięci podręcznej.

> [!NOTE]
> SQL Server 2005 może również używać modelu sondowania, ale ponieważ model oparty na sondowaniu nie jest najbardziej wydajnym modelem, zaleca się użycie modelu opartego na zapytaniach (omówionego później) z SQL Server 2005.

Aby zależność pamięci podręcznej SQL wykorzystująca model sondowania działała poprawnie, tabele muszą mieć włączone powiadomienia. Można to zrobić programowo przy użyciu klasy SqlCacheDependencyAdmin lub za pomocą narzędzia ASPNET\_regsql. exe.

Poniższy wiersz polecenia rejestruje tabelę Products w bazie danych Northwind znajdującą się w wystąpieniu SQL Server o nazwie *dBASE* dla zależności pamięci podręcznej SQL.

[!code-console[Main](caching/samples/sample6.cmd)]

Poniżej znajduje się wyjaśnienie przełączników wiersza polecenia użytych w powyższym poleceniu:

| **Przełącznik wiersza polecenia** | **Przeznaczenie** |
| --- | --- |
| -S *serwer* | Określa nazwę serwera. |
| -Ed | Określa, że baza danych powinna być włączona dla zależności pamięci podręcznej SQL. |
| -d *nazwa\_bazy danych* | Określa nazwę bazy danych, która ma być włączona dla zależności pamięci podręcznej SQL. |
| -E | Określa, że w przypadku nawiązywania połączenia z bazą danych ASPNET\_regsql powinny używać uwierzytelniania systemu Windows. |
| -et | Określa, że jest włączana tabela bazy danych dla zależności pamięci podręcznej SQL. |
| -t *tabela\_nazwa* | Określa nazwę tabeli bazy danych, która ma zostać włączona dla zależności pamięci podręcznej SQL. |

> [!NOTE]
> Dostępne są inne przełączniki dla ASPNET\_regsql. exe. Aby uzyskać pełną listę, uruchom polecenie ASPNET\_regsql. exe-? z wiersza polecenia.

Po uruchomieniu tego polecenia w bazie danych SQL Server są wprowadzane następujące zmiany:

- Dodano tabelę **AspNet\_SqlCacheTablesForChangeNotification** . Ta tabela zawiera jeden wiersz dla każdej tabeli w bazie danych, dla której włączono zależność pamięci podręcznej SQL.
- Następujące procedury składowane są tworzone wewnątrz bazy danych programu:

| AspNet\_SqlCachePollingStoredProcedure | Wysyła zapytanie do tabeli SqlCacheTablesForChangeNotification języka\_AspNet i zwraca wszystkie tabele, dla których włączono zależność pamięci podręcznej SQL i wartość changeId dla każdej tabeli. Ten przechowywany proces służy do sondowania w celu ustalenia, czy dane uległy zmianie. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Zwraca wszystkie tabele, dla których włączono zależność pamięci podręcznej SQL, wykonując zapytania dotyczące tabeli AspNet\_SqlCacheTablesForChangeNotification i zwraca wszystkie tabele z włączoną zależnością pamięci podręcznej SQL. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Rejestruje tabelę dla zależności pamięci podręcznej SQL przez dodanie niezbędnej pozycji w tabeli powiadomień i dodanie wyzwalacza. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Wyrejestrowuje tabelę dla zależności pamięci podręcznej SQL przez usunięcie wpisu w tabeli powiadomień i usunięcie wyzwalacza. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Aktualizuje tabelę powiadomień przez zwiększenie changeId dla zmienionej tabeli. ASP.NET używa tej wartości, aby określić, czy dane zostały zmienione. Jak wskazano poniżej, ten przechowywany proces jest wykonywany przez wyzwalacz utworzony, gdy tabela jest włączona. |

- Wyzwalacz SQL Server o nazwie  **_Table\_Name_\_AspNet\_SqlCacheNotification\_Trigger** został utworzony dla tabeli. Ten wyzwalacz wykonuje SqlCacheUpdateChangeIdStoredProcedure\_AspNet, gdy w tabeli zostanie wykonane Wstawianie, aktualizowanie lub usuwanie.
- Do bazy danych zostanie dodana rola SQL Server wywołana jako **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** .

**ChangeNotification aspnet\_ReceiveNotificationsOnlyAccess\_** SQL Server ma uprawnienia programu EXEC do\_SqlCachePollingStoredProcedure. Aby model sondowania działał prawidłowo, należy dodać konto procesu do elementu ASPNET\_ChangeNotification\_role ReceiveNotificationsOnlyAccess. Narzędzie ASPNET\_regsql. exe nie będzie tego robić.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Konfigurowanie zależności w pamięci podręcznej SQL opartej na sondowaniu

Należy wykonać kilka czynności, które są wymagane do skonfigurowania zależności w pamięci podręcznej SQL opartej na sondach. Pierwszym krokiem jest włączenie bazy danych i tabeli zgodnie z powyższym opisem. Po wykonaniu tego kroku pozostała konfiguracja jest następująca:

- Konfigurowanie pliku konfiguracji ASP.NET.
- Konfigurowanie SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Konfigurowanie pliku konfiguracji ASP.NET

Oprócz dodawania parametrów połączenia zgodnie z opisem w poprzednim module, należy również skonfigurować &lt;pamięci podręcznej&gt; elementu za pomocą &lt;elementu&gt; sqlCacheDependency, jak pokazano poniżej:

[!code-xml[Main](caching/samples/sample7.xml)]

Ta konfiguracja umożliwia zależność pamięci podręcznej SQL w bazie danych *pubs* . Należy zauważyć, że atrybut pollTime w elemencie &lt;sqlCacheDependency&gt; wartością domyślną 60000 milisekund lub 1 minutę. (Ta wartość nie może być mniejsza niż 500 milisekund). W tym przykładzie &lt;dodanie elementu&gt; powoduje dodanie nowej bazy danych i zastąpienie pollTime, ustawienie na 9000000 milisekund.

#### <a name="configuring-the-sqlcachedependency"></a>Konfigurowanie SqlCacheDependency

Następnym krokiem jest skonfigurowanie SqlCacheDependency. Najprostszym sposobem osiągnięcia tego jest określenie wartości atrybutu SqlDependency w dyrektywie @ subcache w następujący sposób:

[!code-aspx[Main](caching/samples/sample8.aspx)]

W powyższej dyrektywie @ OutputCache zależność pamięci podręcznej SQL jest konfigurowana dla tabeli *autorów* w bazie danych *pubs* . Wiele zależności można skonfigurować, rozdzielając je średnikami, takich jak:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Inną metodą konfigurowania SqlCacheDependency jest to, że należy to zrobić programowo. Poniższy kod tworzy nową zależność pamięci podręcznej SQL w tabeli *autorów* w bazie danych *pubs* .

[!code-csharp[Main](caching/samples/sample10.cs)]

Jedną z zalet programistycznego definiowania zależności pamięci podręcznej SQL jest to, że można obsługiwać wszystkie wyjątki, które mogą wystąpić. Na przykład jeśli spróbujesz zdefiniować zależność pamięci podręcznej SQL dla bazy danych, która nie została włączona dla powiadomienia, zostanie zgłoszony wyjątek **DatabaseNotEnabledForNotificationException —** . W takim przypadku można spróbować włączyć dla tej bazy danych powiadomienia przez wywołanie metody **SqlCacheDependencyAdmin. EnableNotifications** i przekazanie jej do nazwy bazy danych.

Podobnie, jeśli spróbujesz zdefiniować zależność pamięci podręcznej SQL dla tabeli, która nie została włączona dla powiadomienia, zostanie zgłoszony **TableNotEnabledForNotificationException —** . Następnie można wywołać metodę **SqlCacheDependencyAdmin. EnableTableForNotifications** , przekazując ją nazwę bazy danych i nazwę tabeli.

Poniższy przykład kodu ilustruje sposób poprawnego skonfigurowania obsługi wyjątków podczas konfigurowania zależności pamięci podręcznej SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Więcej informacji: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Zależności pamięci podręcznej SQL oparte na zapytaniach (tylko SQL Server 2005)

W przypadku korzystania z SQL Server 2005 dla zależności pamięci podręcznej SQL model oparty na sondowaniu nie jest konieczny. W przypadku używania z SQL Server 2005 zależności pamięci podręcznej SQL komunikują się bezpośrednio za pośrednictwem połączeń SQL z wystąpieniem SQL Server (nie jest wymagana żadna dodatkowa konfiguracja) przy użyciu powiadomień zapytań SQL Server 2005.

Najprostszym sposobem włączania powiadomień opartych na zapytaniach jest to, że ustawienie atrybutu **SqlCacheDependency** obiektu źródła danych na **CommandNotification** i ustawienie atrybutu **EnableCaching** na **true**. Korzystając z tej metody, żaden kod nie jest wymagany. Jeśli wynik polecenia wykonanego względem źródła danych ulegnie zmianie, spowoduje to unieważnienie danych pamięci podręcznej.

Poniższy przykład konfiguruje kontrolę źródła danych dla zależności pamięci podręcznej SQL:

[!code-aspx[Main](caching/samples/sample12.aspx)]

W takim przypadku, jeśli zapytanie określone w elemencie **SelectCommand** zwróci inny wynik niż pierwotnie, wyniki w pamięci podręcznej są unieważnione.

Możesz również określić, że wszystkie źródła danych mają być włączone dla zależności w pamięci podręcznej SQL, ustawiając atrybut **SqlDependency** dyrektywy **@ OutputCache** na **CommandNotification**. Ilustruje to Poniższy przykład.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Aby uzyskać więcej informacji na temat powiadomień o zapytaniach w SQL Server 2005, zobacz dokumentację SQL Server Books Online.

Inną metodą konfigurowania zależności pamięci podręcznej SQL opartej na kwerendach jest to, że należy to zrobić programowo przy użyciu klasy SqlCacheDependency. Poniższy przykład kodu ilustruje, jak to zrobić.

[!code-csharp[Main](caching/samples/sample14.cs)]

Więcej informacji: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Post-Cache Substitution

Buforowanie strony może znacząco zwiększyć wydajność aplikacji sieci Web. Jednak w niektórych przypadkach większość stron ma być buforowana, a niektóre fragmenty na stronie mają być dynamiczne. Na przykład jeśli utworzysz stronę ze historiemi wiadomości, które są całkowicie statyczne dla ustawionych okresów czasu, możesz ustawić całą stronę jako buforowaną. Jeśli zechcesz dołączyć transparent usługi AD, który został zmieniony na każde żądanie strony, część strony zawierającej anons musi być dynamiczna. Aby umożliwić buforowanie strony, ale w sposób dynamiczny należy zastąpić część zawartości, można użyć podstawiania po pamięci podręcznej ASP.NET. W przypadku podstawiania po pamięci podręcznej cała strona jest buforowana z określonymi częściami oznaczonymi jako wykluczone z buforowania. W przykładzie transparentów usługi AD formant AdRotator umożliwia korzystanie z podstawienia podręcznego po podwyższeniu poziomu, dzięki czemu reklamy są tworzone dynamicznie dla każdego użytkownika i dla każdego odświeżania strony.

Istnieją trzy sposoby implementacji podstawiania po pamięci podręcznej:

- Deklaratywnie, przy użyciu formantu podstawiania.
- Programowo przy użyciu interfejsu API kontroli podstawiania.
- Niejawnie, przy użyciu formantu AdRotator.

### <a name="substitution-control"></a>Formant podstawiania

Kontrolka podstawienia ASP.NET określa sekcję buforowanej strony, która jest tworzona dynamicznie, a nie w pamięci podręcznej. Na stronie, w której ma się pojawić zawartość dynamiczna, należy umieścić formant podstawienia. W czasie wykonywania, formant podstawienia wywołuje metodę określoną za pomocą właściwości MethodName. Metoda musi zwracać ciąg, który następnie zastępuje zawartość formantu podstawiania. Metoda musi być metodą statyczną na stronie zawierającej formant UserControl. Użycie kontroli podstawiania powoduje zmianę pamięci podręcznej po stronie klienta na pamięć podręczną serwera, dzięki czemu Strona nie będzie buforowana na kliencie. Pozwala to zagwarantować, że przyszłe żądania kierowane do strony wywołują metodę w celu wygenerowania zawartości dynamicznej.

### <a name="substitution-api"></a>Interfejs API podstawiania

Aby programowo utworzyć zawartość dynamiczną dla buforowanej strony, można wywołać metodę [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) w kodzie strony, przekazując ją nazwą metody jako parametr. Metoda, która obsługuje tworzenie zawartości dynamicznej, przyjmuje jeden parametr [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) i zwraca ciąg. Zwracanym ciągiem jest zawartość, która zostanie zastąpiona w danej lokalizacji. Zaletą wywołania metody WriteSubstitution zamiast używania kontroli podstawiania jest deklaratywne, że można wywołać metodę dowolnego dowolnego obiektu zamiast wywoływania statycznej metody strony lub obiektu UserControl.

Wywołanie metody WriteSubstitution powoduje zmianę pamięci podręcznej po stronie klienta na pamięć podręczną serwera, dzięki czemu Strona nie będzie buforowana na kliencie. Pozwala to zagwarantować, że przyszłe żądania kierowane do strony wywołują metodę w celu wygenerowania zawartości dynamicznej.

### <a name="adrotator-control"></a>AdRotator — formant

Formant serwera AdRotator implementuje obsługę podstawiania po pamięci podręcznej. Jeśli umieścisz formant AdRotator na stronie, będzie on wyświetlał unikatowe Anonsy dla każdego żądania, niezależnie od tego, czy strona nadrzędna jest buforowana. W efekcie Strona zawierająca formant AdRotator jest tylko w pamięci podręcznej po stronie serwera.

## <a name="controlcachepolicy-class"></a>Klasa ControlCachePolicy

Klasa ControlCachePolicy umożliwia programistyczne sterowanie buforowaniem fragmentów przy użyciu kontrolek użytkownika. ASP.NET osadza formanty użytkownika w wystąpieniu [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) . Klasa BasePartialCachingControl reprezentuje kontrolkę użytkownika, która ma włączone buforowanie danych wyjściowych.

Gdy uzyskujesz dostęp do właściwości [BasePartialCachingControl. CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) formantu [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) , zawsze otrzymujesz prawidłowy obiekt ControlCachePolicy. Jednak w przypadku uzyskania dostępu do właściwości [UserControl. CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) kontrolki [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) otrzymujesz prawidłowy obiekt ControlCachePolicy tylko wtedy, gdy kontrolka użytkownika została już opakowana przez formant BasePartialCachingControl. Jeśli nie jest opakowany, obiekt ControlCachePolicy zwrócony przez właściwość będzie generować wyjątki podczas próby manipulowania nim, ponieważ nie ma skojarzonego BasePartialCachingControl. Aby określić, czy wystąpienie elementu UserControl obsługuje buforowanie bez generowania wyjątków, należy sprawdzić Właściwość [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) .

Korzystanie z klasy ControlCachePolicy jest jednym z kilku sposobów włączania buforowania danych wyjściowych. Na poniższej liście opisano metody, których można użyć w celu włączenia buforowania danych wyjściowych:

- Aby włączyć buforowanie danych wyjściowych w scenariuszach deklaratywnych, należy użyć dyrektywy [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) .
- Użyj atrybutu [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) , aby włączyć buforowanie dla kontrolki użytkownika w pliku związanym z kodem.
- Użyj klasy ControlCachePolicy, aby określić ustawienia pamięci podręcznej w scenariuszach programistycznych, w których pracujesz z wystąpieniami BasePartialCachingControl z włączoną obsługą pamięci podręcznej przy użyciu jednej z poprzednich metod i dynamicznie ładowany przy użyciu metody [System. Web. UI. TemplateControl. LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) .

Wystąpienie ControlCachePolicy można pomyślnie manipulować tylko między etapami init i PreRender cyklu życia kontroli. Jeśli zmodyfikujesz obiekt ControlCachePolicy po fazie PreRender, ASP.NET zgłosi wyjątek, ponieważ wszelkie zmiany wprowadzone po kontrolce są renderowane nie mogą faktycznie wpływać na ustawienia pamięci podręcznej (formant jest buforowany na etapie renderowania). Na koniec wystąpienie kontrolki użytkownika (i w związku z tym obiekt ControlCachePolicy) jest dostępne tylko na potrzeby manipulowania programistycznego, gdy jest faktycznie renderowany.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Zmiany w konfiguracji buforowania — &lt;buforowania&gt; elementu

Istnieje kilka zmian konfiguracji buforowania w programie ASP.NET 2,0. &lt;buforowania&gt; element jest nowy w ASP.NET 2,0 i umożliwia wprowadzanie zmian konfiguracji pamięci podręcznej w pliku konfiguracji. Dostępne są następujące atrybuty.

| **Element** | **Opis** |
| --- | --- |
| **Chow** | Element opcjonalny. Definiuje globalne ustawienia pamięci podręcznej aplikacji. |
| **outputCache** | Element opcjonalny. Określa ustawienia wyjściowe pamięci podręcznej całej aplikacji. |
| **outputCacheSettings** | Element opcjonalny. Określa ustawienia pamięci podręcznej danych wyjściowych, które można zastosować do stron w aplikacji. |
| **sqlCacheDependency** | Element opcjonalny. Konfiguruje zależności pamięci podręcznej SQL dla aplikacji ASP.NET. |

### <a name="the-ltcachegt-element"></a>&gt; element pamięci podręcznej &lt;

Następujące atrybuty są dostępne w &lt;&gt; pamięci podręcznej:

| **Atrybut** | **Opis** |
| --- | --- |
| **disableMemoryCollection** | Opcjonalny atrybut **Boolean** . Pobiera lub ustawia wartość wskazującą, czy kolekcja pamięci podręcznej występuje, gdy maszyna jest w stanie wyłączać pamięć. |
| **disableExpiration** | Opcjonalny atrybut **Boolean** . Pobiera lub ustawia wartość wskazującą, czy wygaśnięcie pamięci podręcznej jest wyłączone. Po wyłączeniu elementy w pamięci podręcznej nie wygasają, a oczyszczanie w tle wygasłych elementów pamięci podręcznej nie występuje. |
| **privateBytesLimit** | Opcjonalny atrybut **Int64** . Pobiera lub ustawia wartość wskazującą maksymalny rozmiar prywatnych bajtów aplikacji przed rozpoczęciem opróżniania przez pamięć podręczną elementów i próby odzyskania pamięci. Ten limit obejmuje zarówno pamięć używaną przez pamięć podręczną, jak i normalne obciążenie pamięci z działającej aplikacji. Ustawienie wartości zero wskazuje, że ASP.NET będzie używać własnych algorytmów heurystycznych do określania, kiedy należy rozpocząć odzyskiwanie pamięci. |
| **percentagePhysicalMemoryUsedLimit** | Opcjonalny atrybut **Int32** . Pobiera lub ustawia wartość wskazującą maksymalną wartość procentową pamięci fizycznej maszyny, która może być używana przez aplikację, zanim pamięć podręczna zacznie opróżniać wygasłe elementy, a następnie próbuje odzyskać pamięć, w której jest również używana pamięć podręczna. jak normalne użycie pamięci przez działającą aplikację. Ustawienie wartości zero wskazuje, że ASP.NET będzie używać własnych algorytmów heurystycznych do określania, kiedy należy rozpocząć odzyskiwanie pamięci. |
| **privateBytesPollTime** | Opcjonalny atrybut **TimeSpan** . Pobiera lub ustawia wartość wskazującą przedział czasu między sondowaniem użycia pamięci przez prywatne bajty w aplikacji. |

### <a name="the-ltoutputcachegt-element"></a>Element &lt;outputCache&gt;

Następujące atrybuty są dostępne dla elementu &lt;outputCache&gt;.

|       <strong>Atrybut</strong>        |                                                                                                                                                                                                                                                       <strong>Opis</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Opcjonalny atrybut <strong>Boolean</strong> . Włącza/wyłącza wyjściową pamięć podręczną strony. Jeśli ta wartość jest wyłączona, żadne strony nie są buforowane niezależnie od ustawień programistycznych lub deklaratywnych. Wartość domyślna to <strong>true</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Opcjonalny atrybut <strong>Boolean</strong> . Włącza/wyłącza pamięć podręczną fragmentu aplikacji. Jeśli ta wartość jest wyłączona, żadne strony nie są buforowane niezależnie od używanej dyrektywy [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) lub profilu buforowania. Zawiera nagłówek kontroli pamięci podręcznej wskazujący, że nadrzędne serwery proxy oraz klienci przeglądarki nie powinny podejmować prób buforowania danych wyjściowych strony. Wartość domyślna to <strong>false</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Opcjonalny atrybut <strong>Boolean</strong> . Pobiera lub ustawia wartość wskazującą, czy blok <strong>pamięci podręcznej: nagłówek prywatny</strong> jest domyślnie wysyłany przez moduł wyjściowej pamięci podręcznej. Wartość domyślna to <strong>false</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Opcjonalny atrybut <strong>Boolean</strong> . Włącza/wyłącza Wysyłanie komunikatu http "<strong>Zróżnicuj: \</strong ><em>" w odpowiedzi. W przypadku domyślnego ustawienia false</em>jest wysyłany nagłówek "* Vary: \*<strong>" dla stron wyjściowych w pamięci podręcznej. W przypadku wysłania nagłówka Vary można buforować różne wersje w zależności od tego, co jest określone w nagłówku różnic. Na przykład <em>różnice: użytkownik-agenci</em> będą przechowywać różne wersje strony na podstawie agenta użytkownika, który wystawił żądanie. Wartość domyślna to * * false</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>Element &lt;outputCacheSettings&gt;

Element &lt;outputCacheSettings&gt; umożliwia tworzenie profilów pamięci podręcznej zgodnie z wcześniejszym opisem. Jedynym elementem podrzędnym elementu &lt;outputCacheSettings&gt; jest element &lt;outputCacheProfiles&gt; do konfigurowania profili pamięci podręcznej.

### <a name="the-ltsqlcachedependencygt-element"></a>Element &lt;sqlCacheDependency&gt;

Następujące atrybuty są dostępne dla elementu &lt;sqlCacheDependency&gt;.

| **Atrybut** | **Opis** |
| --- | --- |
| **dostępny** | Wymagany atrybut **Boolean** . Wskazuje, czy zmiany są sondowane. |
| **pollTime** | Opcjonalny atrybut **Int32** . Ustawia częstotliwość, z jaką SqlCacheDependency sonduje tabelę bazy danych pod kątem zmian. Ta wartość odpowiada liczbie milisekund między kolejnymi sondowami. Nie można ustawić wartości mniejszej niż 500 milisekund. Wartość domyślna to 1 minuta. |

### <a name="more-information"></a>Więcej informacji

Istnieją pewne dodatkowe informacje dotyczące konfiguracji pamięci podręcznej.

- Jeśli limit bajtów prywatnych procesu roboczego nie jest ustawiony, pamięć podręczna będzie używać jednego z następujących limitów: 

    - x86 2 GB: 800MB lub 60% fizycznej pamięci RAM, w zależności od tego, co jest mniejsze
    - x86 WŁĄCZONĄ: 1800MB lub 60% fizycznej pamięci RAM, w zależności od tego, co jest mniejsze
    - x64:1 terabajt lub 60% fizycznej pamięci RAM, w zależności od tego, co jest mniejsze
- Jeśli ustawiono zarówno limit bajtów prywatnych procesu roboczego, jak i &lt;pamięci podręcznej privateBytesLimit/&gt;, pamięć podręczna będzie używać wartości minimum obu.
- Podobnie jak w przypadku 1. x, porzucamy wpisy pamięci podręcznej i Wywołaj GC. Zbierz z dwóch powodów: 

    - Bardzo blisko limitu liczby prywatnych bajtów
    - Dostępna pamięć jest bliska lub mniejsza niż 10%
- Można skutecznie wyłączyć przycinanie i pamięć podręczną dla niewielkich dostępnych warunków pamięci przez ustawienie &lt;cache percentagePhysicalMemoryUseLimit/&gt; do 100.
- W przeciwieństwie do 1. x, 2,0 spowoduje zawieszenie przycinania i zbieranie wywołań, jeśli ostatnie GC. Funkcja Collect nie zmniejsza liczby bajtów prywatnych ani rozmiaru sterty zarządzanych przez więcej niż 1% limitu pamięci w pamięci podręcznej.

## <a name="lab1-custom-cache-dependencies"></a>Lab1: niestandardowe zależności pamięci podręcznej

1. Utwórz nową witrynę sieci Web.
2. Dodaj nowy plik XML o nazwie cache. XML i Zapisz go w folderze głównym aplikacji sieci Web.
3. Dodaj następujący kod do strony,\_metodę ładowania w kodzie domyślnym. aspx: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Dodaj następujący na górze wartości Default. aspx w widoku źródła: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Przeglądaj default. aspx. Co powiedzie?
6. Odśwież przeglądarkę. Co powiedzie?
7. Otwórz plik cache. XML i Dodaj następujący kod: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Zapisz plik cache. XML.
9. Odśwież przeglądarkę. Co powiedzie?
10. Wyjaśnij, dlaczego aktualizowany czas zamiast wyświetlania poprzednio zbuforowanych wartości:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Lab 2: używanie zależności pamięci podręcznej opartej na sondowaniu

To laboratorium używa projektu utworzonego w poprzednim module, który umożliwia edytowanie danych w bazie danych Northwind za pośrednictwem kontrolki GridView i DetailsView.

1. Otwórz projekt w programie Visual Studio 2005.
2. Uruchom narzędzie ASPNET\_regsql w bazie danych Northwind, aby włączyć bazę danych i tabelę Products. Użyj następującego polecenia w wierszu polecenia programu Visual Studio: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Dodaj następujący plik do pliku Web. config: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Dodaj nowy formularz WebForm o nazwie showData. aspx.
5. Dodaj następującą dyrektywę @ OutputCache do strony showData. aspx: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Dodaj następujący kod na stronie\_ładowania showData. aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Dodaj nową kontrolkę kontrolki SqlDataSource do showData. aspx i skonfiguruj ją tak, aby korzystała z połączenia bazy danych Northwind. Kliknij przycisk Dalej.
8. Zaznacz pola wyboru NazwaProduktu i ProductID, a następnie kliknij przycisk Dalej.
9. Kliknij przycisk Zakończ.
10. Dodaj nowy element GridView do strony showData. aspx.
11. Wybierz pozycję SqlDataSource1 z listy rozwijanej.
12. Zapisz i Przeglądaj showData. aspx. Zanotuj wyświetlony czas.

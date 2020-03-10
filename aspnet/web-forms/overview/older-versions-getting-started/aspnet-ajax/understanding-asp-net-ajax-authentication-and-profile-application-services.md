---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Informacje na temat uwierzytelniania i Usługi aplikacji profilu ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Usługa uwierzytelniania umożliwia użytkownikom podanie poświadczeń w celu uzyskania pliku cookie uwierzytelniania i jest usługą bramy zezwalającą na użytkownika niestandardowego...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: cab9acb1ffd75cca87f6c575a6abdd000235828e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640533"
---
# <a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Objaśnienie usług uwierzytelniania i profilów ASP.NET AJAX

przez [Scott Cate](https://github.com/scottcate)

[Pobierz plik PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> Usługa uwierzytelniania umożliwia użytkownikom podanie poświadczeń w celu otrzymywania plików cookie uwierzytelniania i jest usługą bramy zezwalającą na niestandardowe profile użytkowników udostępniane przez usługę ASP.NET. Korzystanie z usługi uwierzytelniania ASP.NET AJAX jest zgodne ze standardowym uwierzytelnianiem formularzy ASP.NET, dlatego aplikacje korzystające obecnie z uwierzytelniania formularzy (na przykład z kontrolą logowania) nie zostaną zerwane przez uaktualnienie do usługi uwierzytelniania AJAX.

## <a name="introduction"></a>Wprowadzenie

W ramach .NET Framework 3,5 Firma Microsoft dostarcza zmiennym uaktualnieniu środowiska; jest to nie tylko nowe środowisko programistyczne, ale nastąpi nowe funkcje języka LINQ (Language-Integrated Query) i inne ulepszenia językowe. Ponadto niektóre znane funkcje innych zestawów narzędzi, w szczególności rozszerzenia AJAX ASP.NET, są dołączane jako członkowie pierwszej klasy .NET Framework biblioteki klas podstawowych. Te rozszerzenia umożliwiają korzystanie z wielu nowych zaawansowanych funkcji klienta, w tym częściowego renderowania stron, bez konieczności odświeżania pełnych stron, możliwość uzyskiwania dostępu do usług sieci Web za pośrednictwem skryptu klienta (w tym interfejsu API profilowania ASP.NET) i szerokiego interfejsu API po stronie klienta zaprojektowana do dublowania wielu schematów formantów widzianych w zestawie kontroli po stronie serwera ASP.NET.

W tym dokumencie przedstawiono implementację i korzystanie z usług ASP.NET profilowanie i usługi uwierzytelniania formularzy, ponieważ są one udostępniane przez rozszerzenia Microsoft ASP.NET AJAX ExtensionsThe AJAX, dzięki czemu uwierzytelnianie formularzy niezwykle łatwą w obsłudze, jak również Usługa profilowania jest udostępniana za pomocą skryptu serwera proxy usługi sieci Web. Rozszerzenia AJAX obsługują również uwierzytelnianie niestandardowe za pomocą klasy AuthenticationServiceManager.

Niniejszy dokument jest oparty na wersji beta 2 programu Visual Studio 2008 i .NET Framework 3,5. W tym dokumencie przyjęto również założenie, że użytkownik pracuje z programem Visual Studio 2008 Beta 2, nie Visual Web Developer Express i zapewni wskazówki zgodne z interfejsem użytkownika programu Visual Studio. Niektóre przykłady kodu mogą korzystać z szablonów projektów dostępnych w programie Visual Web Developer Express.

## <a name="profiles-and-authentication"></a>*Profile i uwierzytelnianie*

Microsoft ASP.NET Profile i usługi uwierzytelniania są udostępniane przez system uwierzytelniania formularzy ASP.NET i są standardowymi składnikami ASP.NET. Rozszerzenia AJAX ASP.NET zapewniają dostęp do tych usług za pośrednictwem serwerów proxy skryptów za pośrednictwem dość prostego modelu w przestrzeni nazw sys. Services w bibliotece AJAX klienta.

Usługa uwierzytelniania umożliwia użytkownikom podanie poświadczeń w celu otrzymywania plików cookie uwierzytelniania i jest usługą bramy zezwalającą na niestandardowe profile użytkowników udostępniane przez usługę ASP.NET. Korzystanie z usługi uwierzytelniania ASP.NET AJAX jest zgodne ze standardowym uwierzytelnianiem formularzy ASP.NET, dlatego aplikacje korzystające obecnie z uwierzytelniania formularzy (na przykład z kontrolą logowania) nie zostaną zerwane przez uaktualnienie do usługi uwierzytelniania AJAX.

Usługa profilowania umożliwia automatyczną integrację i przechowywanie danych użytkowników na podstawie członkostwa zgodnie z usługą uwierzytelniania. Przechowywane dane są określane przez plik Web. config, a różne dostawcy usług profilowania obsługują zarządzanie danymi. Podobnie jak w przypadku usługi uwierzytelniania usługa profilu AJAX jest zgodna ze standardową usługą profilu ASP.NET, dzięki czemu strony, które aktualnie obejmują funkcje usługi profilu ASP.NET, nie powinny być dzielone przez włączenie obsługi technologii AJAX.

Dołączanie ASP.NET uwierzytelniania i usług profilowania do aplikacji wykracza poza zakres tego dokumentu. Aby uzyskać więcej informacji na temat tego tematu, zobacz artykuł dotyczący biblioteki MSDN dotyczący zarządzania użytkownikami przy użyciu członkostwa w [https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET zawiera również narzędzie do automatycznego konfigurowania członkostwa przy użyciu SQL Server, który jest domyślnym dostawcą usługi uwierzytelniania dla członkostwa ASP.NET. Aby uzyskać więcej informacji, zobacz artykuł ASP.NET SQL Server Registration Tool (ASPNET\_regsql. exe) w [https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Korzystanie z usługi uwierzytelniania ASP.NET AJAX*

W pliku Web. config musi być włączona usługa uwierzytelniania ASP.NET AJAX:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

Usługa uwierzytelniania wymaga włączenia uwierzytelniania formularzy ASP.NET i wymaga włączenia plików cookie w przeglądarce klienta (skrypt nie może włączyć sesji bez plików cookie, ponieważ sesje bez plików cookie wymagają parametrów adresu URL).

Po włączeniu i skonfigurowaniu usługi uwierzytelniania AJAX skrypt klienta może natychmiast skorzystać z obiektu sys. Services. AuthenticationService. Przede wszystkim skrypt klienta chce skorzystać z metody `login` i właściwości `isLoggedIn`. Istnieje kilka właściwości, aby zapewnić wartości domyślne dla metody logowania, która może akceptować wiele parametrów.

*Elementy członkowskie sys. Services. AuthenticationService*

*Metoda logowania:*

Metoda Login () rozpoczyna żądanie uwierzytelnienia poświadczeń użytkownika. Ta metoda jest asynchroniczna i nie blokuje wykonywania.

*Wejściowe*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| userName | Wymagany. Nazwa użytkownika do uwierzytelnienia. |
| hasło | Opcjonalne (Domyślnie wartość null). Hasło użytkownika. |
| IsPersistent | Opcjonalne (wartość domyślna to false). Czy plik cookie uwierzytelniania użytkownika powinien być utrwalany między sesjami. W przypadku wartości false użytkownik będzie wylogowywał się, gdy przeglądarka zostanie zamknięta lub sesja wygaśnie. |
| redirectUrl | Opcjonalne (Domyślnie wartość null). Adres URL, pod który ma zostać przekierowany przeglądarka po pomyślnym uwierzytelnieniu. Jeśli ten parametr ma wartość null lub jest pustym ciągiem, nie ma żadnego przekierowania. |
| customInfo | Opcjonalne (Domyślnie wartość null). Ten parametr jest aktualnie nieużywany i jest zarezerwowany do użytku w przyszłości. |
| loginCompletedCallback | Opcjonalne (Domyślnie wartość null). Funkcja do wywołania, gdy logowanie zostało pomyślnie zakończone. Jeśli ta wartość jest określona, ten parametr zastępuje właściwość defaultLoginCompleted. |
| failedCallback | Opcjonalne (Domyślnie wartość null). Funkcja do wywołania, gdy logowanie nie powiodło się. Jeśli ta wartość jest określona, ten parametr zastępuje właściwość defaultFailedCallback. |
| userContext | Opcjonalne (Domyślnie wartość null). Niestandardowe dane kontekstowe użytkownika, które powinny być przesyłane do funkcji wywołania zwrotnego. |

*Wartość zwracana:*

Ta funkcja nie zawiera wartości zwracanej. Jednak po zakończeniu wywołania tej funkcji są uwzględniane różne zachowania:

- Bieżąca strona zostanie odświeżona lub zmieniona, jeśli parametr `redirectUrl` nie miał wartości null ani pustego ciągu.
- Jeśli jednak parametr ma wartość null lub jest pustym ciągiem, parametr `loginCompletedCallback` lub właściwość `defaultLoginCompletedCallback` jest wywoływana.
- Jeśli wywołanie usługi sieci Web nie powiedzie się, zostanie wywołany parametr `failedCallback` właściwości `defaultFailedCallback`.

*Metoda wylogowywania:*

Metoda Wyloguj () usuwa plik cookie poświadczeń i Wylogowuje bieżącego użytkownika z aplikacji sieci Web.

*Wejściowe*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| redirectUrl | Opcjonalne (Domyślnie wartość null). Adres URL, pod który ma zostać przekierowany przeglądarka po pomyślnym uwierzytelnieniu. Jeśli ten parametr ma wartość null lub jest pustym ciągiem, nie ma żadnego przekierowania. |
| logoutCompletedCallback | Opcjonalne (Domyślnie wartość null). Funkcja do wywołania po pomyślnym zakończeniu wylogowywania. Jeśli ta wartość jest określona, ten parametr zastępuje właściwość defaultLogoutCompleted. |
| failedCallback | Opcjonalne (Domyślnie wartość null). Funkcja do wywołania, gdy logowanie nie powiodło się. Jeśli ta wartość jest określona, ten parametr zastępuje właściwość defaultFailedCallback. |
| userContext | Opcjonalne (Domyślnie wartość null). Niestandardowe dane kontekstowe użytkownika, które powinny być przesyłane do funkcji wywołania zwrotnego. |

*Wartość zwracana:*

Ta funkcja nie zawiera wartości zwracanej. Jednak po zakończeniu wywołania tej funkcji są uwzględniane różne zachowania:

- Bieżąca strona zostanie odświeżona lub zmieniona, jeśli parametr `redirectUrl` nie miał wartości null ani pustego ciągu.
- Jeśli jednak parametr ma wartość null lub jest pustym ciągiem, parametr `logoutCompletedCallback` lub właściwość `defaultLogoutCompletedCallback` jest wywoływana.
- Jeśli wywołanie usługi sieci Web nie powiedzie się, zostanie wywołany parametr `failedCallback` właściwości `defaultFailedCallback`.

*defaultFailedCallback Właściwość (Get, Set):*

Ta właściwość określa funkcję, która powinna zostać wywołana, jeśli wystąpi błąd komunikacji z usługą sieci Web. Powinien otrzymać delegata (lub odwołanie do funkcji).

Odwołanie do funkcji określone przez tę właściwość powinna mieć następującą sygnaturę:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Wejściowe*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| error | Określa informacje o błędzie. |
| userContext | Określa informacje kontekstu użytkownika podane podczas wywołania funkcji logowania lub wylogowania. |
| MethodName | Nazwa metody wywołującej. |

*defaultLoginCompletedCallback Właściwość (Get, Set):*

Ta właściwość określa funkcję, która ma być wywoływana po zakończeniu wywołania usługi sieci Web logowania. Powinien otrzymać delegata (lub odwołanie do funkcji).

Odwołanie do funkcji określone przez tę właściwość powinna mieć następującą sygnaturę:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Wejściowe*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| validCredentials | Określa, czy użytkownik podał prawidłowe poświadczenia. `true`, jeśli użytkownik zalogował się pomyślnie; w przeciwnym razie `false`. |
| userContext | Określa informacje kontekstu użytkownika podane podczas wywołania funkcji logowania. |
| MethodName | Nazwa metody wywołującej. |

*defaultLogoutCompletedCallback Właściwość (Get, Set):*

Ta właściwość określa funkcję, która ma być wywoływana po zakończeniu wywołania usługi sieci Web Wyloguj. Powinien otrzymać delegata (lub odwołanie do funkcji).

Odwołanie do funkcji określone przez tę właściwość powinna mieć następującą sygnaturę:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Wejściowe*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| wynik | Ten parametr będzie zawsze `null`; jest on zarezerwowany do użytku w przyszłości. |
| userContext | Określa informacje kontekstu użytkownika podane podczas wywołania funkcji logowania. |
| MethodName | Nazwa metody wywołującej. |

*islogowany Właściwość (Get):*

Ta właściwość pobiera bieżący stan uwierzytelniania użytkownika. jest on ustawiany przez obiekt ScriptManager podczas żądania strony.

Ta właściwość zwraca `true`, jeśli użytkownik jest aktualnie zalogowany; w przeciwnym razie zwraca `false`.

*Path — właściwość (Get, Set):*

Ta właściwość programowo określa lokalizację usługi sieci Web uwierzytelniania. Może służyć do przesłonięcia domyślnego dostawcy uwierzytelniania oraz jednego zestawu w właściwości Path węzła podrzędnego AuthenticationService formantu ScriptManager (Aby uzyskać więcej informacji, zobacz Korzystanie z niestandardowego dostawcy usługi uwierzytelniania. temat poniżej).

Należy pamiętać, że lokalizacja domyślnej usługi uwierzytelniania nie jest zmieniana. Jednak ASP.NET AJAX pozwala określić lokalizację usługi sieci Web, która zapewnia ten sam interfejs klasy co serwer proxy usługi uwierzytelniania ASP.NET AJAX.

Należy również pamiętać, że ta właściwość nie powinna być ustawiona na wartość, która kieruje żądanie skryptu poza bieżącą lokację. Ponieważ bieżąca aplikacja nie odbiera poświadczeń uwierzytelniania, będzie bezużyteczny; Ponadto, podstawowa technologia AJAX nie powinna ogłaszać żądań między lokacjami i może generować wyjątek zabezpieczeń w przeglądarce klienta.

Ta właściwość jest obiektem `String` reprezentującym ścieżkę do usługi sieci Web uwierzytelniania.

*Właściwość Timeout (Get, Set):*

Ta właściwość określa czas oczekiwania na zaczekanie usługi uwierzytelniania przed założeniem, że żądanie logowania nie powiodło się. Jeśli upłynął limit czasu podczas oczekiwania na zakończenie wywołania, wywołanie zwrotne żądania nie zostanie wywołane i wywołanie nie zostanie zakończone.

Ta właściwość jest obiektem `Number` reprezentującym liczbę milisekund oczekiwania na wyniki usługi uwierzytelniania.

*Przykład kodu: Logowanie do usługi uwierzytelniania*

Poniższy znacznik to przykładowa strona ASP.NET z prostym wywołaniem skryptu do metody logowania i wylogowywania klasy AuthenticationService.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>Uzyskiwanie dostępu do danych profilowania ASP.NET za pomocą technologii AJAX

Usługa profilowania ASP.NET jest również dostępna za pomocą rozszerzeń ASP.NET AJAX. Ponieważ usługa profilowania ASP.NET oferuje bogaty, szczegółowy interfejs API, dzięki któremu można przechowywać i pobierać dane użytkowników, może to być doskonałe narzędzie do wydajnej produktywności.

Usługa profilu musi być włączona w pliku Web. config; nie jest to domyślne ustawienie. W tym celu należy się upewnić, że `profileService` element podrzędny Enabled = true określony w pliku Web. config oraz że określono, które właściwości mogą być odczytywane lub zapisywane w następujący sposób:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

Należy również skonfigurować usługę profilu. Mimo że konfiguracja usługi profilowania znajduje się poza zakresem tego oficjalnego dokumentu, jest to wartościowa, że grupy zdefiniowane w ustawieniach konfiguracji profilu będą dostępne jako właściwości podrzędne nazwy grupy. Na przykład, jeśli określono następującą sekcję profilu:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Skrypt klienta będzie mógł uzyskać dostęp do nazwy, adresu. wiersz1, Address. wiersz2, Address. City, Address. State, Address. zip i BackgroundColor jako właściwości pola Properties klasy ProfileService.

Po skonfigurowaniu usługi profilowania AJAX będzie ona natychmiast dostępna na stronach. jednak będzie musiał zostać załadowany raz przed użyciem.

*Elementy członkowskie sys. Services. ProfileService*

*pole właściwości:*

W polu właściwości są ujawniane wszystkie skonfigurowane dane profilu jako właściwości podrzędne, do których można odwoływać się za pomocą Konwencji operatora kropki-nazwy. Właściwości, które są elementami podrzędnymi grup właściwości, są określane jako GroupName. PropertyName. W przykładowej konfiguracji profilu przedstawionej powyżej, aby uzyskać stan użytkownika, można użyć następującego identyfikatora:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*Metoda ładowania:*

Ładuje wybraną listę lub wszystkie właściwości z serwera.

*Wejściowe*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| propertyName | Opcjonalne (Domyślnie wartość null). Właściwości, które mają zostać załadowane z serwera. |
| loadCompletedCallback | Opcjonalne (Domyślnie wartość null). Funkcja do wywołania po zakończeniu ładowania. |
| failedCallback | Opcjonalne (Domyślnie wartość null). Funkcja wywoływana w przypadku wystąpienia błędu. |
| userContext | Opcjonalne (Domyślnie wartość null). Informacje kontekstowe do przesłania do funkcji wywołania zwrotnego. |

Funkcja load nie ma zwracanej wartości. Jeśli wywołanie zakończyło się pomyślnie, wywoła parametr `loadCompletedCallback` lub właściwość `defaultLoadCompletedCallback`. Jeśli wywołanie nie powiodło się lub upłynął limit czasu, zostanie wywołany parametr `failedCallback` lub właściwość `defaultFailedCallback`.

Jeśli parametr `propertyNames` nie zostanie podany, wszystkie właściwości skonfigurowane do odczytu są pobierane z serwera.

*Zapisz metodę:*

Metoda Save () zapisuje określoną listę właściwości (lub wszystkie właściwości) do profilu ASP.NET użytkownika.

*Wejściowe*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| propertyName | Opcjonalne (Domyślnie wartość null). Właściwości, które mają zostać zapisane na serwerze. |
| saveCompletedCallback | Opcjonalne (Domyślnie wartość null). Funkcja do wywołania po zakończeniu zapisywania. |
| failedCallback | Opcjonalne (Domyślnie wartość null). Funkcja wywoływana w przypadku wystąpienia błędu. |
| userContext | Opcjonalne (Domyślnie wartość null). Informacje kontekstowe do przesłania do funkcji wywołania zwrotnego. |

Funkcja Save nie ma zwracanej wartości. Jeśli wywołanie zakończyło się pomyślnie, wywoła Właściwość `saveCompletedCallback` lub `defaultSaveCompletedCallback`. Jeśli wywołanie nie powiodło się lub upłynął limit czasu, zostanie wywołana Właściwość `failedCallback` lub `defaultFailedCallback`.

Jeśli parametr `propertyNames` ma wartość null, wszystkie właściwości profilu zostaną wysłane do serwera, a serwer zdecyduje, które właściwości można zapisać, a które nie.

*defaultFailedCallback Właściwość (Get, Set):*

Ta właściwość określa funkcję, która powinna zostać wywołana, jeśli wystąpi błąd komunikacji z usługą sieci Web. Powinien otrzymać delegata (lub odwołanie do funkcji).

Odwołanie do funkcji określone przez tę właściwość powinna mieć następującą sygnaturę:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Wejściowe*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| Błąd | Określa informacje o błędzie. |
| userContext | Określa informacje kontekstu użytkownika podane podczas wywołania funkcji ładowania lub zapisywania. |
| MethodName | Nazwa metody wywołującej. |

*defaultSaveCompleted Właściwość (Get, Set):*

Ta właściwość określa funkcję, która powinna być wywoływana po zakończeniu zapisywania danych profilu użytkownika. Powinien otrzymać delegata (lub odwołanie do funkcji).

Odwołanie do funkcji określone przez tę właściwość powinna mieć następującą sygnaturę:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Wejściowe*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| numPropsSaved | Określa liczbę zapisanych właściwości. |
| userContext | Określa informacje kontekstu użytkownika podane podczas wywołania funkcji ładowania lub zapisywania. |
| MethodName | Nazwa metody wywołującej. |

*defaultLoadCompleted Właściwość (Get, Set):*

Ta właściwość określa funkcję, która powinna być wywoływana po zakończeniu ładowania danych profilu użytkownika. Powinien otrzymać delegata (lub odwołanie do funkcji).

Odwołanie do funkcji określone przez tę właściwość powinna mieć następującą sygnaturę:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Wejściowe*

| **Nazwa parametru** | **Znaczenie** |
| --- | --- |
| numPropsLoaded | Określa liczbę załadowanych właściwości. |
| userContext | Określa informacje kontekstu użytkownika podane podczas wywołania funkcji ładowania lub zapisywania. |
| MethodName | Nazwa metody wywołującej. |

*Path — właściwość (Get, Set):*

Ta właściwość programowo określa lokalizację usługi sieci Web profilu. Może służyć do przesłonięcia domyślnego dostawcy usługi profilu, a także do jednego zestawu w właściwości Path węzła podrzędnego ProfileService formantu ScriptManager.

Należy pamiętać, że lokalizacja domyślnej usługi profilu nie jest zmieniana. Jednak ASP.NET AJAX pozwala określić lokalizację usługi sieci Web, która zapewnia ten sam interfejs klasy co serwer proxy usługi uwierzytelniania ASP.NET AJAX.

Należy również pamiętać, że ta właściwość nie powinna być ustawiona na wartość, która kieruje żądanie skryptu poza bieżącą lokację. Podstawowa technologia AJAX nie powinna publikować żądań między lokacjami i może generować wyjątek zabezpieczeń w przeglądarce klienta.

Ta właściwość jest obiektem `String` reprezentującym ścieżkę do usługi sieci Web profilu.

*Właściwość Timeout (Get, Set):*

Ta właściwość określa czas oczekiwania usługi profilu przed założeniem, że żądanie załadowania lub zapisania nie powiodło się. Jeśli upłynął limit czasu podczas oczekiwania na zakończenie wywołania, wywołanie zwrotne żądania nie zostanie wywołane i wywołanie nie zostanie zakończone.

Ta właściwość jest obiektem `Number` reprezentującym liczbę milisekund oczekiwania na wyniki z usługi profilu.

*Przykład kodu: ładowanie danych profilu podczas ładowania strony*

Poniższy kod sprawdzi, czy użytkownik jest uwierzytelniony, a jeśli tak, załaduje kolor tła preferowany przez użytkownika jako stronę.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Korzystanie z niestandardowego dostawcy usługi uwierzytelniania*

Rozszerzenia AJAX ASP.NET umożliwiają tworzenie niestandardowego dostawcy usługi uwierzytelniania skryptów przez udostępnienie funkcjonalności za pomocą niestandardowej usługi sieci Web. Aby można było użyć usługi sieci Web, należy uwidocznić dwie metody, `Login` i `Logout`; te metody muszą być określone przy użyciu tych samych sygnatur metod co domyślna usługa sieci Web uwierzytelniania ASP.NET AJAX.

Po utworzeniu niestandardowej usługi sieci Web należy określić jej ścieżkę, na przykład na stronie, programowo w kodzie lub za pośrednictwem skryptu klienta.

*Aby ustawić ścieżkę deklaratywnie:*

Aby ustawić ścieżkę deklaratywnie, Uwzględnij element podrzędny AuthenticationService obiektu ScriptManager na stronie ASP.NET:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Aby ustawić ścieżkę w kodzie:*

Aby programowo ustawić ścieżkę, określ ścieżkę za pośrednictwem wystąpienia Menedżera skryptów:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Aby ustawić ścieżkę w skrypcie:*

Aby programowo ustawić ścieżkę w skrypcie, użyj właściwości `path` klasy AuthenticationService:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Przykładowa usługa sieci Web na potrzeby uwierzytelniania niestandardowego*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Podsumowanie

Usługi ASP.NET Services — szczególnie takie jak profilowanie, członkostwo i usługi uwierzytelniania — są łatwo uwidocznione w języku JavaScript w przeglądarce klienta. Dzięki temu deweloperzy mogą bezproblemowo integrować kod po stronie klienta z mechanizmem uwierzytelniania bez względu na kontrolki, takie jak elementy UpdatePanel, aby przeprowadzić duże podnoszenie. Dane profilowe można także chronić z poziomu klienta, wykorzystując ustawienia konfiguracji sieci Web. żadne dane nie są dostępne domyślnie, a deweloperzy muszą wyrazić zgodę na właściwości profilu.

Ponadto tworząc uproszczone implementacje usług sieci Web z równoważnymi sygnaturami metod, deweloperzy mogą tworzyć niestandardowych dostawców skryptów dla tych wewnętrznych usług ASP.NET. Obsługa tych technik upraszcza opracowywanie rozbudowanych aplikacji klienckich, dzięki czemu deweloperzy mają szeroką gamę możliwości.

## <a name="bio"></a>*Materiał*

Scott Cate pracował z technologiami sieci Web firmy Microsoft od 1997 i jest prezydentem myKB.com ([www.myKB.com](http://www.myKB.com)), w którym wyspecjalizowany jest pisanie aplikacji opartych na ASP.NET, które są zgodne z podstawowymi rozwiązaniami oprogramowania. W witrynie Scotta można skontaktować się z pocztą e-mail na [scott.cate@myKB.com](mailto:scott.cate@myKB.com) lub w blogu w witrynie [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Poprzednie](understanding-asp-net-ajax-updatepanel-triggers.md)
> [dalej](understanding-asp-net-ajax-localization.md)

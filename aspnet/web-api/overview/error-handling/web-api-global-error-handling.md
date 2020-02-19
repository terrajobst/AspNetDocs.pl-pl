---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Globalna obsługa błędów w ASP.NET Web API 2-ASP.NET 4. x
author: davidmatson
description: Omówienie globalnej obsługi błędów w ASP.NET Web API 2 dla ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 94f2d6d31d0b37f9bb0077e6258c70a2dfb1918d
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457742"
---
# <a name="global-error-handling-in-aspnet-web-api-2"></a>Globalna obsługa błędów w ASP.NET Web API 2

przez [David Matson](https://github.com/davidmatson), [Rick Anderson](https://twitter.com/RickAndMSFT)

Ten temat zawiera omówienie globalnej obsługi błędów w ASP.NET Web API 2 dla ASP.NET 4. x. Obecnie nie ma łatwego sposobu na interfejs API sieci Web do rejestrowania lub obsługi błędów globalnie. Niektóre Nieobsłużone wyjątki mogą być przetwarzane za pośrednictwem [filtrów wyjątków](exception-handling.md), ale istnieją różne przypadki, w których nie można obsłużyć filtrów wyjątków. Na przykład:

1. Wyjątki zgłoszone przez konstruktory kontrolerów.
2. Wyjątki zgłoszone przez programy obsługi komunikatów.
3. Wyjątki zgłoszone podczas routingu.
4. Wyjątki zgłoszone podczas serializacji zawartości odpowiedzi.

Chcemy zapewnić prosty, spójny sposób rejestrowania i obsługi (tam, gdzie to możliwe) tych wyjątków. 

Istnieją dwa główne przypadki obsługi wyjątków, przypadek, w którym możemy wysłać odpowiedź na błąd i przypadek, gdzie wszystko, co możemy zrobić, rejestruje wyjątek. Przykładem dla ostatniego przypadku jest zgłaszanie wyjątku w środku zawartości odpowiedzi przesyłania strumieniowego; w takim przypadku zbyt późno na wysłanie nowego komunikatu odpowiedzi, ponieważ kod stanu, nagłówki i częściowa zawartość już zniknęły w sieci, dlatego po prostu przerywamy połączenie. Mimo że nie można obsłużyć tego wyjątku, aby utworzyć nowy komunikat odpowiedzi, nadal obsługujemy rejestrowanie wyjątku. W przypadkach, w których możemy wykryć błąd, możemy zwrócić odpowiednią odpowiedź na błąd, jak pokazano na poniższej liście:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Istniejące opcje

Oprócz [filtrów wyjątków](exception-handling.md), [procedury obsługi komunikatów](../advanced/http-message-handlers.md) mogą być używane dzisiaj do obserwowania wszystkich odpowiedzi na poziomie 500, ale działające na tych odpowiedzi jest trudne, ponieważ nie mają kontekstu o pierwotnym błędzie. Procedury obsługi komunikatów mają także niektóre z tych samych ograniczeń co filtry wyjątków dotyczące przypadków, w których mogą one obsłużyć. Mimo że interfejs API sieci Web ma infrastrukturę śledzenia, która przechwytuje warunki błędu, Infrastruktura śledzenia jest przeznaczona do celów diagnostycznych i nie jest zaprojektowana ani odpowiednia do uruchamiania w środowiskach produkcyjnych. Globalna obsługa wyjątków i rejestrowanie powinna być usługami, które mogą być uruchamiane w trakcie produkcji i są podłączane do istniejących rozwiązań monitorowania (na przykład [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Przegląd rozwiązania

 Udostępniamy dwie nowe, wymienne usługi, [interfejsu iexceptionlogger](../releases/whats-new-in-aspnet-web-api-21.md) i IExceptionHandler, aby rejestrować i obsługiwać Nieobsłużone wyjątki. Usługi są bardzo podobne i mają dwie główne różnice:

1. Obsługujemy rejestrowanie wielu rejestratorów wyjątków, ale tylko jednego programu obsługi wyjątków.
2. Rejestratory wyjątków zawsze są wywoływane, nawet jeśli zamierzasz przerwać połączenie. Programy obsługi wyjątków są wywoływane tylko wtedy, gdy nadal można wybrać komunikat odpowiedzi do wysłania.

Obie usługi zapewniają dostęp do kontekstu wyjątku zawierającego istotne informacje od punktu, w którym został wykryty wyjątek, szczególnie [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), zgłoszony wyjątek i źródło wyjątku (szczegóły poniżej).

### <a name="design-principles"></a>Zasady projektowania

1. **Brak zmian powodujących przerwanie** Ponieważ ta funkcja jest dodawana w wersji pomocniczej, jedno ważne ograniczenie wpływające na rozwiązanie polega na tym, że nie ma żadnych znaczących zmian w przypadku kontraktów lub zachowania. To ograniczenie ogranicza pewne czynności, które należy wykonać w ramach istniejących bloków catch, włączając wyjątki do 500 odpowiedzi. To dodatkowe czyszczenie jest coś, co możemy wziąć pod uwagę w przypadku kolejnej wersji głównej. Jeśli jest to ważne, należy zagłosować na nim przy [ASP.NET użytkownika interfejsu API sieci Web](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Zachowanie spójności z konstrukcjami interfejsów API sieci Web** Potok filtru internetowego interfejsu API to świetny sposób na objęcie problemów związanych z wycinaniem i elastycznością zastosowania logiki w zakresie konkretnych czynności, specyficznych dla kontrolera lub globalnych. Filtry, w tym filtry wyjątków, zawsze mają konteksty akcji i kontrolerów, nawet w przypadku zarejestrowania w zakresie globalnym. Ta umowa ma sens dla filtrów, ale oznacza, że filtry wyjątków, nawet z zakresem globalnym, nie są dobrym rozwiązaniem w przypadku niektórych przypadków obsługi wyjątków, takich jak wyjątki od programów obsługi komunikatów, gdzie nie istnieje kontekst akcji ani kontrolera. Jeśli chcemy używać elastycznego zakresu, który jest obsługiwany przez filtry dla obsługi wyjątków, nadal potrzebujemy filtrów wyjątków. Jeśli jednak musimy obsłużyć wyjątek poza kontekstem kontrolera, potrzebujemy również oddzielnej konstrukcji dla pełnej globalnej obsługi błędów (coś bez kontekstu kontrolera i ograniczeń kontekstu akcji).

### <a name="when-to-use"></a>Kiedy używać

- Rejestratory wyjątków to rozwiązanie do wyświetlania wszystkich nieobsłużonych wyjątków przechwyconych przez internetowy interfejs API.
- Obsługa wyjątków to rozwiązanie służące do dostosowywania wszystkich możliwych odpowiedzi do nieobsłużonych wyjątków przechwyconych przez internetowy interfejs API.
- Filtry wyjątków są najłatwiejszym rozwiązaniem do przetwarzania nieobsługiwanych wyjątków podzbioru związanych z określoną akcją lub kontrolerem.

### <a name="service-details"></a>Szczegóły usługi

 Interfejsy usługi rejestrowania wyjątków i obsługi są prostymi metodami asynchronicznymi, które pobierają odpowiednie konteksty: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Udostępniamy również klasy bazowe dla obu tych interfejsów. Zastępowanie metod Core (Synchronize lub Async) jest wymagane do zarejestrowania lub obsłużenia w zalecanej godzinie. W przypadku rejestrowania, Klasa bazowa `ExceptionLogger` zapewnia, że podstawowa Metoda rejestrowania jest wywoływana tylko raz dla każdego wyjątku (nawet jeśli później propaguje dalej stos wywołań i zostanie przechwycony ponownie). Klasa bazowa `ExceptionHandler` wywoła podstawową metodę obsługi tylko dla wyjątków w górnej części stosu wywołań, ignorując starsze zagnieżdżone bloki catch. (Uproszczone wersje tych klas podstawowych znajdują się w załączniku poniżej). Zarówno `IExceptionLogger`, jak i `IExceptionHandler` otrzymują informacje o wyjątku za pośrednictwem `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Gdy struktura wywołuje rejestratora wyjątków lub procedurę obsługi wyjątków, zawsze będzie zapewniać `Exception` i `Request`. Z wyjątkiem testów jednostkowych, będzie również zawsze zapewniać `RequestContext`. Będzie rzadko zapewniał `ControllerContext` i `ActionContext` (tylko w przypadku wywołania z bloku catch dla filtrów wyjątków). Bardzo rzadko zapewnia `Response`(tylko w niektórych przypadkach usług IIS, gdy w trakcie próby zapisania odpowiedzi). Należy pamiętać, że niektóre z tych właściwości mogą być `null` przed uzyskaniem dostępu do elementów członkowskich klasy wyjątków, aby sprawdzić, czy nie `null`.`CatchBlock` jest ciągiem wskazującym, który blok catch wykrywa wyjątek. Ciągi bloku catch są następujące:

- HttpServer (Metoda SendAsync)
- HttpControllerDispatcher (Metoda SendAsync)
- HttpBatchHandler (Metoda SendAsync)
- IExceptionFilter (ApiController przetworzenia potoku filtru wyjątków w wywoływanie ExecuteAsync)
- Host OWIN:

    - HttpMessageHandlerAdapter. BufferResponseContentAsync (do buforowania danych wyjściowych)
    - HttpMessageHandlerAdapter. CopyResponseContentAsync (do przesyłania strumieniowego danych wyjściowych)
- Host sieci Web:

    - HttpControllerHandler. WriteBufferedResponseContentAsync (do buforowania danych wyjściowych)
    - HttpControllerHandler. WriteStreamedResponseContentAsync (do przesyłania strumieniowego danych wyjściowych)
    - HttpControllerHandler. WriteErrorResponseContentAsync (w przypadku błędów odzyskiwania błędów w trybie buforowanego wyjścia)

Lista ciągów bloku catch jest również dostępna za pośrednictwem statycznych właściwości tylko do odczytu. (Rdzeń podstawowego bloku catch znajduje się na statycznej ExceptionCatchBlocks; reszta jest wyświetlana w jednej klasie statycznej każdy dla OWIN i hosta sieci Web).`IsTopLevelCatchBlock` jest przydatne w przypadku stosowania zalecanego wzorca obsługi wyjątków tylko w górnej części stosu wywołań. Zamiast przełączać wyjątki do 500 odpowiedzi wszędzie tam, gdzie występuje zagnieżdżony blok catch, program obsługi wyjątków może pozwolić na propagację wyjątków do momentu, gdy mają być widoczne dla hosta.

Oprócz `ExceptionContext`, rejestrator pobiera jedną więcej informacji za pośrednictwem pełnego `ExceptionLoggerContext`:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

Druga właściwość, `CanBeHandled`, umożliwia Rejestratorowi zidentyfikowanie wyjątku, którego nie można obsłużyć. Gdy połączenie zostanie przerwane i nie można wysłać nowego komunikatu odpowiedzi, rejestratory będą wywoływane, ale procedura obsługi ***nie*** zostanie wywołana, a rejestratory mogą zidentyfikować ten scenariusz z tej właściwości.

W przypadku dodatkowych `ExceptionContext`program obsługi pobiera jeszcze jedną właściwość, którą można ustawić dla pełnego `ExceptionHandlerContext`, aby obsłużyć wyjątek:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Procedura obsługi wyjątków wskazuje, że obsłużył wyjątek przez ustawienie właściwości `Result` na wynik akcji (na przykład [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx)lub wynik niestandardowy). Jeśli właściwość `Result` ma wartość null, wyjątek nie jest obsługiwany, a oryginalny wyjątek zostanie ponownie wygenerowany.

W przypadku wyjątków w górnej części stosu wywołań wprowadziliśmy dodatkowy krok, aby upewnić się, że odpowiedź jest odpowiednia dla wywołań interfejsu API. Jeśli wyjątek propaguje do hosta, obiekt wywołujący zobaczy żółty ekran zgonu lub inny dostarczony Host odpowiedzi, który zwykle jest w formacie HTML i nie jest zazwyczaj odpowiednią odpowiedzią błędu interfejsu API. W takich przypadkach wynik zaczyna się nierówna null i tylko wtedy, gdy niestandardowa procedura obsługi wyjątków jawnie ustawi ją z powrotem na `null` (nieobsłużony), spowoduje to przekazanie wyjątku do hosta. Ustawienie `Result` na `null` w takich przypadkach może być przydatne w przypadku dwóch scenariuszy:

1. OWIN hostowany interfejs API sieci Web z niestandardowym obsługą wyjątków zarejestrowanego przed/poza interfejsem API sieci Web.
2. Debugowanie lokalne za pośrednictwem przeglądarki, gdzie żółty ekran zgonu jest w rzeczywistości przydatną odpowiedzią na nieobsługiwany wyjątek.

W przypadku rejestratorów wyjątków i obsługi wyjątków nie robimy niczego do odzyskania, jeśli Rejestrator lub procedura obsługi zgłasza wyjątek. (Inne niż to, aby umożliwić propagowanie wyjątku, wystaw opinię w dolnej części tej strony, jeśli masz lepsze podejście). Umowa dotycząca rejestratorów wyjątków i programów obsługi polega na tym, że nie powinny one zezwalać na propagację wyjątków do ich wywołujących; w przeciwnym razie, wyjątek zostanie po prostu rozpropagowany, często cały sposób do hosta, co spowoduje błąd HTML (np. ASP. Żółty ekran sieci jest wysyłany z powrotem do klienta (zazwyczaj nie jest to preferowana opcja dla wywołań interfejsu API, które oczekują JSON lub XML).

## <a name="examples"></a>Przykłady

### <a name="tracing-exception-logger"></a>Rejestrator wyjątków śledzenia

Rejestrator wyjątków poniżej wysyła dane wyjątku do skonfigurowanych źródeł śladów (w tym okna dane wyjściowe debugowania w programie Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Procedura obsługi wyjątków niestandardowych komunikatów o błędach

Poniższy poniżej generuje niestandardową odpowiedź na błędy dla klientów, w tym adres e-mail do kontaktowania się z pomocą techniczną.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Rejestrowanie filtrów wyjątków

Jeśli tworzysz projekt przy użyciu szablonu projektu "aplikacja sieci Web ASP.NET MVC 4", umieść kod konfiguracyjny interfejsu API sieci Web wewnątrz klasy `WebApiConfig`, w folderze *aplikacji/_Start* :

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Dodatek: Szczegóły klasy bazowej

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]

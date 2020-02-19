---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: Korzystanie z metod asynchronicznych w ASP.NET 4,5 | Microsoft Docs
author: Rick-Anderson
description: Ten samouczek zawiera informacje na temat tworzenia asynchronicznej aplikacji ASP.NET Web Forms przy użyciu Visual Studio Express 2012 dla sieci Web, która jest bezpłatna...
ms.author: riande
ms.date: 01/02/2019
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 7abc3d7acc60d7d868958f2a313bc408f96c95a4
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457573"
---
# <a name="using-asynchronous-methods-in-aspnet-45"></a>Używanie metod asynchronicznych na platformie ASP.NET 4.5

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ten samouczek zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms asynchronicznie przy użyciu [Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/11), która jest bezpłatną wersją Microsoft Visual Studio. Możesz również użyć [programu Visual Studio 2012](https://www.microsoft.com/visualstudio/11). Poniższe sekcje zostały uwzględnione w tym samouczku.
> 
> - [Jak żądania są przetwarzane przez pulę wątków](#HowRequestsProcessedByTP)
> - [Wybieranie metod synchronicznych lub asynchronicznych](#ChoosingSyncVasync)
> - [Przykładowa aplikacja](#SampleApp)
> - [Strona synchroniczna elementy Gizmo](#GizmosSynch)
> - [Tworzenie asynchronicznej strony elementy Gizmo](#CreatingAsynchGizmos)
> - [Wykonywanie wielu operacji równolegle](#Parallel)
> - [Korzystanie z tokenu anulowania](#CancelToken)
> - [Konfiguracja serwera dla wywołań usługi sieci Web o wysokim poziomie współbieżności/dużych opóźnień](#ServerConfig)
> 
> Pełny przykład jest dostępny dla tego samouczka w  
> [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) w witrynie [GitHub](https://github.com/) .

ASP.NET 4,5 strony sieci Web w połączeniu z [platformą .net 4,5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) umożliwiają rejestrowanie metod asynchronicznych zwracających obiekt typu [zadanie](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). W .NET Framework 4 wprowadzono asynchroniczną koncepcję programistyczną, o której mowa jako [zadanie](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) i ASP.NET 4,5 obsługuje [zadanie](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Zadania są reprezentowane przez typ **zadania** i powiązane typy w przestrzeni nazw [System. Threading. Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) . .NET Framework 4,5 jest oparta na tej asynchronicznej obsłudze z słowami kluczowymi [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) , które współpracują z obiektami [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) znacznie mniej skomplikowane niż poprzednie podejście asynchroniczne. Słowo kluczowe [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) jest skrótową składnią wskazującą, że fragment kodu powinien asynchronicznie oczekiwać na inne fragmenty kodu. Słowo kluczowe [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) reprezentuje wskazówkę, której można użyć do oznaczenia metod jako metod asynchronicznych opartych na zadaniach. Połączenie obiektów **await**, **Async**i **Task** ułatwia pisanie kodu asynchronicznego w programie .NET 4,5. Nowy model metod asynchronicznych jest nazywany *wzorcem asynchronicznym opartym na zadaniach* (**TAP**). W tym samouczku założono, że masz pewną wiedzę z funkcją asynchronicznego programowania przy użyciu słów kluczowych [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) oraz przestrzeni nazw [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) .

Aby uzyskać więcej informacji na temat używania [oczekujących](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [asynchronicznych](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) słów kluczowych oraz przestrzeni nazw [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) , zobacz następujące odwołania.

- [Oficjalny dokument: asynchroniczności na platformie .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Asynchroniczne/await — często zadawane pytania](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programowanie asynchroniczne programu Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Jak żądania są przetwarzane przez pulę wątków

Na serwerze sieci Web .NET Framework obsługuje pulę wątków, które są używane do obsługi żądań ASP.NET. Po nadejściu żądania wątek z puli jest wysyłany w celu przetworzenia tego żądania. Jeśli żądanie jest przetwarzane synchronicznie, wątek, który przetwarza żądanie, jest zajęty podczas przetwarzania żądania, a ten wątek nie może obsłużyć innego żądania.   
  
Może to nie być problem, ponieważ pula wątków może być zbyt duża, aby pomieścić wiele zajętych wątków. Liczba wątków w puli wątków jest jednak ograniczona (domyślna wartość maksymalna dla programu .NET 4,5 to 5 000). W dużych aplikacjach z wysokim poziomem współbieżności długotrwałych żądań wszystkie dostępne wątki mogą być zajęte. Ten stan jest określany jako zablokowanie wątku. Po osiągnięciu tego warunku serwer sieci Web będzie kolejkować żądania. Jeśli kolejka żądań zostanie zapełniona, serwer sieci Web odrzuca żądania ze stanem HTTP 503 (serwer jest zbyt zajęty). Pula wątków CLR ma ograniczenia dotyczące nowych iniekcji wątków. Jeśli współbieżność odbywa się na rozerwanie (oznacza to, że witryna sieci Web może nagle uzyskać dużą liczbę żądań), a wszystkie dostępne wątki żądań są zajęte ze względu na wywołania zaplecza z dużym opóźnieniem, ograniczenie liczby iniekcji wątków może sprawiać, że aplikacja reaguje bardzo źle. Ponadto każdy nowy wątek dodany do puli wątków ma narzuty (np. 1 MB pamięci stosu). Aplikacja internetowa korzystająca z metod synchronicznych do obsługi połączeń o dużym opóźnieniu, gdy pula wątków rośnie do wartości domyślnej programu .NET 4,5 w liczbie 5 000 wątków zużywa około 5 GB więcej pamięci niż aplikacja może korzystać z tych samych żądań metody asynchroniczne i tylko 50 wątków. Gdy wykonujesz zadania asynchroniczne, nie zawsze używasz wątku. Na przykład po wprowadzeniu asynchronicznego żądania usługi sieci Web ASP.NET nie będzie używać żadnych wątków między wywołaniem metody **asynchronicznej** a **oczekującą**. Użycie puli wątków do obsługi żądań o dużym opóźnieniu może prowadzić do znacznego wykorzystania pamięci i niedostatecznego użycia sprzętu serwera.

## <a name="processing-asynchronous-requests"></a>Przetwarzanie żądań asynchronicznych

W aplikacjach sieci Web, które widzą dużą liczbę równoczesnych żądań podczas uruchamiania lub mają obciążenie pierścieniowe (w przypadku gwałtownego wydłużenia współbieżności), dzięki czemu wywołania usługi sieci Web asynchroniczne zwiększą czas odpowiedzi aplikacji. Żądania asynchroniczne zajmują ten sam czas do przetworzenia w ramach żądania synchronicznego. Na przykład, jeśli żądanie wykonuje wywołanie usługi sieci Web, które wymaga wykonania dwóch sekund, żądanie trwa dwa sekundy niezależnie od tego, czy jest wykonywane synchronicznie czy asynchronicznie. Jednak podczas wywołania asynchronicznego wątek nie jest blokowany przed odpowiedzią na inne żądania podczas oczekiwania na ukończenie pierwszego żądania. W związku z tym żądania asynchroniczne uniemożliwiają zwiększenie liczby kolejkowania żądań i puli wątków, gdy istnieje wiele współbieżnych żądań, które wywołują długotrwałe operacje.

## <a id="ChoosingSyncVasync"></a>Wybieranie metod synchronicznych lub asynchronicznych

Ta sekcja zawiera wskazówki dotyczące sytuacji, w których należy używać metod synchronicznych lub asynchronicznych. Są to jedynie wskazówki. osobno sprawdzaj poszczególne aplikacje, aby określić, czy metody asynchroniczne pomagają w wydajności.

Ogólnie rzecz biorąc, należy użyć metod synchronicznych dla następujących warunków:

- Operacje są proste lub krótkoterminowe.
- Prostota jest bardziej ważna niż wydajność.
- Operacje są wykonywane głównie przez procesor CPU zamiast operacji, które obejmują rozbudowane obciążenie dysku lub sieci. Korzystanie z metod asynchronicznych w operacjach powiązanych z PROCESORem nie zapewnia żadnych korzyści i daje większe obciążenie.

Ogólnie rzecz biorąc, należy używać metod asynchronicznych dla następujących warunków:

- Dzwonisz do usług, które mogą być używane za pośrednictwem metod asynchronicznych, i używasz platformy .NET w wersji 4,5 lub nowszej.
- Operacje są powiązane z siecią lub we/wy, a nie związane z PROCESORem.
- Równoległość jest ważniejsze niż prostota kodu.
- Chcesz zapewnić mechanizm, który umożliwia użytkownikom anulowanie długotrwałego żądania.
- Gdy korzyść przełączenia wątków powoduje obniżenie kosztów przełączenia kontekstu. Ogólnie rzecz biorąc, należy wprowadzić metodę asynchroniczną, jeśli metoda synchroniczna blokuje wątek żądania ASP.NET podczas wykonywania zadań. W wyniku wywołania asynchronicznego wątek żądania ASP.NET nie jest blokowany, nie działa podczas oczekiwania na ukończenie żądania usługi sieci Web.
- Testy pokazują, że operacje blokowania to wąskie gardła w wydajności lokacji i że usługi IIS mogą obsłużyć więcej żądań, używając metod asynchronicznych dla tych wywołań blokowania.

  Przykład do pobrania pokazuje, jak efektywnie używać metod asynchronicznych. Dostarczony przykład został zaprojektowany w celu zapewnienia prostej demonstracji programowania asynchronicznego w ASP.NET 4,5. Przykład nie jest przeznaczony do architektury referencyjnej dla programowania asynchronicznego w ASP.NET. Przykładowy program wywołuje metody internetowego [interfejsu API ASP.NET](../../../web-api/index.md) , które z kolei powodują [zadanie Call. Opóźnij](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) , aby symulować długotrwałe wywołania usługi sieci Web. Większość aplikacji produkcyjnych nie będzie pokazywała oczywistych korzyści związanych z korzystaniem z metod asynchronicznych.   
  
Niektóre aplikacje wymagają, aby wszystkie metody były asynchroniczne. Często konwersja kilku metod synchronicznych na metody asynchroniczne zapewnia optymalny wzrost wydajności dla wymaganej ilości pracy.

## <a id="SampleApp"></a>Przykładowa aplikacja

Przykładową aplikację można pobrać z [https://github.com/RickAndMSFT/Async-ASP.NET](https://github.com/RickAndMSFT/Async-ASP.NET) w witrynie [GitHub](https://github.com/) . Repozytorium składa się z trzech projektów:

- *WebAppAsync*: projekt formularzy sieci Web ASP.NET, który korzysta z usługi internetowego interfejsu API **WebAPIpwg** . Większość kodu dla tego samouczka pochodzi z tego projektu.
- *WebAPIpgw*: projekt interfejsu API sieci Web ASP.NET MVC 4, który implementuje kontrolery `Products, Gizmos and Widgets`. Udostępnia dane dla projektu *WebAppAsync* i projektu *Mvc4Async* .
- *Mvc4Async*: projekt ASP.NET MVC 4, który zawiera kod używany w innym samouczku. Powoduje to wywołanie interfejsu API sieci Web do usługi **WebAPIpwg** .

## <a id="GizmosSynch"></a>Strona synchroniczna elementy Gizmo

 Poniższy kod przedstawia metodę synchroniczną `Page_Load`, która jest używana do wyświetlania listy elementy Gizmo. (W tym artykule Gizmo jest fikcyjnym urządzeniem mechanicznym). 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

Poniższy kod przedstawia metodę `GetGizmos` usługi Gizmo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

Metoda `GizmoService GetGizmos` przekazuje identyfikator URI do usługi HTTP Web API ASP.NET, która zwraca listę danych elementy Gizmo. Projekt *WebAPIpgw* zawiera implementację kontrolerów Web API `gizmos, widget` i `product`.  
Na poniższej ilustracji przedstawiono stronę elementy Gizmo z przykładowego projektu.

![Elementy Gizmo](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Tworzenie asynchronicznej strony elementy Gizmo

Przykład używa nowych słów kluczowych [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) i [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) (dostępnych w oprogramowaniu .NET 4,5 i Visual Studio 2012), aby umożliwić kompilatorowi odpowiadanie na zachowanie skomplikowanych transformacji niezbędnych do programowania asynchronicznego. Kompilator pozwala pisać kod przy użyciu C#konstrukcji przepływu sterowania synchronicznego, a kompilator automatycznie stosuje przekształcenia wymagane do użycia wywołań zwrotnych w celu uniknięcia blokowania wątków.

Strony asynchroniczne ASP.NET muszą zawierać dyrektywę [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) z atrybutem `Async` ustawionym na wartość "true". Poniższy kod przedstawia dyrektywę [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) z atrybutem `Async` ustawionym na wartość "true" dla strony *GizmosAsync. aspx* .

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

Poniższy kod przedstawia `Gizmos` metody `Page_Load` synchronicznej i strony asynchronicznej `GizmosAsync`. Jeśli przeglądarka obsługuje [znacznik &lt;HTML 5&gt; element](http://www.w3.org/wiki/HTML/Elements/mark), zobaczysz zmiany w `GizmosAsync`.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

Wersja asynchroniczna:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 Następujące zmiany zostały zastosowane, aby umożliwić asynchronicznej stronie `GizmosAsync`.

- Dyrektywa [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) musi mieć atrybut `Async` ustawiony na wartość "true".
- Metoda `RegisterAsyncTask` służy do rejestrowania asynchronicznego zadania zawierającego kod, który jest uruchamiany asynchronicznie.
- Nowa metoda `GetGizmosSvcAsync` jest oznaczona za pomocą słowa kluczowego [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) , co informuje kompilator, aby generował wywołania zwrotne dla części treści i automatycznie utworzyć `Task`, który jest zwracany.
- &quot;asynchroniczne&quot; dołączone do nazwy metody asynchronicznej. Dołączanie "Async" nie jest wymagane, ale jest konwencją podczas pisania metod asynchronicznych.
- Zwracany typ nowej metody `GetGizmosSvcAsync` jest `Task`. Zwracany typ `Task` reprezentuje bieżącą czynność i zapewnia wywołujących metodę z dojściem, za pomocą którego czeka na zakończenie operacji asynchronicznej.
- Słowo kluczowe [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) zostało zastosowane do wywołania usługi sieci Web.
- Wywołano interfejs API usługi asynchronicznej sieci Web (`GetGizmosAsync`).

Wewnątrz metody `GetGizmosSvcAsync` Metoda inna metoda asynchroniczna, `GetGizmosAsync` jest wywoływana. `GetGizmosAsync` natychmiast zwraca `Task<List<Gizmo>>`, który ostatecznie zakończy się, gdy dane będą dostępne. Ponieważ nie chcesz niczego robić, dopóki nie masz danych Gizmo, kod oczekuje na zadanie (za pomocą słowa kluczowego **await** ). Słowa kluczowego **await** można używać tylko w metodach z adnotacją za pomocą słowa kluczowego **Async** .

Słowo kluczowe **await** nie blokuje wątku do momentu ukończenia zadania. Rejestruje resztę metody jako wywołanie zwrotne zadania i natychmiast zwraca wartość. Gdy oczekujące zadanie zakończy się ostatecznie, wywoła to wywołanie zwrotne i w efekcie wznowi wykonywanie metody w prawidłowym miejscu. Aby uzyskać więcej informacji o używaniu [oczekujących](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [asynchronicznych](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) słów kluczowych i przestrzeni nazw [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) , zobacz [odwołania asynchroniczne](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

W poniższym kodzie przedstawiono metody `GetGizmos` i `GetGizmosAsync`.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 Zmiany asynchroniczne są podobne do tych, które zostały wprowadzone do **GizmosAsync** powyżej. 

- Sygnatura metody została oznaczona przy użyciu słowa kluczowego [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) , a zwracany typ został zmieniony na `Task<List<Gizmo>>`, a *Async* został dołączony do nazwy metody.
- Klasa asynchroniczna [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) jest używana zamiast synchronicznej klasy [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) .
- Słowo kluczowe [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) zostało zastosowane do metody asynchronicznej[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx) [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx).

Na poniższej ilustracji przedstawiono widok Gizmo asynchronicznych.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

Prezentacja przeglądarek danych elementy Gizmo jest taka sama jak widok utworzony przez wywołanie synchroniczne. Jedyną różnicą jest to, że wersja asynchroniczna może być bardziej wydajna w przypadku dużych obciążeń.

## <a name="registerasynctask-notes"></a>RegisterAsyncTask uwagi

Metody podłączane do `RegisterAsyncTask` będą uruchamiane natychmiast po [zdarzeniu PreRender](https://msdn.microsoft.com/library/ms178472.aspx). Można również używać asynchronicznych zdarzeń stron typu void, jak pokazano w poniższym kodzie:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Minusem do asynchronicznych zdarzeń void polega na tym, że deweloperzy nie mają już pełnej kontroli nad wykonywaniem zdarzeń. Na przykład jeśli zarówno. aspx, jak i. Wzorce definiują zdarzenia `Page_Load` i jedno lub oba z nich są asynchroniczne, nie można zagwarantować kolejności wykonywania. Ta sama kolejność indeterminiate dla programów obsługi niezdarzeń (takich jak `async void Button_Click`) ma zastosowanie. Dla większości deweloperów powinna być to akceptowalne, ale te, które wymagają pełnej kontroli nad kolejnością wykonywania, powinny używać tylko interfejsów API, takich jak `RegisterAsyncTask`, które używają metod, które zwracają obiekt zadania.

## <a id="Parallel"></a>Wykonywanie wielu operacji równolegle

Metody asynchroniczne mają znaczną korzyść w porównaniu z metodami synchronicznymi, gdy akcja musi wykonywać kilka niezależnych operacji. W podanym przykładzie Strona synchroniczna *PWG. aspx*(dla produktów, widżetów i elementy Gizmo) zawiera wyniki trzech wywołań usługi sieci Web w celu uzyskania listy produktów, widżetów i elementy Gizmo. Projekt [interfejsu API sieci Web ASP.NET](../../../web-api/index.md) , który udostępnia te usługi, używa [zadania. opóźnienie](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) do symulowania opóźnień lub wolnych połączeń sieciowych. Gdy opóźnienie jest ustawione na 500 milisekund, asynchroniczna strona *PWGasync. aspx* zajmie nieco ponad 500 milisekund, gdy synchroniczna wersja `PWG` przekroczy 1 500 milisekund. Synchroniczna strona *PWG. aspx* jest pokazana w poniższym kodzie.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

Poniżej przedstawiono kod asynchronicznej `PWGasync`.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

Na poniższej ilustracji przedstawiono widok zwrócony z asynchronicznej strony *PWGasync. aspx* .

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>Korzystanie z tokenu anulowania

Metody asynchroniczne zwracające `Task`są anulowane, które przyjmują parametr [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) , gdy jeden z nich jest dostarczany z atrybutem `AsyncTimeout` dyrektywy [Page](https://msdn.microsoft.com/library/ydy4x04a.aspx) . Poniższy kod przedstawia stronę *GizmosCancelAsync. aspx* z limitem czasu na sekundę.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

Poniższy kod przedstawia plik *GizmosCancelAsync.aspx.cs* .

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

W podanej przykładowej aplikacji wybranie linku *GizmosCancelAsync* wywołuje stronę *GizmosCancelAsync. aspx* i pokazuje anulowanie (przez przekroczenie limitu czasu) wywołania asynchronicznego. Ponieważ czas opóźnienia mieści się w zakresie losowym, może być konieczne odświeżenie strony kilka razy, aby wyświetlić komunikat o błędzie limitu czasu.

## <a id="ServerConfig"></a>Konfiguracja serwera dla wywołań usługi sieci Web o wysokim poziomie współbieżności/dużych opóźnień

Aby zrealizować zalety asynchronicznej aplikacji sieci Web, może być konieczne wprowadzenie pewnych zmian w domyślnej konfiguracji serwera. Podczas konfigurowania i obciążeniowania asynchronicznej aplikacji sieci Web należy pamiętać o następujących kwestiach.

- System Windows 7, Windows Vista, Window 8 i wszystkie klienckie systemy operacyjne Windows mają maksymalnie 10 współbieżnych żądań. Musisz mieć system operacyjny Windows Server, aby zobaczyć zalety metod asynchronicznych w ramach dużego obciążenia.
- Zarejestruj platformę .NET 4,5 z usługami IIS z poziomu wiersza polecenia z podwyższonym poziomem uprawnień przy użyciu następującego polecenia:  
  %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis-i  
  Zobacz [ASP.NET narzędzia rejestracji usług IIS (Aspnet\_regiis. exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Może być konieczne zwiększenie limitu kolejki [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) z wartości domyślnej 1 000 do 5 000. Jeśli ustawienie jest zbyt niskie, mogą zostać wyświetlone żądania [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) odrzucone ze stanem HTTP 503. Aby zmienić limit kolejki HTTP. sys:

    - Otwórz Menedżera usług IIS i przejdź do okienka pule aplikacji.
    - Kliknij prawym przyciskiem myszy docelową pulę aplikacji i wybierz pozycję **Ustawienia zaawansowane**.  
        ![Advanced](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - W oknie dialogowym **Ustawienia zaawansowane** Zmień *długość kolejki* z 1 000 na 5 000.  
        Długość kolejki ![](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
  Zanotuj powyższe obrazy, a program .NET Framework jest wymieniony jako v 4.0, nawet jeśli Pula aplikacji korzysta z platformy .NET 4,5. Aby zrozumieć ten rozbieżność, zobacz następujące tematy:

- [Wersja .NET i wiele elementów docelowych — .NET 4,5 to uaktualnienie w miejscu do programu .NET 4,0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
- [Jak ustawić aplikację IIS lub puli aplikacji do korzystania z ASP.NET 3,5 zamiast 2,0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
- [Wersje i zależności platformy .NET Framework](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)

- Jeśli aplikacja korzysta z usług sieci Web lub System.NET do komunikowania się z zapleczem za pośrednictwem protokołu HTTP, może być konieczne zwiększenie elementu [connectionManagement/MaxConnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) . W przypadku aplikacji ASP.NET jest to ograniczone przez funkcję autokonfiguracji do 12 razy liczba procesorów. Oznacza to, że w ramach procesora można maksymalnie 12 \* 4 = 48 połączeń współbieżnych do punktu końcowego IP. Ze względu na to, że jest to związane z [Autokonfiguracjami](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), najprostszym sposobem zwiększenia `maxconnection` w aplikacji ASP.NET jest ustawienie `Application_Start` w pliku *Global. asax* [systemu .NET. ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) programowo. Zapoznaj się z przykładowym pobraniem.
- W programie .NET 4,5 wartość domyślna 5000 dla [maxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) powinna być dobra.

## <a name="contributors"></a>Współautorzy

- [Broderick opłat](http://stackoverflow.com/users/59641/levi)
- [Dykstra Tomasz](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei GE](https://blogs.msdn.com/b/hongmeig/)

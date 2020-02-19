---
uid: mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
title: Korzystanie z metod asynchronicznych w ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku przedstawiono podstawowe informacje dotyczące tworzenia asynchronicznej aplikacji sieci Web ASP.NET w warstwie MVC przy użyciu Visual Studio Express 2012 dla sieci Web, która jest bezpłatnym...
ms.author: riande
ms.date: 06/06/2012
ms.assetid: a56572ba-81c3-47af-826d-941e9c4775ec
msc.legacyurl: /mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 15692b18fc112c4c6cce4d50a243a0e8d5fb52a4
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457755"
---
# <a name="using-asynchronous-methods-in-aspnet-mvc-4"></a>Używanie metod asynchronicznych na platformie ASP.NET MVC 4

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

> W tym samouczku przedstawiono podstawowe informacje dotyczące tworzenia asynchronicznej aplikacji sieci Web ASP.NET w wersji MVC przy użyciu [Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/11), która jest bezpłatną wersją Microsoft Visual Studio. Możesz również użyć [programu Visual Studio 2012](https://www.microsoft.com/visualstudio/11).
> 
> Pełny przykład jest dostępny dla tego samouczka w witrynie GitHub [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/)

Klasa [kontrolera](https://msdn.microsoft.com/library/system.web.mvc.controller(VS.108).aspx) MVC 4 ASP.NET w połączeniu z [platformą .NET 4,5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) umożliwia pisanie asynchronicznych metod akcji, które zwracają obiekt typu [zadanie&lt;ActionResult&gt;](https://msdn.microsoft.com/library/dd321424(VS.110).aspx). W .NET Framework 4 wprowadzono asynchroniczną koncepcję programistyczną, o której mowa jako [zadanie](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) i ASP.NET MVC 4 obsługuje [zadanie](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Zadania są reprezentowane przez typ **zadania** i powiązane typy w przestrzeni nazw [System. Threading. Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) . .NET Framework 4,5 jest oparta na tej asynchronicznej obsłudze z słowami kluczowymi [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) , które współpracują z obiektami [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) znacznie mniej skomplikowane niż poprzednie podejście asynchroniczne. Słowo kluczowe [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) jest skrótową składnią wskazującą, że fragment kodu powinien asynchronicznie oczekiwać na inne fragmenty kodu. Słowo kluczowe [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) reprezentuje wskazówkę, której można użyć do oznaczenia metod jako metod asynchronicznych opartych na zadaniach. Połączenie obiektów **await**, **Async**i **Task** ułatwia pisanie kodu asynchronicznego w programie .NET 4,5. Nowy model metod asynchronicznych jest nazywany *wzorcem asynchronicznym opartym na zadaniach* (**TAP**). W tym samouczku założono, że masz pewną wiedzę z funkcją asynchronicznego programowania przy użyciu słów kluczowych [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) oraz przestrzeni nazw [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) .

Aby uzyskać więcej informacji na temat używania [oczekujących](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [asynchronicznych](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) słów kluczowych oraz przestrzeni nazw [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) , zobacz następujące odwołania.

- [Oficjalny dokument: asynchroniczności na platformie .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Asynchroniczne/await — często zadawane pytania](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Programowanie asynchroniczne programu Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Jak żądania są przetwarzane przez pulę wątków

Na serwerze sieci Web .NET Framework obsługuje pulę wątków, które są używane do obsługi żądań ASP.NET. Po nadejściu żądania wątek z puli jest wysyłany w celu przetworzenia tego żądania. Jeśli żądanie jest przetwarzane synchronicznie, wątek, który przetwarza żądanie, jest zajęty podczas przetwarzania żądania, a ten wątek nie może obsłużyć innego żądania.   
  
Może to nie być problem, ponieważ pula wątków może być zbyt duża, aby pomieścić wiele zajętych wątków. Liczba wątków w puli wątków jest jednak ograniczona (domyślna wartość maksymalna dla programu .NET 4,5 to 5 000). W dużych aplikacjach z wysokim poziomem współbieżności długotrwałych żądań wszystkie dostępne wątki mogą być zajęte. Ten stan jest określany jako zablokowanie wątku. Po osiągnięciu tego warunku serwer sieci Web będzie kolejkować żądania. Jeśli kolejka żądań zostanie zapełniona, serwer sieci Web odrzuca żądania ze stanem HTTP 503 (serwer jest zbyt zajęty). Pula wątków CLR ma ograniczenia dotyczące nowych iniekcji wątków. Jeśli współbieżność odbywa się na rozerwanie (oznacza to, że witryna sieci Web może nagle uzyskać dużą liczbę żądań), a wszystkie dostępne wątki żądań są zajęte ze względu na wywołania zaplecza z dużym opóźnieniem, ograniczenie liczby iniekcji wątków może sprawiać, że aplikacja reaguje bardzo źle. Ponadto każdy nowy wątek dodany do puli wątków ma narzuty (np. 1 MB pamięci stosu). Aplikacja internetowa korzystająca z metod synchronicznych do obsługi połączeń o dużym opóźnieniu, gdy pula wątków rośnie do wartości domyślnej programu .NET 4,5 w liczbie 5 000 wątków zużywa około 5 GB więcej pamięci niż aplikacja może korzystać z tych samych żądań metody asynchroniczne i tylko 50 wątków. Gdy wykonujesz zadania asynchroniczne, nie zawsze używasz wątku. Na przykład po wprowadzeniu asynchronicznego żądania usługi sieci Web ASP.NET nie będzie używać żadnych wątków między wywołaniem metody **asynchronicznej** a **oczekującą**. Użycie puli wątków do obsługi żądań o dużym opóźnieniu może prowadzić do znacznego wykorzystania pamięci i niedostatecznego użycia sprzętu serwera.

## <a name="processing-asynchronous-requests"></a>Przetwarzanie żądań asynchronicznych

W aplikacji sieci Web, która widzi dużą liczbę równoczesnych żądań w momencie uruchomienia lub ma obciążenie pierścieniowe (w przypadku gwałtownego wydłużenia współbieżności), dzięki czemu wywołania usługi sieci Web asynchroniczne zwiększają czas odpowiedzi aplikacji. Żądania asynchroniczne zajmują ten sam czas do przetworzenia w ramach żądania synchronicznego. Jeśli żądanie wykonuje wywołanie usługi sieci Web, które wymaga wykonania dwóch sekund, żądanie trwa dwa sekundy niezależnie od tego, czy jest wykonywane synchronicznie, czy asynchronicznie. Jednak podczas wywołania asynchronicznego wątek nie może odpowiadać na inne żądania podczas oczekiwania na ukończenie pierwszego żądania. W związku z tym żądania asynchroniczne uniemożliwiają zwiększenie liczby kolejkowania żądań i puli wątków, gdy istnieje wiele współbieżnych żądań, które wywołują długotrwałe operacje.

## <a id="ChoosingSyncVasync"></a>Wybieranie metod akcji synchronicznych lub asynchronicznych

W tej sekcji przedstawiono wskazówki dotyczące stosowania synchronicznych lub asynchronicznych metod akcji. Są to jedynie wskazówki. osobno sprawdzaj poszczególne aplikacje, aby określić, czy metody asynchroniczne pomagają w wydajności.

Ogólnie rzecz biorąc, należy użyć metod synchronicznych dla następujących warunków:

- Operacje są proste lub krótkoterminowe.
- Prostota jest bardziej ważna niż wydajność.
- Operacje są wykonywane głównie przez procesor CPU zamiast operacji, które obejmują rozbudowane obciążenie dysku lub sieci. Używanie asynchronicznych metod akcji w operacjach powiązanych z PROCESORem nie zapewnia żadnych korzyści i daje większe obciążenie.

Ogólnie rzecz biorąc, należy używać metod asynchronicznych dla następujących warunków:

- Dzwonisz do usług, które mogą być używane za pośrednictwem metod asynchronicznych, i używasz platformy .NET w wersji 4,5 lub nowszej.
- Operacje są powiązane z siecią lub we/wy, a nie związane z PROCESORem.
- Równoległość jest ważniejsze niż prostota kodu.
- Chcesz zapewnić mechanizm, który umożliwia użytkownikom anulowanie długotrwałego żądania.
- Gdy korzyść przełączenia wątków powoduje obniżenie kosztów przełączenia kontekstu. Ogólnie rzecz biorąc, należy zmienić metodę asynchroniczną, jeśli metoda synchroniczna czeka na wątek żądania ASP.NET podczas wykonywania zadań. Wykonując asynchroniczne wywołanie, wątek żądania ASP.NET nie jest wstrzymany podczas oczekiwania na ukończenie żądania usługi sieci Web.
- Testy pokazują, że operacje blokowania to wąskie gardła w wydajności lokacji i że usługi IIS mogą obsłużyć więcej żądań, używając metod asynchronicznych dla tych wywołań blokowania.

Przykład do pobrania pokazuje, jak skutecznie używać metod akcji asynchronicznych. Dostarczony przykład został zaprojektowany w celu zapewnienia prostej demonstracji programowania asynchronicznego w ASP.NET MVC 4 przy użyciu platformy .NET 4,5. Przykład nie jest przeznaczony do architektury referencyjnej dla programowania asynchronicznego w ASP.NET MVC. Przykładowy program wywołuje metody internetowego [interfejsu API ASP.NET](../../../web-api/index.md) , które z kolei powodują [zadanie Call. Opóźnij](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) , aby symulować długotrwałe wywołania usługi sieci Web. Większość aplikacji produkcyjnych nie będzie pokazywała oczywistych korzyści związanych z korzystaniem z metod akcji asynchronicznych.   
  
Niektóre aplikacje wymagają, aby wszystkie metody akcji były asynchroniczne. Często konwersja kilku synchronicznych metod akcji na metody asynchroniczne zapewnia optymalny wzrost wydajności dla wymaganej ilości pracy.

## <a id="SampleApp"></a>Przykładowa aplikacja

Przykładową aplikację można pobrać z [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET) w witrynie [GitHub](https://github.com/) . Repozytorium składa się z trzech projektów:

- *Mvc4Async*: projekt ASP.NET MVC 4, który zawiera kod używany w tym samouczku. Powoduje to wywołanie interfejsu API sieci Web do usługi **WebAPIpgw** .
- *WebAPIpgw*: projekt interfejsu API sieci Web ASP.NET MVC 4, który implementuje kontrolery `Products, Gizmos and Widgets`. Udostępnia dane dla projektu *WebAppAsync* i projektu *Mvc4Async* .
- *WebAppAsync*: projekt formularzy sieci Web ASP.NET używany w innym samouczku.

## <a id="GizmosSynch"></a>Metoda synchronicznej akcji elementy Gizmo

 Poniższy kod przedstawia metodę synchronicznej akcji `Gizmos`, która jest używana do wyświetlania listy elementy Gizmo. (W tym artykule Gizmo jest fikcyjnym urządzeniem mechanicznym). 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample1.cs)]

Poniższy kod przedstawia metodę `GetGizmos` usługi Gizmo.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample2.cs)]

Metoda `GizmoService GetGizmos` przekazuje identyfikator URI do usługi HTTP Web API ASP.NET, która zwraca listę danych elementy Gizmo. Projekt *WebAPIpgw* zawiera implementację kontrolerów Web API `gizmos, widget` i `product`.  
Na poniższej ilustracji przedstawiono widok elementy Gizmo z przykładowego projektu.

![Elementy Gizmo](using-asynchronous-methods-in-aspnet-mvc-4/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Tworzenie asynchronicznej metody akcji elementy Gizmo

Przykład używa nowych słów kluczowych [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) i [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) (dostępnych w oprogramowaniu .NET 4,5 i Visual Studio 2012), aby umożliwić kompilatorowi odpowiadanie na zachowanie skomplikowanych transformacji niezbędnych do programowania asynchronicznego. Kompilator pozwala pisać kod przy użyciu C#konstrukcji przepływu sterowania synchronicznego, a kompilator automatycznie stosuje przekształcenia wymagane do użycia wywołań zwrotnych w celu uniknięcia blokowania wątków.

Poniższy kod przedstawia metodę synchroniczną `Gizmos` i metodę `GizmosAsync` asynchronicznej. Jeśli przeglądarka obsługuje [element HTML 5 `<mark>`](http://www.w3.org/wiki/HTML/Elements/mark), zmiany są widoczne w `GizmosAsync` na żółto.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample3.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample4.cs?highlight=1,3,5)]

 Następujące zmiany zostały zastosowane, aby zezwalać na asynchroniczne `GizmosAsync`.

- Metoda jest oznaczona za pomocą słowa kluczowego [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) , co informuje kompilator, aby wygenerował wywołania zwrotne dla części treści i automatycznie utworzyć `Task<ActionResult>`, który jest zwracany.
- &quot;asynchroniczne&quot; dołączone do nazwy metody. Dołączanie "Async" nie jest wymagane, ale jest konwencją podczas pisania metod asynchronicznych.
- Typ zwracany został zmieniony z `ActionResult` na `Task<ActionResult>`. Zwracany typ `Task<ActionResult>` reprezentuje bieżącą czynność i zapewnia wywołujących metodę z dojściem, za pomocą którego czeka na zakończenie operacji asynchronicznej. W takim przypadku obiekt wywołujący jest usługą sieci Web. `Task<ActionResult>` reprezentuje bieżącą współpracę z wynikiem `ActionResult.`
- Słowo kluczowe [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) zostało zastosowane do wywołania usługi sieci Web.
- Wywołano interfejs API usługi asynchronicznej sieci Web (`GetGizmosAsync`).

Wewnątrz metody `GetGizmosAsync` Metoda inna metoda asynchroniczna, `GetGizmosAsync` jest wywoływana. `GetGizmosAsync` natychmiast zwraca `Task<List<Gizmo>>`, który ostatecznie zakończy się, gdy dane będą dostępne. Ponieważ nie chcesz niczego robić, dopóki nie masz danych Gizmo, kod oczekuje na zadanie (za pomocą słowa kluczowego **await** ). Słowa kluczowego **await** można używać tylko w metodach z adnotacją za pomocą słowa kluczowego **Async** .

Słowo kluczowe **await** nie blokuje wątku do momentu ukończenia zadania. Rejestruje resztę metody jako wywołanie zwrotne zadania i natychmiast zwraca wartość. Gdy oczekujące zadanie zakończy się ostatecznie, wywoła to wywołanie zwrotne i w efekcie wznowi wykonywanie metody w prawidłowym miejscu. Aby uzyskać więcej informacji o używaniu [oczekujących](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) i [asynchronicznych](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) słów kluczowych i przestrzeni nazw [zadań](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) , zobacz [odwołania asynchroniczne](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

W poniższym kodzie przedstawiono metody `GetGizmos` i `GetGizmosAsync`.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample5.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample6.cs?highlight=1,4-8)]

 Zmiany asynchroniczne są podobne do tych, które zostały wprowadzone do **GizmosAsync** powyżej. 

- Sygnatura metody została oznaczona przy użyciu słowa kluczowego [Async](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) , a zwracany typ został zmieniony na `Task<List<Gizmo>>`, a *Async* został dołączony do nazwy metody.
- Klasa asynchroniczna [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) jest używana zamiast klasy [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) .
- Słowo kluczowe [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) zostało zastosowane do metod asynchronicznych [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) .

Na poniższej ilustracji przedstawiono widok Gizmo asynchronicznych.

![async](using-asynchronous-methods-in-aspnet-mvc-4/_static/image2.png)

Prezentacja przeglądarek danych elementy Gizmo jest taka sama jak widok utworzony przez wywołanie synchroniczne. Jedyną różnicą jest to, że wersja asynchroniczna może być bardziej wydajna w przypadku dużych obciążeń.

## <a id="Parallel"></a>Wykonywanie wielu operacji równolegle

Metody akcji asynchronicznych mają znaczną korzyść w porównaniu z metodami synchronicznymi, gdy akcja musi wykonywać kilka niezależnych operacji. W podanym przykładzie metoda synchroniczna `PWG`(dla produktów, widżetów i elementy Gizmo) wyświetla wyniki trzech wywołań usługi sieci Web w celu uzyskania listy produktów, widżetów i elementy Gizmo. Projekt [interfejsu API sieci Web ASP.NET](../../../web-api/index.md) , który udostępnia te usługi, używa [zadania. opóźnienie](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) do symulowania opóźnień lub wolnych połączeń sieciowych. Gdy opóźnienie jest ustawione na 500 milisekund, asynchroniczna Metoda `PWGasync` trwa nieco dłużej niż 500 milisekund, gdy synchroniczna wersja `PWG` trwa ponad 1 500 milisekund. Synchroniczna Metoda `PWG` jest pokazana w poniższym kodzie.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample7.cs)]

Metoda `PWGasync` asynchronicznej jest pokazana w poniższym kodzie.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample8.cs?highlight=1,3,12)]

Na poniższej ilustracji przedstawiono widok zwrócony z metody **PWGasync** .

![pwgAsync](using-asynchronous-methods-in-aspnet-mvc-4/_static/image3.png)

## <a id="CancelToken"></a>Korzystanie z tokenu anulowania

Metody akcji asynchronicznych zwracające `Task<ActionResult>`są anulowane, które przyjmują parametr [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) , gdy jeden z nich jest dostarczany z atrybutem [AsyncTimeout](https://msdn.microsoft.com/library/system.web.mvc.asynctimeoutattribute(VS.108).aspx) . Poniższy kod przedstawia metodę `GizmosCancelAsync` z limitem czasu 150 milisekund.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample9.cs?highlight=1-3,5,10)]

Poniższy kod przedstawia Przeciążenie GetGizmosAsync, które przyjmuje parametr [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) .

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample10.cs)]

W podanej przykładowej aplikacji wybranie linku *demonstracyjnego tokenu anulowania* wywołuje metodę `GizmosCancelAsync` i demonstruje anulowanie wywołania asynchronicznego.

## <a id="ServerConfig"></a>Konfiguracja serwera dla wywołań usługi sieci Web o wysokim poziomie współbieżności/dużych opóźnień

Aby zrealizować zalety asynchronicznej aplikacji sieci Web, może być konieczne wprowadzenie pewnych zmian w domyślnej konfiguracji serwera. Podczas konfigurowania i obciążeniowania asynchronicznej aplikacji sieci Web należy pamiętać o następujących kwestiach.

- System Windows 7, Windows Vista i wszystkie systemy operacyjne klienta Windows mają maksymalnie 10 współbieżnych żądań. Musisz mieć system operacyjny Windows Server, aby zobaczyć zalety metod asynchronicznych w ramach dużego obciążenia.
- Zarejestruj platformę .NET 4,5 z usługami IIS z poziomu wiersza polecenia z podwyższonym poziomem uprawnień:  
  %windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_regiis-i  
  Zobacz [ASP.NET narzędzia rejestracji usług IIS (Aspnet\_regiis. exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Może być konieczne zwiększenie limitu kolejki [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) z wartości domyślnej 1 000 do 5 000. Jeśli ustawienie jest zbyt niskie, mogą zostać wyświetlone żądania [http. sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) odrzucone ze stanem HTTP 503. Aby zmienić limit kolejki HTTP. sys:

    - Otwórz Menedżera usług IIS i przejdź do okienka pule aplikacji.
    - Kliknij prawym przyciskiem myszy docelową pulę aplikacji i wybierz pozycję **Ustawienia zaawansowane**.  
        ![Advanced](using-asynchronous-methods-in-aspnet-mvc-4/_static/image4.png)
    - W oknie dialogowym **Ustawienia zaawansowane** Zmień *długość kolejki* z 1 000 na 5 000.  
        Długość kolejki ![](using-asynchronous-methods-in-aspnet-mvc-4/_static/image5.png)  
  
  Zanotuj powyższe obrazy, a program .NET Framework jest wymieniony jako v 4.0, nawet jeśli Pula aplikacji korzysta z platformy .NET 4,5. Aby zrozumieć ten rozbieżność, zobacz następujące tematy:

    - [Wersja .NET i wiele elementów docelowych — .NET 4,5 to uaktualnienie w miejscu do programu .NET 4,0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
    - [Jak ustawić aplikację IIS lub puli aplikacji do korzystania z ASP.NET 3,5 zamiast 2,0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
    - [Wersje i zależności platformy .NET Framework](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Jeśli aplikacja korzysta z usług sieci Web lub System.NET do komunikowania się z zapleczem za pośrednictwem protokołu HTTP, może być konieczne zwiększenie elementu [connectionManagement/MaxConnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) . W przypadku aplikacji ASP.NET jest to ograniczone przez funkcję autokonfiguracji do 12 razy liczba procesorów. Oznacza to, że w ramach procesora można maksymalnie 12 \* 4 = 48 połączeń współbieżnych do punktu końcowego IP. Ze względu na to, że jest to związane z [Autokonfiguracjami](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), najprostszym sposobem zwiększenia `maxconnection` w aplikacji ASP.NET jest ustawienie `Application_Start` w pliku *Global. asax* [systemu .NET. ServicePointManager. DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) programowo. Zapoznaj się z przykładowym pobraniem.
- W programie .NET 4,5 wartość domyślna 5000 dla [maxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) powinna być dobra.

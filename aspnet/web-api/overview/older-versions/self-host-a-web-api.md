---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Samohostowanie interfejsu API sieci Web ASP.NETC#1 () — ASP.NET 4. x
author: MikeWasson
description: Samouczek z kodem pokazuje, jak hostować interfejs API sieci Web w aplikacji konsolowej.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: bae1737ba5b16bc67fa0ed0474ff04df0add1b3a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525089"
---
# <a name="self-host-aspnet-web-api-1-c"></a>Samoobsługowy interfejs API sieci Web ASP.NETC#1 ()

według [Jan Wasson](https://github.com/MikeWasson)

> W tym samouczku pokazano, jak hostować interfejs API sieci Web w aplikacji konsolowej. Interfejs API sieci Web ASP.NET nie wymaga usług IIS. Możesz samodzielnie hostować internetowy interfejs API w procesie hosta. 
> 
> **Nowe aplikacje powinny używać OWIN do samoobsługowego interfejsu API sieci Web.** Zobacz [Używanie Owin do samoobsługowego hostowania ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - Interfejs API sieci Web 1
> - Visual Studio 2012

## <a name="create-the-console-application-project"></a>Tworzenie projektu aplikacji konsolowej

Uruchom program Visual Studio i wybierz pozycję **Nowy projekt** na stronie **startowej** . Lub w menu **plik** wybierz polecenie **Nowy** , a następnie pozycję **projekt**.

W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  . W **obszarze C#Wizualizacja** wybierz pozycję **Windows**. Na liście szablonów projektu wybierz pozycję **Aplikacja konsolowa**. Nazwij projekt &quot;SelfHost&quot; a następnie kliknij przycisk **OK**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Ustawianie platformy docelowej (Visual Studio 2010)

W przypadku korzystania z programu Visual Studio 2010 Zmień platformę docelową na .NET Framework 4,0. (Domyślnie szablon projektu jest przeznaczony dla [profilu klienta .NET Framework](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile)).

W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Właściwości**. Na liście rozwijanej **platforma docelowa** Zmień platformę docelową na .NET Framework 4,0. Po wyświetleniu monitu o zastosowanie zmiany kliknij przycisk **tak**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Zainstaluj Menedżera pakietów NuGet

Menedżer pakietów NuGet to najprostszy sposób, aby dodać zestawy interfejsów API sieci Web do projektu non-ASP.NET.

Aby sprawdzić, czy zainstalowano Menedżera pakietów NuGet, kliknij menu **Narzędzia** w programie Visual Studio. Jeśli zobaczysz element menu o nazwie **Menedżer pakietów NuGet**, będziesz mieć Menedżera pakietów NuGet.

Aby zainstalować Menedżera pakietów NuGet:

1. Uruchom program Visual Studio.
2. W menu **Narzędzia** wybierz pozycję **rozszerzenia i aktualizacje**.
3. W oknie dialogowym **rozszerzenia i aktualizacje** wybierz pozycję **online**.
4. Jeśli nie widzisz "Menedżera pakietów NuGet", wpisz "Menedżer pakietów NuGet" w polu wyszukiwania.
5. Wybierz pozycję Menedżer pakietów NuGet, a następnie kliknij pozycję **Pobierz**.
6. Po zakończeniu pobierania zostanie wyświetlony monit o zainstalowanie programu.
7. Po zakończeniu instalacji może zostać wyświetlony monit o ponowne uruchomienie programu Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Dodawanie pakietu NuGet interfejsu API sieci Web

Po zainstalowaniu Menedżera pakietów NuGet Dodaj do projektu pakiet samoobsługowy interfejsu API sieci Web.

1. W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**. *Uwaga*: Jeśli nie widzisz tego elementu menu, upewnij się, że Menedżer pakietów NuGet został prawidłowo zainstalowany.
2. Wybierz pozycję **Zarządzaj pakietami NuGet dla rozwiązania**
3. W oknie dialogowym **Zarządzanie pakietami część** wybierz pozycję **online**.
4. W polu wyszukiwania wpisz &quot;Microsoft. AspNet. WebApi. SelfHost&quot;.
5. Wybierz pakiet internetowego interfejsu API ASP.NET, a następnie kliknij przycisk **Instaluj**.
6. Po zainstalowaniu pakietu kliknij przycisk **Zamknij** , aby zamknąć okno dialogowe.

> [!NOTE]
> Upewnij się, że zainstalowano pakiet o nazwie Microsoft. AspNet. WebApi. SelfHost, a nie AspNetWebApi. SelfHost.

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Tworzenie modelu i kontrolera

Ten samouczek używa tych samych klas modelu i kontrolera co samouczek [wprowadzenie](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) .

Dodaj klasę publiczną o nazwie `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Dodaj klasę publiczną o nazwie `ProductsController`. Utwórz tę klasę z klasy **System. Web. http. ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Aby uzyskać więcej informacji na temat kodu w tym kontrolerze, zobacz samouczek [wprowadzenie](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) . Ten kontroler definiuje trzy akcje GET:

| Identyfikator URI | Opis |
| --- | --- |
| /api/products | Pobierz listę wszystkich produktów. |
| *Identyfikator* /API/Products/ | Pobierz produkt według identyfikatora. |
| /API/Products/? Category =*Kategoria* | Pobierz listę produktów według kategorii. |

## <a name="host-the-web-api"></a>Hostowanie internetowego interfejsu API

Otwórz plik Program.cs i Dodaj następujące instrukcje using:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Dodaj następujący kod do klasy **program** .

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>Obowiązkowe Dodawanie rezerwacji przestrzeni nazw adresu URL HTTP

Ta aplikacja nasłuchuje `http://localhost:8080/`. Domyślnie nasłuchiwanie na określonym adresie HTTP wymaga uprawnień administratora. W związku z tym ten błąd może zostać wyświetlony podczas uruchamiania samouczka: "protokół HTTP nie może zarejestrować adresu URL http://+:8080/" Istnieją dwa sposoby uniknięcia tego błędu:

- Uruchom program Visual Studio z podniesionymi uprawnieniami administratora lub
- Użyj narzędzia Netsh. exe, aby nadać kontu uprawnienia do rezerwowania adresu URL.

Aby użyć narzędzia Netsh. exe, Otwórz wiersz polecenia z uprawnieniami administratora i wprowadź następujące polecenie: następujące polecenie:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

gdzie *machine\username* jest kontem użytkownika.

Po zakończeniu samoobsługowego udostępniania Pamiętaj o usunięciu rezerwacji:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Wywoływanie interfejsu API sieci Web z aplikacji klienckiej (C#)

Napiszmy prostą aplikację konsolową, która wywołuje internetowy interfejs API.

Dodaj nowy projekt aplikacji konsolowej do rozwiązania:

- W Eksplorator rozwiązań kliknij prawym przyciskiem myszy rozwiązanie i wybierz polecenie **Dodaj nowy projekt**.
- Utwórz nową aplikację konsolową o nazwie &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Użyj Menedżera pakietów NuGet, aby dodać pakiet podstawowych bibliotek interfejsu API sieci Web ASP.NET:

- W menu Narzędzia wybierz pozycję **Menedżer pakietów NuGet**.
- Wybierz pozycję **Zarządzaj pakietami NuGet dla rozwiązania**
- W oknie dialogowym **Zarządzanie pakietami NuGet** wybierz pozycję **online**.
- W polu wyszukiwania wpisz &quot;Microsoft. AspNet. WebApi. Client&quot;.
- Wybierz pakiet biblioteki klienta interfejsu Web API Microsoft ASP.NET i kliknij przycisk **Instaluj**.

Dodaj odwołanie w ClientApp do projektu SelfHost:

- W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt ClientApp.
- Wybierz pozycję **Dodaj odwołanie**.
- W oknie dialogowym **Menedżer odwołań** w obszarze **rozwiązanie**wybierz pozycję **projekty**.
- Wybierz projekt SelfHost.
- Kliknij przycisk **OK**.

![](self-host-a-web-api/_static/image6.png)

Otwórz plik Client/program. cs. Dodaj następującą instrukcję **using** :

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Dodaj statyczne wystąpienie **HttpClient** :

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Dodaj następujące metody, aby wyświetlić listę wszystkich produktów, wyświetlić listę produktów według identyfikatora oraz listę produktów według kategorii.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Każda z tych metod jest zgodna z tym samym wzorcem:

1. Wywołaj **HttpClient. GetAsync** , aby wysłać żądanie Get do odpowiedniego identyfikatora URI.
2. Wywołaj metodę **HttpResponseMessage. EnsureSuccessStatusCode**. Ta metoda zgłasza wyjątek, jeśli stan odpowiedzi HTTP to kod błędu.
3. Wywołaj **ReadAsAsync&lt;t&gt;** , aby deserializować typ CLR z odpowiedzi HTTP. Ta metoda jest metodą rozszerzającą zdefiniowaną w **systemie .NET. http. HttpContentExtensions**.

Metody **GetAsync** i **ReadAsAsync** są asynchroniczne. Zwracają one obiekty **zadań** , które reprezentują operację asynchroniczną. Pobieranie właściwości **wynik** blokuje wątek do momentu zakończenia operacji.

Aby uzyskać więcej informacji o korzystaniu z programu HttpClient, w tym o sposobie wykonywania wywołań nieblokujących, zobacz [wywoływanie interfejsu API sieci Web z klienta platformy .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Przed wywołaniem tych metod ustaw właściwość BaseAddress w wystąpieniu HttpClient na wartość "`http://localhost:8080`". Na przykład:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

Powinno to spowodować wyjście z poniższych danych. (Pamiętaj, aby najpierw uruchomić aplikację SelfHost).

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)

---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
title: Korzystanie z ASP.NET MVC z różnymi wersjami usługC#IIS () | Microsoft Docs
author: microsoft
description: W ramach tego samouczka nauczysz się używać ASP.NET MVC i routingu URL z różnymi wersjami Internet Information Services. Poznaj różne strategie...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: b0cf4a34-2c1d-4717-bb54-ff029e722990
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
msc.type: authoredcontent
ms.openlocfilehash: 3b0a9509c0600f3598fd1218a7b383430548d4c0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601102"
---
# <a name="using-aspnet-mvc-with-different-versions-of-iis-c"></a>Używanie wzorca ASP.NET MVC z różnymi wersjami usług IIS (C#)

przez [firmę Microsoft](https://github.com/microsoft)

> W ramach tego samouczka nauczysz się używać ASP.NET MVC i routingu URL z różnymi wersjami Internet Information Services. Poznaj różne strategie używania ASP.NET MVC z usługami IIS 7,0 (Tryb klasyczny), IIS 6,0 i wcześniejszymi wersjami usług IIS.

Struktura ASP.NET MVC zależy od routingu ASP.NET do kierowania żądań przeglądarki do akcji kontrolera. Aby móc korzystać z usługi ASP.NET routing, może być konieczne wykonanie dodatkowych czynności konfiguracyjnych na serwerze sieci Web. Wszystkie te zależności są zależne od wersji programu Internet Information Services (IIS) i trybu przetwarzania żądań aplikacji.

Oto podsumowanie różnych wersji usług IIS:

- IIS 7,0 (tryb zintegrowany) — nie jest wymagana specjalna konfiguracja do używania routingu ASP.NET.
- IIS 7,0 (Tryb klasyczny) — należy wykonać specjalną konfigurację, aby użyć routingu ASP.NET.
- IIS 6,0 lub niższym — musisz wykonać specjalną konfigurację, aby użyć routingu ASP.NET.

Najnowsza wersja usług IIS jest w wersji 7,5 (w witrynie Win7). USŁUGI IIS 7 usług IIS są dołączone do systemów Windows Server 2008 i VISTA/SP1 i nowszych. Można również zainstalować usługi IIS 7,0 w dowolnej wersji systemu operacyjnego Vista, z wyjątkiem narzędzia Home Basic (zobacz [https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

Usługi IIS 7,0 obsługują dwa tryby przetwarzania żądań. Można użyć trybu zintegrowanego lub klasycznego. W przypadku korzystania z usług IIS 7,0 w trybie zintegrowanym nie trzeba wykonywać żadnych specjalnych czynności konfiguracyjnych. Należy jednak wykonać dodatkową konfigurację w przypadku korzystania z usług IIS 7,0 w trybie klasycznym.

System Microsoft Windows Server 2003 zawiera usługi IIS 6,0. W przypadku korzystania z systemu operacyjnego Windows Server 2003 nie można uaktualnić usług IIS 6,0 do usług IIS 7,0. W przypadku korzystania z usług IIS 6,0 należy wykonać dodatkowe czynności konfiguracyjne.

Microsoft Windows XP Professional includes IIS 5.1. W przypadku korzystania z usług IIS 5,1 należy wykonać dodatkowe czynności konfiguracyjne.

Na koniec systemy Microsoft Windows 2000 i Microsoft Windows 2000 Professional zawierają usługi IIS 5,0. W przypadku korzystania z usług IIS 5,0 należy wykonać dodatkowe czynności konfiguracyjne.

## <a name="integrated-versus-classic-mode"></a>Zintegrowane w porównaniu z trybem klasycznym

Usługi IIS 7,0 mogą przetwarzać żądania przy użyciu dwóch różnych trybów przetwarzania żądań: zintegrowanych i klasycznych. Tryb zintegrowany zapewnia lepszą wydajność i więcej funkcji. Tryb klasyczny jest uwzględniany w celu zapewnienia zgodności z poprzednimi wersjami w starszych wersjach usług IIS.

Tryb przetwarzania żądań jest określany przez pulę aplikacji. Można określić, który tryb przetwarzania jest używany przez określoną aplikację sieci Web, określając pulę aplikacji skojarzoną z aplikacją. Wykonaj następujące kroki:

1. Uruchom Menedżera Internet Information Services
2. W oknie połączenia wybierz aplikację
3. W oknie Akcje kliknij link **Ustawienia podstawowe** , aby otworzyć okno dialogowe Edytowanie aplikacji (patrz rysunek 1).
4. Zanotuj wybraną pulę aplikacji.

Domyślnie usługi IIS są skonfigurowane do obsługi dwóch pul aplikacji: **Domyślna pula** **programu .NET puli aplikacji**. Jeśli zostanie wybrana domyślna pula aplikacji, aplikacja działa w trybie zintegrowanego przetwarzania żądań. W przypadku wybrania klasycznej platformy .NET puli aplikacji aplikacja działa w trybie klasycznego przetwarzania żądań.

[![okno dialogowe Nowy projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.png)

**Rysunek 1**. wykrywanie trybu przetwarzania żądań ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.png))

Należy zauważyć, że tryb przetwarzania żądań można modyfikować w oknie dialogowym Edytowanie aplikacji. Kliknij przycisk Wybierz i Zmień pulę aplikacji skojarzoną z aplikacją. Należy pamiętać, że występują problemy ze zgodnością podczas zmieniania aplikacji ASP.NET z trybu klasycznego na zintegrowany. Aby uzyskać więcej informacji zobacz następujące artykuły:

- Uaktualnianie ASP.NET 1,1 do usług IIS 7,0 w systemach Windows Vista i Windows Server 2008-- [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)
- Integracja ASP.NET z usługami IIS 7,0 — [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

Jeśli aplikacja ASP.NET korzysta z tej funkcji, nie trzeba wykonywać żadnych dodatkowych kroków w celu uzyskania routingu ASP.NET (a tym samym ASP.NET MVC) do pracy. Jeśli jednak aplikacja ASP.NET jest skonfigurowana do korzystania z klasycznej platformy .NET puli aplikacji, należy pamiętać, że masz więcej pracy.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Korzystanie z ASP.NET MVC ze starszymi wersjami usług IIS

Jeśli konieczne jest użycie ASP.NET MVC ze starszą wersją programu IIS niż IIS 7,0, lub należy użyć usług IIS 7,0 w trybie klasycznym, dostępne są dwie opcje. Najpierw można zmodyfikować tabelę tras tak, aby korzystała z rozszerzeń plików. Na przykład zamiast żądania adresu URL, takiego jak/Store/Details, należy zażądać adresu URL, takiego jak/Store.aspx/Details.

Druga opcja polega na utworzeniu czegoś o nazwie *wieloznacznej mapy skryptu*. Wieloznaczna Mapa skryptu umożliwia mapowanie każdego żądania do struktury ASP.NET.

Jeśli nie masz dostępu do serwera sieci Web (na przykład aplikacja ASP.NET MVC jest hostowana przez dostawcę usług internetowych), musisz użyć pierwszej opcji. Jeśli nie chcesz modyfikować wyglądu adresów URL i masz dostęp do serwera sieci Web, możesz użyć drugiej opcji.

Szczegółowo omówiono każdą opcję w poniższych sekcjach.

## <a name="adding-extensions-to-the-route-table"></a>Dodawanie rozszerzeń do tabeli tras

Najprostszym sposobem uzyskania ASP.NET routingu do pracy ze starszymi wersjami usług IIS jest zmodyfikowanie tabeli tras w pliku Global. asax. Domyślny i niemodyfikowany plik Global. asax w liście 1 konfiguruje jedną trasę o nazwie trasy domyślnej.

**Wyświetlanie listy 1 — Global. asax (niezmodyfikowane)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample1.cs)]

Trasa domyślna skonfigurowana na liście 1 umożliwia kierowanie adresów URL, które wyglądają następująco:

/Home/Index

/Product/Details/3

/Product

Niestety, starsze wersje usług IIS nie przekazują tych żądań do platformy ASP.NET Framework. W związku z tym te żądania nie będą kierowane do kontrolera. Na przykład, jeśli wprowadzisz żądanie przeglądarki dla adresu URL/Home/Index, otrzymasz stronę błędu na rysunku 2.

[![okno dialogowe Nowy projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.png)

**Rysunek 2**: otrzymywanie błędu 404 nie znaleziono ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.png))

Starsze wersje usług IIS mapują tylko niektóre żądania do platformy ASP.NET Framework. Żądanie musi dotyczyć adresu URL z rozszerzeniem odpowiedniego pliku. Na przykład żądanie/SomePage.aspx zostaje zamapowane na platformę ASP.NET. Jednak żądanie/SomePage.htm nie jest obsługiwane.

W związku z tym, aby umożliwić działanie routingu ASP.NET, należy zmodyfikować domyślną trasę, tak aby zawierała rozszerzenie pliku, które jest mapowane na platformę ASP.NET.

Odbywa się to przy użyciu skryptu o nazwie `registermvc.wsf`. Został on uwzględniony w wersji ASP.NET MVC 1 w `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, ale od ASP.NET 2 ten skrypt został przeniesiony do ASP.NET przyszłości dostępnych w [http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978).

Wykonanie tego skryptu rejestruje nowe rozszerzenie MVC z usługami IIS. Po zarejestrowaniu rozszerzenia MVC można zmodyfikować trasy w pliku Global. asax, aby trasy używały rozszerzenia MVC.

Zmodyfikowany plik Global. asax na liście 2 współpracuje ze starszymi wersjami usług IIS.

**Lista 2 — Global. asax (zmodyfikowano z rozszerzeniami)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample2.cs)]

**Ważne**: Pamiętaj, aby ponownie skompilować aplikację ASP.NET MVC po zmianie pliku Global. asax.

Istnieją dwie ważne zmiany w pliku Global. asax na liście 2. Istnieją teraz dwie trasy zdefiniowane w Global. asax. Wzorzec adresu URL dla trasy domyślnej, Pierwsza trasa, teraz wygląda następująco:

{Controller}. MVC/{Action}/{ID}

Dodanie rozszerzenia MVC powoduje zmianę typu plików przechwytywanych przez moduł routingu ASP.NET. W przypadku tej zmiany aplikacja ASP.NET MVC teraz kieruje żądania podobne do następujących:

/Home.mvc/Index/

/Product.mvc/Details/3

/Product.mvc/

Druga trasa, trasa główna, jest nowa. Ten wzorzec adresu URL dla trasy głównej jest ciągiem pustym. Ta trasa jest niezbędna do dopasowywania żądań skierowanych do katalogu głównego aplikacji. Na przykład trasa główna będzie zgodna z żądaniem, które wygląda następująco:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Po wprowadzeniu tych modyfikacji w tabeli tras należy upewnić się, że wszystkie linki w aplikacji są zgodne z tymi nowymi wzorcami adresów URL. Innymi słowy upewnij się, że wszystkie linki zawierają rozszerzenie. MVC. Jeśli używasz metody pomocnika html. ActionLink () w celu wygenerowania linków, nie musisz wprowadzać żadnych zmian.

Zamiast używać skryptu registermvc. WCF, można dodać nowe rozszerzenie do usług IIS, które są ponownie mapowane na platformę ASP.NET. Dodając nowe rozszerzenie, upewnij się, że pole wyboru z etykietą **Sprawdź, czy plik istnieje** nie jest zaznaczone.

## <a name="hosted-server"></a>Serwer hostowany

Nie zawsze masz dostęp do serwera sieci Web. Na przykład Jeśli zarządzasz aplikacją ASP.NET MVC przy użyciu internetowego dostawcy hostingu, nie musisz mieć dostępu do usług IIS.

W takim przypadku należy użyć jednego z istniejących rozszerzeń plików, które są mapowane na strukturę ASP.NET. Przykłady rozszerzeń plików mapowanych na ASP.NET obejmują rozszerzenia. aspx,. axd i. ashx.

Na przykład zmodyfikowany plik Global. asax na liście 3 używa rozszerzenia aspx zamiast rozszerzenia MVC.

**Lista 3-Global. asax (zmodyfikowano przy użyciu rozszerzeń. aspx)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample3.cs)]

Plik Global. asax na liście 3 jest dokładnie taki sam jak poprzedni plik Global. asax, z wyjątkiem tego, że używa rozszerzenia. aspx zamiast rozszerzenia. MVC. Nie musisz wykonywać żadnych ustawień na zdalnym serwerze sieci Web, aby użyć rozszerzenia. aspx.

## <a name="creating-a-wildcard-script-map"></a>Tworzenie wieloznacznej mapy skryptu

Jeśli nie chcesz modyfikować adresów URL aplikacji ASP.NET MVC i masz dostęp do serwera sieci Web, będziesz mieć dodatkową opcję. Można utworzyć wieloznaczną mapę skryptu, która mapuje wszystkie żądania na serwer sieci Web na strukturę ASP.NET. Dzięki temu można użyć domyślnej tabeli tras ASP.NET MVC z usługami IIS 7,0 (w trybie klasycznym) lub IIS 6,0.

Należy pamiętać, że ta opcja powoduje, że usługi IIS przechwytuje każde żądanie skierowane do serwera sieci Web. Obejmuje to żądania dotyczące obrazów, klasycznych stron ASP i stron HTML. Dlatego włączenie wieloznacznej mapy skryptu do ASP.NET ma wpływ na wydajność.

Poniżej przedstawiono sposób włączania wieloznacznej mapy skryptów dla usług IIS 7,0:

1. Wybierz aplikację w oknie połączenia
2. Upewnij się, że wybrano widok **funkcje**
3. Kliknij dwukrotnie przycisk **mapowania obsługi**
4. Kliknij łącze **Dodaj wieloznaczną mapę skryptu** (patrz rysunek 3).
5. Wprowadź ścieżkę do pliku ASPNET\_ISAPI. dll (można skopiować tę ścieżkę z mapy skryptu PageHandlerFactory)
6. Wprowadź nazwę MVC
7. Kliknij przycisk **OK**

[![okno dialogowe Nowy projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.png)

**Rysunek 3**. Tworzenie wieloznacznej mapy skryptów przy użyciu usług IIS 7,0 ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image6.png))

Wykonaj następujące kroki, aby utworzyć wieloznaczną mapę skryptu z usługami IIS 6,0:

1. Kliknij prawym przyciskiem myszy witrynę sieci Web i wybierz polecenie Właściwości
2. Wybierz kartę **katalog macierzysty**
3. Kliknij przycisk **konfiguracji**
4. Wybierz kartę **mapowania**
5. Kliknij przycisk **Wstaw** (zobacz rysunek 4)
6. Wklej ścieżkę do pliku ASPNET\_ISAPI. dll do pola wykonywalnego (można skopiować tę ścieżkę z mapy skryptów dla plików. aspx)
7. Usuń zaznaczenie pola wyboru z etykietą **Sprawdź, czy plik istnieje**
8. Kliknij przycisk **OK**

[![okno dialogowe Nowy projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image7.png)

**Rysunek 4**. Tworzenie wieloznacznej mapy skryptów z usługami IIS 6,0 ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image8.png))

Po włączeniu map skryptów symboli wieloznacznych należy zmodyfikować tabelę tras w pliku Global. asax, tak aby zawierała trasę główną. W przeciwnym razie otrzymasz stronę błędu na rysunku 5, gdy wyślesz żądanie dotyczące strony głównej aplikacji. Możesz użyć zmodyfikowanego pliku Global. asax na liście 4.

[![okno dialogowe Nowy projekt](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image9.png)

**Rysunek 5**. błąd braku trasy głównej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image10.png))

**Lista 4 — Global. asax (zmodyfikowano przy użyciu trasy głównej)**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample4.cs)]

Po włączeniu wieloznacznej mapy skryptu dla usług IIS 7,0 lub IIS 6,0 można wykonać żądania, które współpracują z domyślną tabelą tras, która wygląda następująco:

/

/Home/Index

/Product/Details/3

/Product

## <a name="summary"></a>Podsumowanie

Celem tego samouczka jest wyjaśnienie, jak używać składnika ASP.NET MVC w przypadku korzystania ze starszej wersji usług IIS (lub usług IIS 7,0 w trybie klasycznym). Omawiamy dwie metody uzyskiwania routingu ASP.NET do pracy ze starszymi wersjami usług IIS: Zmodyfikuj domyślną tabelę tras lub Utwórz wieloznaczną mapę skryptu.

Pierwsza opcja wymaga modyfikacji adresów URL używanych w aplikacji ASP.NET MVC. Jedną z bardzo znaczących zalet tej pierwszej opcji jest brak dostępu do serwera sieci Web w celu zmodyfikowania tabeli tras. Oznacza to, że można użyć tej pierwszej opcji, nawet w przypadku hostowania aplikacji ASP.NET MVC z firmą hostingu internetowego.

Druga opcja polega na utworzeniu wieloznacznej mapy skryptu. Zaletą tej drugiej opcji jest to, że nie trzeba modyfikować adresów URL. Wadą tej drugiej opcji jest to, że może to mieć wpływ na wydajność aplikacji ASP.NET MVC.

> [!div class="step-by-step"]
> [Dalej](using-asp-net-mvc-with-different-versions-of-iis-vb.md)

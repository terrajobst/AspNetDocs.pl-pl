---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
title: Wdrażanie witryny przy użyciu klienta FTP (VB) | Microsoft Docs
author: rick-anderson
description: Najprostszym sposobem wdrożenia aplikacji ASP.NET jest ręczne skopiowanie niezbędnych plików ze środowiska programistycznego do środowiska produkcyjnego. Thi...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 09279194-bcf9-4b59-a09d-c68e5926a758
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
msc.type: authoredcontent
ms.openlocfilehash: 7875304c672625d8c0eaaf0fea8ef509bb801a3a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611843"
---
# <a name="deploying-your-site-using-an-ftp-client-vb"></a>Wdrażanie witryny przy użyciu klienta FTP (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_vb.pdf)

> Najprostszym sposobem wdrożenia aplikacji ASP.NET jest ręczne skopiowanie niezbędnych plików ze środowiska programistycznego do środowiska produkcyjnego. W tym samouczku pokazano, jak za pomocą klienta FTP pobrać pliki z pulpitu do dostawcy hosta sieci Web.

## <a name="introduction"></a>Wprowadzenie

W poprzednim samouczku wprowadzono prostą ASP.NETą aplikację sieci Web, która składa się z kilku stron ASP.NET, strony wzorcowej, niestandardowej klasy podstawowej `Page`, wielu obrazów i trzech arkuszy stylów CSS. Teraz wszystko jest gotowe do wdrożenia tej aplikacji dla dostawcy hosta sieci Web. w takim przypadku aplikacja będzie dostępna dla wszystkich użytkowników mających połączenie z Internetem.

Z naszych dyskusji w temacie [*określanie plików wymagających wdrożenia*](determining-what-files-need-to-be-deployed-vb.md) samouczka wiemy, które pliki muszą zostać skopiowane do dostawcy hosta sieci Web. (Należy odwołać się do tego, jakie pliki są kopiowane, zależy od tego, czy aplikacja jest jawnie czy automatycznie skompilowana). Ale jak można pobrać pliki ze środowiska programistycznego (naszego pulpitu) do środowiska produkcyjnego (serwera sieci Web zarządzanego przez dostawcę hosta sieci Web)? [ Iku **T** ransfer **p** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) jest powszechnie używanym protokołem do kopiowania plików z jednego komputera do drugiego za pośrednictwem sieci. Inna opcja to rozszerzenia FrontPage Server Extensions (FPSE). Ten samouczek koncentruje się na korzystaniu z autonomicznego oprogramowania klienckiego FTP do wdrażania niezbędnych plików ze środowiska programistycznego w środowisku produkcyjnym.

> [!NOTE]
> Program Visual Studio zawiera narzędzia do publikowania witryn sieci Web za pośrednictwem protokołu FTP; te narzędzia, a także Przyjrzyj się narzędziom, które używają rozszerzeń FPSE, zostały omówione w następnym samouczku.

Aby skopiować pliki przy użyciu protokołu FTP, musimy mieć *klienta FTP* w środowisku deweloperskim. Klient FTP to aplikacja, która jest przeznaczona do kopiowania plików z komputera, który jest instalowany na komputerze z uruchomionym *serwerem FTP*. (Jeśli dostawca hosta sieci Web obsługuje transfery plików za pośrednictwem protokołu FTP, to na serwerze sieci Web jest uruchomiony serwer FTP). Istnieje wiele dostępnych aplikacji klienckich FTP. Przeglądarka sieci Web może nawet dwukrotnie być klientem FTP. Moim ulubionym klientem FTP, który będzie używany w tym samouczku, jest [FileZilla](http://filezilla-project.org/), bezpłatny klient FTP typu open source dostępny dla systemów Windows, Linux i Mac. Każdy klient FTP będzie działał, ale może korzystać z dowolnego klienta, który z największym doświadczeniem.

Jeśli jesteś następnym, musisz utworzyć konto z dostawcą hosta sieci Web, aby móc ukończyć ten samouczek lub kolejne. Jak wspomniano w poprzednim samouczku, istnieje Gaggle firm dostawcy hosta sieci Web z szeroką gamę cen, funkcji i jakości usług. W tej serii samouczków użyjemy [rabatu ASP.NET](http://discountasp.net) jako dostawcy hosta sieci Web, ale możesz wykonać te czynności razem z dowolnym dostawcą hosta sieci Web, o ile będą one obsługiwały wersję ASP.NET, w której jest opracowywana witryna. (Te samouczki zostały utworzone przy użyciu ASP.NET 3,5). Ponadto, ponieważ pliki zostaną skopiowane do dostawcy hosta sieci Web przy użyciu protokołu FTP w tym samouczku i w przyszłości będzie to konieczne, aby dostawca hosta sieci Web obsługiwał dostęp za pośrednictwem protokołu FTP do swoich serwerów sieci Web. Praktycznie wszyscy dostawcy hosta sieci Web oferują tę funkcję, ale należy dokładnie sprawdzić przed zarejestrowaniem się.

## <a name="deploying-the-book-review-web-application-project"></a>Wdrażanie projektu aplikacji sieci Web Recenzja książki

Odwołaj się, że istnieją dwie wersje aplikacji sieci Web przeglądu książki: jeden zaimplementowany przy użyciu modelu projektu aplikacji sieci Web (BookReviewsWAP), a drugi przy użyciu modelu projektu witryny sieci Web (BookReviewsWSP). Typ projektu ma wpływ na to, czy lokacja jest skompilowana automatycznie, czy jawnie, i czy ten model kompilacji określa, jakie pliki muszą zostać wdrożone. W związku z tym sprawdzimy osobno wdrażanie projektów BookReviewsWAP i BookReviewsWSP, rozpoczynając od BookReviewsWAP. Poświęć chwilę na pobranie tych dwóch aplikacji ASP.NET, jeśli jeszcze tego nie zrobiono.

Uruchom projekt BookReviewsWAP, przechodząc do folderu `BookReviewsWAP`, a następnie klikając dwukrotnie plik `BookReviewsWAP.sln`. Przed wdrożeniem projektu należy go skompilować, aby upewnić się, że wszystkie zmiany w kodzie źródłowym zostaną uwzględnione w skompilowanym zestawie. Aby skompilować projekt, przejdź do menu Kompilacja i wybierz opcję menu Kompilacja BookReviewsWAP. Spowoduje to skompilowanie kodu źródłowego w projekcie do jednego zestawu, `BookReviewsWAP.dll`, który znajduje się w folderze `Bin`.

Teraz wszystko jest gotowe do wdrożenia wymaganych plików. Uruchom klienta FTP i Połącz się z serwerem sieci Web u dostawcy hosta sieci Web. (Po zarejestrowaniu się w firmie hostingowej w sieci Web wyśle ona pocztą e-mail informacje dotyczące sposobu nawiązywania połączenia z serwerem FTP; obejmuje to adres serwera FTP oraz nazwę użytkownika i hasło).

Skopiuj następujące pliki z pulpitu do folderu głównego witryny sieci Web na swoim dostawcy hosta. Gdy korzystasz z protokołu FTP na serwerze sieci Web u dostawcy hosta sieci Web, najkorzystniej znajduje się w katalogu głównej witryny internetowej. Niektórzy dostawcy hosta sieci Web mają jednak podfolder o nazwie `www` lub `wwwroot`, który służy jako folder główny plików witryny sieci Web. Na koniec podczas FTPing plików może być konieczne utworzenie odpowiedniej struktury folderów w środowisku produkcyjnym — folder `Bin`, folder `Fiction`, folder `Images` i tak dalej.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Pełna zawartość folderu `Styles`
- Pełna zawartość folderu `Images` (wraz z jego podfolderem `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

Rysunek 1 przedstawia FileZilla po skopiowaniu wymaganych plików. FileZilla Wyświetla pliki na komputerze lokalnym po lewej stronie i pliki na komputerze zdalnym po prawej stronie. Jak pokazano na rysunku 1, pliki kodu źródłowego ASP.NET, takie jak `About.aspx.vb`, znajdują się na komputerze lokalnym (środowisku programistycznym), ale nie zostały skopiowane do dostawcy hosta sieci Web (środowisko produkcyjne), ponieważ pliki kodu nie muszą zostać wdrożone podczas korzystania z kompilacji jawnej.

> [!NOTE]
> Nie ma żadnego uszkodzenia plików kodu źródłowego na serwerze produkcyjnym, ponieważ są one ignorowane. ASP.NET domyślnie zabrania żądań HTTP do plików kodu źródłowego, tak że nawet jeśli pliki kodu źródłowego są obecne na serwerze produkcyjnym, są niedostępne dla odwiedzających witrynę sieci Web. (Oznacza to, że jeśli użytkownik próbuje odwiedzić `http://www.yoursite.com/Default.aspx.vb` zostanie wykorzystana strona błędu, która wyjaśnia, że te typy plików — `.vb` plików — są zabronione).

[![użyć klienta FTP do skopiowania niezbędnych plików z pulpitu do serwera sieci Web u dostawcy hosta sieci Web.](deploying-your-site-using-an-ftp-client-vb/_static/image2.png)](deploying-your-site-using-an-ftp-client-vb/_static/image1.png)

**Rysunek 1**. Użyj klienta FTP, aby skopiować wymagane pliki z pulpitu do serwera sieci Web u dostawcy hosta sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-your-site-using-an-ftp-client-vb/_static/image3.png))

Po wdrożeniu witryny Poświęć chwilę na przetestowanie lokacji. Jeśli zakupiono nazwę domeny i skonfigurowano ustawienia DNS poprawnie, można odwiedzić witrynę, wprowadzając nazwę domeny. Alternatywnie dostawca hosta sieci Web powinien przekazać Ci adres URL do witryny, która będzie wyglądać podobnie do tego *konta*. *webhostprovider*. com lub *webhostprovider*. com/*AccountName*. Na przykład adres URL dla mojego konta w przypadku rabatu ASP.NET to: `http://httpruntime.web703.discountasp.net`.

Rysunek 2 przedstawia witrynę wdrożone przeglądy książki. Zauważ, że wyświetlam je w ramach rabatu ASP. Serwery sieci, w `http://httpruntime.web703.discountasp.net`. W tym momencie każda osoba z połączeniem z Internetem może wyświetlić moją witrynę internetową. Zgodnie z oczekiwaniami witryna wygląda i zachowuje się tak samo, jak podczas testowania w środowisku programistycznym.

> [!NOTE]
> Jeśli wystąpi błąd podczas wyświetlania aplikacji, poświęć chwilę, aby upewnić się, że wdrożono poprawny zestaw plików. Następnie sprawdź komunikat o błędzie, aby zobaczyć, czy ujawnia on wszelkie wskazówki dotyczące problemu. Poniżej można włączyć pomoc techniczną firmy hosta sieci Web lub opublikować pytanie na odpowiednim forum na [forach ASP.NET](https://forums.asp.net/).

[![witryna przeglądy książek jest teraz dostępna dla wszystkich użytkowników z połączeniem internetowym.](deploying-your-site-using-an-ftp-client-vb/_static/image5.png)](deploying-your-site-using-an-ftp-client-vb/_static/image4.png)

**Rysunek 2**. witryna przeglądy książki jest teraz dostępna dla wszystkich użytkowników z połączeniem internetowym ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-your-site-using-an-ftp-client-vb/_static/image6.png))

## <a name="deploying-the-book-review-web-site-project"></a>Wdrażanie projektu witryny sieci Web przegląd książki

Podczas wdrażania aplikacji ASP.NET, która używa automatycznej kompilacji, takiej jak projekt witryny sieci Web BookReviewsWSP, nie ma skompilowanego zestawu w folderze `Bin`. W związku z tym pliki kodu źródłowego aplikacji sieci Web muszą zostać wdrożone w środowisku produkcyjnym. Przeprowadzimy Cię przez ten proces.

Podobnie jak w przypadku projektu aplikacji sieci Web, warto najpierw skompilować aplikację przed jej wdrożeniem. Podczas kompilowania projektu witryny sieci Web nie tworzy zestawu, sprawdza wszystkie błędy czasu kompilacji na stronie. Lepiej, aby znaleźć te błędy, a nie wyszukiwać ich przez odwiedzających witrynę!

Po pomyślnym skompilowaniu projektu Użyj klienta FTP, aby skopiować następujące pliki do folderu głównego witryny sieci Web u dostawcy hosta. Może być konieczne utworzenie odpowiedniej struktury folderów w środowisku produkcyjnym.

> [!NOTE]
> Jeśli projekt BookReviewsWAP został już wdrożony, ale nadal chcesz spróbować wdrożyć projekt BookReviewsWSP, najpierw usuń wszystkie pliki na serwerze sieci Web, które zostały przekazane podczas wdrażania BookReviewsWAP, a następnie wdróż pliki dla BookReviewsWSP.

- `~/Default.aspx`
- `~/Default.aspx.vb`
- `~/About.aspx`
- `~/About.aspx.vb`
- `~/Site.master`
- `~/Site.master.vb`
- `~/Web.config`
- `~/Web.sitemap`
- Pełna zawartość folderu `Styles`
- Pełna zawartość folderu `Images` (wraz z jego podfolderem `BookCovers`)
- `~/App_Code/BasePage.vb`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.vb`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.vb`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.vb`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.vb`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.vb`

Rysunek 3 przedstawia FileZilla po skopiowaniu wymaganych plików. Jak widać, pliki kodu źródłowego ASP.NET, takie jak `About.aspx.vb`, znajdują się na komputerze lokalnym (środowisku programistycznym) i dostawcy hosta sieci Web (środowisku produkcyjnym), ponieważ pliki kodu muszą zostać wdrożone podczas korzystania z automatycznej kompilacji.

[![użyć klienta FTP do skopiowania niezbędnych plików z pulpitu do serwera sieci Web u dostawcy hosta sieci Web](deploying-your-site-using-an-ftp-client-vb/_static/image8.png)](deploying-your-site-using-an-ftp-client-vb/_static/image7.png)

**Rysunek 3**. Używanie klienta FTP do kopiowania niezbędnych plików z pulpitu do serwera sieci Web u dostawcy hosta sieci Web ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](deploying-your-site-using-an-ftp-client-vb/_static/image9.png))

Model kompilacji aplikacji nie ma wpływ na środowisko użytkownika. Te same strony ASP.NET są dostępne i wyglądają i zachowują się tak samo, niezależnie od tego, czy witryna sieci Web została utworzona przy użyciu modelu projektu aplikacji internetowej czy modelu projektu witryny sieci Web.

## <a name="updating-a-web-application-on-production"></a>Aktualizowanie aplikacji sieci Web w środowisku produkcyjnym

Tworzenie i wdrażanie aplikacji sieci Web nie jest procesem jednorazowym. Załóżmy na przykład, że podczas tworzenia witryny sieci Web przegląd książki zostały skompilowane różne strony i zapisano kod towarzyszący na komputerze osobistym (środowisko programistyczne). Po osiągnięciu pewnego stabilnego stanu wdrożono moją aplikację, tak aby inni mogli odwiedzić witrynę i przeczytać moje recenzje. Ale wdrożenie nie oznacza końca mojego rozwoju w tej witrynie. Mogę dodać więcej przeglądów książki lub zaimplementować nowe funkcje, takie jak umożliwienie Gościom oceniania książek lub pozostawiania własnych komentarzy. Takie udoskonalenia byłyby opracowywane w środowisku programistycznym i, po zakończeniu, muszą zostać wdrożone. Tworzenie i wdrażanie, w związku z tym, są cykliczne. Tworzysz aplikację, a następnie ją wdróżesz. Gdy witryna działa i w środowisku produkcyjnym, nowe funkcje są dodawane, a błędy są rozwiązane z upływem czasu, co wymaga ponownego wdrożenia aplikacji. Itd.

Zgodnie z oczekiwaniami, podczas ponownego wdrażania aplikacji sieci Web wystarczy tylko skopiować nowe i zmienione pliki. Nie ma potrzeby ponownego wdrażania niezmienionych stron lub plików obsługi po stronie serwera lub klienta (choć nie jest to szkodliwe).

> [!NOTE]
> Należy pamiętać, że podczas korzystania z jawnej kompilacji jest to, że w dowolnym momencie dodasz nową stronę ASP.NET do projektu lub wprowadzisz wszelkie zmiany związane z kodem, należy ponownie skompilować projekt, który zaktualizuje zestaw w folderze `Bin`. W związku z tym należy skopiować ten zaktualizowany zestaw do środowiska produkcyjnego podczas aktualizowania aplikacji sieci Web w środowisku produkcyjnym (wraz z inną nową i zaktualizowaną zawartością).

Należy również zrozumieć, że wszelkie zmiany `Web.config` lub plików w katalogu `Bin` zatrzymają i ponownie uruchamiają pulę aplikacji witryny sieci Web. Jeśli stan sesji jest przechowywany przy użyciu trybu `InProc` (domyślnie), osoby odwiedzające witrynę utracą swój stan sesji przy każdej modyfikacji tych plików kluczy. Aby uniknąć tego Pitfall, należy rozważyć zapisanie sesji przy użyciu trybów `StateServer` lub `SQLServer`. Aby uzyskać więcej informacji na temat tego tematu, zobacz [tryby stanu sesji](https://msdn.microsoft.com/library/ms178586.aspx).

Na koniec należy pamiętać, że ponowne wdrażanie aplikacji może potrwać od kilku sekund do kilku minut, w zależności od liczby i rozmiaru plików, które muszą być skopiowane do środowiska produkcyjnego. W tym czasie użytkownicy odwiedzający witrynę mogą napotkać błędy lub niezachowanie nieparzyste. Możesz "wyłączyć całą aplikację, dodając stronę o nazwie `App_Offline.htm` do katalogu głównego aplikacji, która wyjaśnia użytkownikom, że witryna nie działa do konserwacji (lub niezależnie od tego) i wkrótce zostanie utworzona kopia zapasowa. Gdy plik `App_Offline.htm` jest obecny, środowisko uruchomieniowe ASP.NET przekierowuje wszystkie żądania przychodzące do tej strony.

## <a name="summary"></a>Podsumowanie

Wdrożenie aplikacji sieci Web wiąże się z kopiowaniem niezbędnych plików ze środowiska deweloperskiego do środowiska produkcyjnego. Najczęstszym sposobem transferu plików za pośrednictwem sieci jest usługa protokół transferu plików (FTP), a większość dostawców hosta sieci Web obsługuje dostęp do usługi FTP do swoich serwerów sieci Web. W tym samouczku przedstawiono sposób użycia klienta FTP do wdrożenia wymaganych plików na serwerze sieci Web. Po wdrożeniu witryna sieci Web może być odwiedzana przez dowolną osobę z połączeniem z Internetem.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Aplikacja\_w trybie offline. htm i działa wokół funkcji "przyjazne błędy programu IE"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Tryby stanu sesji](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [Poprzednie](determining-what-files-need-to-be-deployed-vb.md)
> [dalej](deploying-your-site-using-visual-studio-vb.md)

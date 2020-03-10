---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
title: Prekompilowanie witryny sieciC#Web () | Microsoft Docs
author: rick-anderson
description: 'Program Visual Studio oferuje deweloperom ASP.NET dwa typy projektów: projekty aplikacji sieci Web (WAPs) i projekty witryn sieci Web (WSPs). Jedna z różnic kluczowych betwe...'
ms.author: riande
ms.date: 06/09/2009
ms.assetid: ecd5a4de-beb7-4d1d-bbbb-e31003633267
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
msc.type: authoredcontent
ms.openlocfilehash: cb42398f44f3cd16ab83a9976e7b34f132384456
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573508"
---
# <a name="precompiling-your-website-c"></a>Prekompilowanie witryny internetowej (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_CS.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_cs.pdf)

> Program Visual Studio oferuje deweloperom ASP.NET dwa typy projektów: projekty aplikacji sieci Web (WAPs) i projekty witryn sieci Web (WSPs). Jedną z kluczowych różnic między dwoma typami projektów jest to, że WAPs musi mieć kod jawnie skompilowany przed wdrożeniem, podczas gdy kod w programie WSP można automatycznie kompilować na serwerze sieci Web. Istnieje jednak możliwość wstępnego skompilowania programu WSP przed wdrożeniem. W tym samouczku przedstawiono zalety wstępnej kompilacji i pokazano, jak prekompilować witrynę sieci Web z poziomu programu Visual Studio i z wiersza polecenia.

## <a name="introduction"></a>Wprowadzenie

Program Visual Studio oferuje deweloperom ASP.NET dwa różne typy projektów: projekty aplikacji sieci Web (WAP) i projekty witryn sieci Web (WSP). Jedną z kluczowych różnic między tymi typami projektu jest to, że WAPs wymaga *jawnej kompilacji* , a domyślnie WSPs używać *automatycznej kompilacji*. Za pomocą WAPs można skompilować kod aplikacji sieci Web do jednego zestawu, który jest tworzony w folderze `Bin` witryny sieci Web. Wdrożenie wiąże się z kopiowaniem zawartości znacznika (`.aspx.ascx`i `.master` plików) w projekcie wraz z zestawem w folderze `Bin`; pliki klas związanych z kodem nie muszą być wdrażane. Z drugiej strony należy wdrożyć WSPs przez skopiowanie zarówno stron znaczników, jak i odpowiednich klas związanych z kodem do środowiska produkcyjnego. Klasy związane z kodem są kompilowane na żądanie na serwerze sieci Web.

> [!NOTE]
> Zapoznaj się z sekcją "jawne kompilacje i kompilacja automatyczna" w temacie [ *Określanie, które pliki muszą zostać wdrożone* ](determining-what-files-need-to-be-deployed-cs.md) , aby uzyskać więcej informacji na temat różnic między modelami projektu, jawną i automatyczną kompilacją oraz sposobem, w jaki model kompilacji ma wpływ na wdrożenie.

Opcja automatycznej kompilacji jest prosta do użycia. Nie istnieje żaden jawny krok kompilacji i tylko te pliki, które zostały zmodyfikowane, muszą zostać wdrożone, podczas gdy jawna kompilacja wymaga wdrożenia zmienionych stron znaczników i właśnie skompilowanego zestawu. Jednak wdrożenie automatyczne ma dwa potencjalne wady:

- Ze względu na to, że strony muszą być automatycznie kompilowane podczas pierwszej wizyty, może wystąpić krótkie, ale zauważalne opóźnienie, gdy strona ASP.NET jest wymagana po raz pierwszy po wdrożeniu.
- Automatyczne Kompilowanie wymaga, aby na serwerze sieci Web znajdował się zarówno znacznik deklaratywny, jak i kod źródłowy. Może to być nieatrakcyjna opcja w przypadku planowania sprzedaży aplikacji sieci Web klientom, którzy zainstalują ją na serwerach sieci Web.

Jeśli jedno z tych wad jest wyłącznikiem, można przełączyć się do modelu WAP lub *prekompilować* wsp przed wdrożeniem. W tym samouczku przedstawiono opcje wstępnej kompilacji najlepiej dopasowane do hostowanej witryny sieci Web i przeprowadzą proces prekompilacji oraz wdrożenie wstępnie skompilowanej witryny sieci Web.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Omówienie generowania i kompilowania kodu ASP.NET

Zanim zaczniemy dostęp do dostępnych opcji kompilacji, najpierw porozmawiamy o generowaniu kodu i kompilacji, która występuje, gdy strona ASP.NET jest zażądana po raz pierwszy od czasu utworzenia lub ostatniej aktualizacji. Jak wiadomo, strony ASP.NET składają się z dwóch części: znaczników deklaratywnych w pliku `.aspx`; i część kodu źródłowego, zazwyczaj w osobnym pliku klasy (`.aspx.cs`). Kroki wykonywane przez środowisko uruchomieniowe po zażądaniu strony ASP.NET zależą od modelu kompilacji aplikacji.

W przypadku WAPs, kod źródłowy stron musi być jawnie skompilowany do jednego zestawu przed wdrożeniem. Podczas wdrażania ten zestaw i różne strony znaczników są kopiowane do środowiska produkcyjnego. Gdy żądanie dociera do serwera sieci Web dla strony ASP.NET, środowisko uruchomieniowe tworzy wystąpienie klasy związanej z kodem strony i wywołuje jej metodę `ProcessRequest`, która uruchamia cykl życia strony, a ostatecznie generuje zawartość strony, która jest zwracana do obiektu żądającego. Środowisko uruchomieniowe może współdziałać z klasą ASP.NET strony z kodem, ponieważ Klasa Poprzednia kodu została już skompilowana do zestawu przed wdrożeniem.

W przypadku WSPs i automatycznej kompilacji nie istnieje żaden jawny krok kompilacji przed wdrożeniem. Zamiast tego wdrożenie obejmuje kopiowanie deklaratywnej i źródłowej zawartości kodu do środowiska produkcyjnego. Gdy żądanie dociera do serwera sieci Web dla strony ASP.NET po raz pierwszy od momentu utworzenia lub ostatniej aktualizacji strony, środowisko uruchomieniowe musi najpierw skompilować klasę związany z kodem do zestawu. Ten skompilowany zestaw jest zapisywany w folderze `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, mimo że lokalizacja tego folderu może być dostosowywana za pośrednictwem `<compilation tempDirectory="" />` elementu `<system.web>`, zazwyczaj w `Web.config`. Ponieważ zestaw jest zapisywany na dysku, nie trzeba go ponownie kompilować na kolejnych żądaniach do tej samej strony.

> [!NOTE]
> Zgodnie z oczekiwaniami istnieje niewielkie opóźnienie podczas żądania strony po raz pierwszy (lub po raz pierwszy od momentu jego zmiany) w lokacji korzystającej z automatycznej kompilacji, ponieważ serwer programu ma chwilę kompilować kod strony i zapisuje zestaw wynikający z 3,5.

W skrócie, z jawną kompilacją wymagane jest skompilowanie kodu źródłowego witryny sieci Web przed wdrożeniem, co pozwala zaoszczędzić środowisko uruchomieniowe przed wykonaniem tego kroku. Dzięki automatycznej kompilacji środowisko uruchomieniowe obsługuje kompilację kodu źródłowego stron, ale z niewielkim kosztem inicjacji pierwszego odwiedzania strony od momentu utworzenia lub ostatniej aktualizacji.

Ale co się stało z deklaracyjnej częścią stron ASP.NET (plik `.aspx`)? Oczywiste jest, że istnieje relacja między plikami `.aspx` i kodem w ich klasach znajdujących się w kodzie, ponieważ kontrolki sieci Web zdefiniowane w oznakowaniu deklaratywnym są dostępne w kodzie. Jest to również oczywiste, że zawartość w plikach `.aspx` znacząco wpływa na renderowane znaczniki wygenerowane przez stronę. Tak więc jak środowisko uruchomieniowe współpracuje z składnią tekstu, HTML i kontrolki sieci Web zdefiniowaną w pliku `.aspx` w celu wygenerowania renderowanej zawartości strony?

Nie chcę otrzymywać zbyt sidetracked na temat szczegółów implementacji niskiego poziomu, które różnią się między WAPs i WSPs, ale w Nutshell środowisko uruchomieniowe automatycznie generuje plik klasy, który zawiera różne kontrolki sieci Web jako chronione elementy członkowskie i metody. Ten wygenerowany plik jest zaimplementowany jako *Klasa częściowa* do odpowiedniej klasy powiązanej z kodem. ([Częściowe klasy](http://www.dotnet-guide.com/partialclasses.html) umożliwiają rozproszenie zawartości pojedynczej klasy między wiele plików). W związku z tym, Klasa z kodem jest zdefiniowana w dwóch miejscach: w utworzonym pliku `.aspx.cs` i w tej automatycznie generowanej klasie utworzonej przez środowisko uruchomieniowe. Ta automatycznie wygenerowana Klasa jest przechowywana w folderze `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`.

Ważne jest, aby strona ASP.NET mogła być renderowana przez środowisko uruchomieniowe zarówno, jak i fragmenty kodu źródłowego muszą być kompilowane do zestawu. W przypadku WAPs kod źródłowy jest jawnie kompilowany do zestawu przed wdrożeniem, ale znaczniki deklaratywne muszą być nadal konwertowane na kod i kompilowane przez środowisko uruchomieniowe na serwerze sieci Web. W przypadku WSPs przy użyciu automatycznej kompilacji zarówno kod źródłowy, jak i znaczniki deklaratywne muszą być kompilowane przez serwer sieci Web.

Istnieje możliwość użycia kompilacji jawnej z modelem WSP. Można jawnie skompilować fragment kodu źródłowego, taki jak model WAP. Co więcej, można także skompilować deklaratywne znaczniki.

## <a name="precompilation-options"></a>Opcje wstępnej kompilacji

.NET Framework jest dostarczany z [narzędziem kompilacji ASP.NET (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) , które pozwala kompilować kod źródłowy (a nawet zawartość) aplikacji ASP.NET utworzonej przy użyciu modelu wsp. To narzędzie zostało wydane z .NET Framework wersja 2,0 i znajduje się w folderze `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`; można go użyć z wiersza polecenia lub uruchomić z poziomu programu Visual Studio za pomocą opcji Publikuj witrynę sieci Web menu Kompilacja.

Narzędzie do kompilacji oferuje dwie ogólne formy kompilacji: prekompilacja w miejscu i prekompilacja do wdrożenia. W przypadku wstępnej kompilacji w miejscu należy uruchomić narzędzie `aspnet_compiler.exe` z wiersza polecenia i określić ścieżkę do katalogu wirtualnego lub ścieżki fizycznej witryny sieci Web, która znajduje się na komputerze. Następnie narzędzie kompilacji kompiluje każdą stronę ASP.NET w projekcie, przechowując skompilowaną wersję w folderze `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, tak jak gdyby strony były odwiedzane po raz pierwszy z przeglądarki. Prekompilacja w miejscu może przyspieszyć pierwsze żądanie wykonane na nowo wdrożonych stronach ASP.NET w witrynie, ponieważ pozwala to wyeliminować czas wykonywania tego kroku. Jednak prekompilacja w miejscu nie jest przydatna w przypadku większości hostowanych witryn sieci Web, ponieważ wymaga, aby można było uruchamiać programy z poziomu wiersza polecenia serwera internetowego. W środowiskach hostingu udostępnionego ten poziom dostępu jest niedozwolony.

> [!NOTE]
> Aby uzyskać więcej informacji na temat wstępnej kompilacji w miejscu, zapoznaj [się z tematem: prekompilowanie witryn sieci Web ASP.NET](https://msdn.microsoft.com/library/ms227972.aspx) i wstępnej [kompilacji w ASP.NET 2,0](http://www.odetocode.com/Articles/417.aspx).

Zamiast kompilowania stron w witrynie sieci Web do folderu `Temporary ASP.NET Files`, prekompilacja do wdrożenia kompiluje strony do katalogu wybieranego i w formacie, który można wdrożyć w środowisku produkcyjnym.

Istnieją dwa rodzaje prekompilacji do wdrożenia, które opisano w tym samouczku: prekompilacja z aktualizowalnym interfejsem użytkownika i prekompilowanie przy użyciu nieaktualizowalnego interfejsu użytkownika. Prekompilacja przy użyciu aktualizowalnego interfejsu użytkownika pozostawia znaczniki deklaratywne w plikach `.aspx`, `.ascx`i `.master`, dzięki czemu deweloperzy mogą wyświetlać i, w razie potrzeby, modyfikować znaczniki deklaratywne na serwerze produkcyjnym. Prekompilacja z nieaktualizowalnym interfejsem użytkownika generuje `.aspx` stron, które są puste dla żadnej zawartości i usuwa `.ascx` i `.master` pliki, ukrywając znaczniki deklaratywne i uniemożliwiając deweloperowi zmianę go ze środowiska produkcyjnego.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Prekompilowanie wdrożenia przy użyciu aktualizowalnego interfejsu użytkownika

Najlepszym sposobem zrozumienia prekompilowania wdrożenia jest Zobacz przykład akcji. Skompilujemy ponownie kompilacje w programie WSP for Deployment przy użyciu aktualizowalnego interfejsu użytkownika. Narzędzie kompilacji ASP.NET może być wywoływane z menu kompilacji programu Visual Studio lub z wiersza polecenia. Ta sekcja jest badana przy użyciu narzędzia z programu Visual Studio; sekcja "prekompilowanie z wiersza polecenia" szuka uruchomienia narzędzia kompilatora w wierszu polecenia.

Otwórz przegląd książki w programie Visual Studio, przejdź do menu Kompilacja i wybierz opcję menu Publikuj witrynę sieci Web. Spowoduje to uruchomienie okna dialogowego publikowanie witryny sieci Web (patrz **rysunek 1**), w którym można określić lokalizację docelową niezależnie od tego, czy interfejs użytkownika prekompilowanej lokacji ma być aktualizowalny, oraz inne opcje narzędzia kompilatora. Lokalizacją docelową może być zdalny serwer sieci Web lub serwer FTP, ale w przypadku wybrania teraz folderu na dysku twardym komputera. Ponieważ chcemy wstępnie skompilować lokację przy użyciu aktualizowalnego interfejsu użytkownika, pozostaw zaznaczone pole wyboru "Zezwalaj na aktualizowalną tę wstępnie skompilowaną witrynę", a następnie kliknij przycisk OK.

[![](precompiling-your-website-cs/_static/image2.png)](precompiling-your-website-cs/_static/image1.png)

**Rysunek 1**. narzędzie kompilacji ASP.NETą kompiluje swoją witrynę sieci Web do określonej lokalizacji docelowej  
 ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](precompiling-your-website-cs/_static/image3.png))

> [!NOTE]
> Opcja Publikuj witrynę sieci Web w menu Kompilacja jest niedostępna w programie Visual Web Developer. Jeśli używasz programu Visual Web Developer, musisz użyć wersji wiersza polecenia narzędzia kompilacji ASP.NET, która jest omówiona w sekcji "prekompilowanie z wiersza polecenia".

Po wstępnej kompilacji witryny sieci Web przejdź do lokalizacji docelowej wprowadzonej w oknie dialogowym Publikowanie witryny sieci Web. Poświęć chwilę, aby porównać zawartość tego folderu z zawartością witryny sieci Web. **Rysunek 2** przedstawia folder witryny sieci Web przeglądy książki. Należy zauważyć, że zawiera zarówno pliki `.aspx`, jak i `.aspx.cs`. Należy również pamiętać, że katalog `Bin` zawiera tylko jeden plik, `Elmah.dll`, który został dodany w [poprzednim samouczku](logging-error-details-with-elmah-cs.md)

[![](precompiling-your-website-cs/_static/image5.png)](precompiling-your-website-cs/_static/image4.png)

**Rysunek 2**. Katalog projektu zawiera pliki `.aspx` i `.aspx.cs`; Folder `Bin` zawiera tylko `Elmah.dll`  
 ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](precompiling-your-website-cs/_static/image6.png))

**Rysunek 3** przedstawia folder lokalizacji docelowej, którego zawartość została utworzona za pomocą narzędzia kompilacji ASP.NET. Ten folder nie zawiera żadnych plików związanych z kodem. Ponadto katalog `Bin` tego folderu zawiera kilka zestawów i dwa pliki `.compiled` oprócz zestawu `Elmah.dll`.

[![](precompiling-your-website-cs/_static/image8.png)](precompiling-your-website-cs/_static/image7.png)

**Rysunek 3**. folder lokalizacji docelowej obejmuje pliki do wdrożenia  
 ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](precompiling-your-website-cs/_static/image9.png))

W przeciwieństwie do kompilacji jawnej w WAPs, prekompilacja dla procesu wdrażania nie tworzy jednego zestawu dla całej lokacji. Zamiast tego przetwarza kilka stron do każdego zestawu. Kompiluje także plik `Global.asax` (jeśli istnieje) do własnego zestawu, a także wszystkich klas w folderze `App_Code`. Pliki przechowujące deklaratywne znaczniki dla ASP.NET stron sieci Web, kontrolek użytkownika i stron wzorcowych (odpowiednio`.aspx`, `.ascx`i `.master`) są kopiowane jako-do katalogu lokalizacji docelowej. Podobnie plik `Web.config` jest kopiowany bezpośrednio wraz ze wszystkimi plikami statycznymi, takimi jak obrazy, klasy CSS i pliki PDF. Aby uzyskać bardziej formalny opis sposobu obsługi różnych typów plików przez narzędzie kompilacji, zapoznaj się z tematem [Obsługa plików podczas wstępnej kompilacji ASP.NET](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> Możesz nakazać narzędziu do kompilowania tworzenie jednego zestawu na stronie ASP.NET, kontrolki użytkownika lub strony wzorcowej, zaznaczając pole wyboru "użyte stałe nazewnictwo i zestawy pojedynczej strony" w oknie dialogowym Publikowanie witryny sieci Web. Każda strona ASP.NET skompilowana do własnego zestawu umożliwia dokładniejszą kontrolę nad wdrażaniem. Na przykład jeśli zaktualizowano pojedynczą stronę sieci Web ASP.NET i jest ona potrzebna do wdrożenia tej zmiany, należy wdrożyć tylko ten plik `.aspx` i skojarzony zestaw w środowisku produkcyjnym. Aby uzyskać więcej informacji, zapoznaj [się z tematem: generowanie stałych nazw za pomocą narzędzia kompilacji ASP.NET](https://msdn.microsoft.com/library/ms228040.aspx) .

Docelowy katalog lokalizacji zawiera również plik, który nie jest częścią prekompilowanego projektu sieci Web, mianowicie `PrecompiledApp.config`. Ten plik informuje środowisko uruchomieniowe ASP.NET, że aplikacja została wstępnie skompilowana i czy została wstępnie skompilowana za pomocą aktualizowalnego lub w południe, aktualizowalnego interfejsu użytkownika.

Na koniec Poświęć chwilę na otwarcie jednego z plików `.aspx` w lokalizacji docelowej przy użyciu programu Visual Studio lub edytora tekstu. Podczas wstępnego kompilowania wdrożenia za pomocą aktualizowalnego interfejsu użytkownika strony ASP.NET w katalogu lokalizacji docelowej zawierają dokładnie te same znaczniki co odpowiednie pliki w witrynie sieci Web.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Prekompilowanie w celu wdrożenia z nieaktualizowalnym interfejsem użytkownika

Narzędzia kompilatora ASP.NET można także użyć do prekompilowania lokacji do wdrożenia z nieaktualizowalnym interfejsem użytkownika. Wstępne Kompilowanie lokacji z nieaktualizowalnym interfejsem użytkownika działa podobnie do prekompilowania z aktualizowalnym interfejsem użytkownika, ponieważ kluczową różnicą jest to, że strony ASP.NET, kontrolki użytkownika i strony wzorcowe w katalogu docelowym są usuwane z ich znaczników. Aby wstępnie skompilować witrynę sieci Web na potrzeby wdrożenia z nieaktualizowalnym interfejsem użytkownika, wybierz opcję Publikuj witrynę internetową z menu Kompilacja, a następnie usuń zaznaczenie pola wyboru Zezwalaj na aktualizowalną tę wstępnie skompilowaną witrynę (zobacz **rysunek 4**).

[![](precompiling-your-website-cs/_static/image11.png)](precompiling-your-website-cs/_static/image10.png)

**Ilustracja 4**. Usuń zaznaczenie opcji "Zezwalaj na aktualizowalną tę wstępnie skompilowaną witrynę, aby przeprowadzić kompilację z nieaktualizowalnym interfejsem użytkownika  
 ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](precompiling-your-website-cs/_static/image12.png))

**Rysunek 5** przedstawia folder lokalizacja docelowa po prekompilowaniu z nieaktualizowalnym interfejsem użytkownika.

[![](precompiling-your-website-cs/_static/image14.png)](precompiling-your-website-cs/_static/image13.png)

**Rysunek 5**. docelowy folder lokalizacji wdrożenia z nieaktualizowalnym interfejsem użytkownika  
 ([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](precompiling-your-website-cs/_static/image15.png))

Porównaj **rysunek 3** z **rysunkiem 5**. Chociaż dwa foldery mogą wyglądać identycznie, należy zauważyć, że folder nieaktualizowalnego interfejsu użytkownika nie ma strony głównej, `Site.master`. Natomiast **rysunek 5** zawiera różne strony ASP.NET, Jeśli zobaczysz zawartość tych plików, zobaczysz, że zostały one usunięte ze znaczników deklaratywnych i zastąpione tekstem zastępczym: "to jest plik znacznika wygenerowany przez narzędzie prekompilacji i nie należy go usuwać!"

[![](precompiling-your-website-cs/_static/image17.png)](precompiling-your-website-cs/_static/image16.png)

**Rysunek 5**. znaczniki deklaratywne zostały usunięte ze stron ASP.NET

Foldery `Bin` na **rysunkach 3** i **5** różnią się znacznie. Oprócz zestawów, folder `Bin` na **rysunku 5** zawiera plik `.compiled` dla każdej strony ASP.NET, kontrolki użytkownika i strony głównej.

Prekompilowanie witryny z nieaktualizowalnym interfejsem użytkownika jest przydatne w sytuacjach, gdy nie chcesz, aby zawartość ASP.NET stron była modyfikowana przez osobę lub firmę instalującą witrynę sieci Web w środowisku produkcyjnym. W przypadku tworzenia aplikacji sieci Web ASP.NET, która jest sprzedawana klientom do zainstalowania na własnych serwerach sieci Web, warto upewnić się, że nie modyfikują one wyglądu i sposobu działania witryny, bezpośrednio edytując `.aspx` strony, na których je wysyłasz. Wstępnie kompilując witrynę sieci Web z nieaktualizowalnym interfejsem użytkownika, możesz dostarczyć symbol zastępczy `.aspx` stronach w ramach instalacji, co uniemożliwi klientom badanie lub modyfikowanie zawartości.

### <a name="precompiling-from-the-command-line"></a>Prekompilowanie z wiersza polecenia

W tle okno dialogowe publikowanie witryny sieci Web programu Visual Studio wywołuje narzędzie kompilacji ASP.NET (`aspnet_compiler.exe`), aby wstępnie skompilować witrynę sieci Web. Alternatywnie można wywołać to narzędzie z wiersza polecenia. W przypadku korzystania z programu Visual Web Developer, należy uruchomić narzędzie kompilatora z wiersza polecenia, ponieważ menu Kompilacja w programie Visual Web Developer nie obejmuje opcji Publikuj witrynę sieci Web.

Aby użyć narzędzia kompilatora w wierszu polecenia, Zacznij od porzucenie do wiersza polecenia i przejściu do katalogu struktury, `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Następnie wprowadź następującą instrukcję w wierszu polecenia:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

Powyższe polecenie uruchamia narzędzie kompilatora ASP.NET (`aspnet_compiler.exe`) i za pośrednictwem przełącznika `-p` instruuje go do wstępnego skompilowania witryny sieci Web w *ścieżce\_fizycznej\_\_aplikacji*. Ta wartość będzie wyglądać podobnie jak `C:\MySites\BookReviews`i powinna być ujęta w cudzysłów.

Przełącznik `-v` określa katalog wirtualny witryny. Jeśli witryna jest zarejestrowana jako domyślna witryna sieci Web w metabazie usług IIS, możesz pominąć przełącznik `-p` i po prostu określić katalog wirtualny aplikacji. W przypadku korzystania z przełącznika `-p` wartość, która przejdzie do przełącznika `-v` wskazuje katalog główny witryny sieci Web i służy do rozpoznawania odwołań do katalogu głównego aplikacji. Na przykład jeśli określisz wartość `-v /MySite`, odwołania w aplikacji do `~/path/file` zostaną rozpoznane jako `~/MySite/path/file`. Ponieważ witryna przeglądy książek znajduje się w katalogu głównym w mojej firmie hostingu w sieci Web, a `-v /`przełącznika.

Przełącznik `-f`, jeśli jest obecny, nakazuje narzędziu kompilacji zastąpienie *docelowej lokalizacji\_\_katalogu folderu* , jeśli już istnieje. Jeśli pominięto przełącznik `-f` i folder lokalizacji docelowej już istnieje, narzędzie kompilacji zakończy działanie z powodu błędu: "Error ASPRUNTIME: katalog docelowy nie jest pusty. Usuń ją ręcznie lub wybierz inny element docelowy ".

Przełącznik `-u`, jeśli jest obecny, informuje narzędzie, aby utworzyć aktualizowalny interfejs użytkownika. Pomiń ten przełącznik, aby wstępnie skompilować lokację z nieaktualizowalnym interfejsem użytkownika.

Na koniec *folder docelowy lokalizacji\_\_* jest ścieżką fizyczną do katalogu lokalizacji docelowej; Ta wartość będzie wyglądać podobnie jak `C:\MySites\Output\BookReviews`i powinna być ujęta w cudzysłów.

## <a name="deploying-the-precompiled-website"></a>Wdrażanie wstępnie skompilowanej witryny sieci Web

W tym momencie dowiesz się, jak używać narzędzia kompilacji ASP.NET do wstępnego kompilowania witryny sieci Web przy użyciu opcji interfejsu użytkownika aktualizowalnych i nieaktualizowalnych. Jednak nasze przykłady z tego względu zostały wstępnie skompilowane w folderze lokalnym, a nie w środowisku produkcyjnym. Dobra wiadomość polega na tym, że wdrożenie prekompilowanej witryny sieci Web jest Breeze i może odbywać się za pośrednictwem programu Visual Studio lub innego mechanizmu kopiowania plików, na przykład z autonomicznego klienta FTP.

Okno dialogowe publikowanie witryny sieci Web (pierwsze pokazane na **rysunku 1**) ma opcję lokalizacji docelowej, która wskazuje, gdzie wstępnie skompilowane pliki witryny sieci Web są kopiowane do programu. Ta lokalizacja może być zdalnym serwerem sieci Web lub serwerem FTP. Wprowadzenie zdalnego serwera do tego pola tekstowego spowoduje wstępne skompilowanie i wdrożenie witryny sieci Web na określonym serwerze w jednym kroku. Alternatywnie można wstępnie skompilować witrynę sieci Web do folderu lokalnego, a następnie ręcznie skopiować zawartość tego folderu do środowiska produkcyjnego za pośrednictwem protokołu FTP lub innej metody.

Gdy wstępnie skompilowana witryna sieci Web jest automatycznie wdrażana za pośrednictwem okna dialogowego publikowanie witryny sieci Web programu Visual Studio, jest przydatne w przypadku prostych witryn, w których nie ma żadnych różnic w konfiguracji między środowiskami deweloperskim i produkcyjnym. Jednak zgodnie z [ *typowymi różnicami w konfiguracji między* samouczkiem dotyczącym tworzenia i produkcji](common-configuration-differences-between-development-and-production-cs.md) nie jest to rzadko spotykane w przypadku takich różnic. Na przykład aplikacja sieci Web przegląda książkę używa innej bazy danych w środowisku produkcyjnym niż w środowisku programistycznym. Gdy program Visual Studio opublikuje witrynę internetową na serwerze zdalnym, odślepe kopiuje informacje o pliku konfiguracji w środowisku deweloperskim.

W przypadku lokacji z różnicami w konfiguracji między środowiskami deweloperskimi i produkcyjnymi najlepszym rozwiązaniem może być wstępne skompilowanie lokacji do katalogu lokalnego, skopiowanie plików konfiguracyjnych specyficznych dla produkcji, a następnie skopiowanie zawartości wstępnie skompilowanych danych wyjściowych do narzędzi.

Odświeżacz dotyczący kopiowania plików ze środowiska deweloperskiego do środowiska produkcyjnego zapoznaj się z artykułem [*wdrażanie witryny sieci Web przy użyciu klienta FTP*](deploying-your-site-using-an-ftp-client-cs.md) i [*wdrażanie witryny sieci Web przy użyciu samouczków programu Visual Studio*](determining-what-files-need-to-be-deployed-cs.md) .

## <a name="summary"></a>Podsumowanie

ASP.NET obsługuje dwa tryby kompilacji: automatyczne i jawne. Zgodnie z opisem w poprzednich samouczkach projekty aplikacji sieci Web (WAPs) używają jawnej kompilacji, a domyślnie projekty witryn sieci Web (WSPs) używają automatycznej kompilacji. Jednak można jawnie skompilować program WSP przed wdrożeniem przy użyciu narzędzia do kompilacji ASP.NET.

Ten samouczek koncentruje się na wstępnej kompilacji narzędzia kompilacji na potrzeby obsługi wdrożenia. Podczas wstępnego kompilowania wdrożenia narzędzie kompilacji tworzy folder lokalizacji docelowej, kompiluje kod źródłowy aplikacji sieci Web i kopiuje te skompilowane zestawy oraz pliki zawartości do folderu lokalizacji docelowej. Narzędzie kompilacji można skonfigurować tak, aby utworzyć aktualizowalny lub nieaktualizowalny interfejs użytkownika. Podczas kompilowania z użyciem opcji nieaktualizowalnego interfejsu użytkownika, znaczniki deklaratywne w plikach zawartości są usuwane. W Nutshell wstępnej kompilacji pozwala na wdrożenie aplikacji opartej na projekcie witryny sieci Web bez uwzględniania plików kodu źródłowego i z usuniętym znacznikiem deklaratywnym, jeśli jest to konieczne.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Prekompilowanie witryny sieci Web ASP.NET](https://msdn.microsoft.com/library/ms228015.aspx)
- [CodeBehind i kompilacja w ASP.NET 2,0](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Prekompilacja w ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Wstępnie skompilowane opcje lokacji w ASP.NET](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Poprzednie](logging-error-details-with-elmah-cs.md)
> [dalej](users-and-roles-on-the-production-website-cs.md)

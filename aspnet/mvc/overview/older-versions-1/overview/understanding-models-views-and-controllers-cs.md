---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: Zrozumienie modeli, widoków i kontrolerów (C#) | Microsoft Docs
author: StephenWalther
description: Nie masz wątpliwości dotyczących modeli, widoków i kontrolerów? W tym samouczku Stephen Walther przedstawia różne części aplikacji ASP.NET MVC.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: 57dc82d02d38adc2514aa2c02c6f156ed0fb88a6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600591"
---
# <a name="understanding-models-views-and-controllers-c"></a>Objaśnienie modeli, widoków i kontrolerów (C#)

Autor [Stephen Walther](https://github.com/StephenWalther)

> Nie masz wątpliwości dotyczących modeli, widoków i kontrolerów? W tym samouczku Stephen Walther przedstawia różne części aplikacji ASP.NET MVC.

Ten samouczek zawiera ogólne omówienie modeli, widoków i kontrolerów MVC ASP.NET. Inaczej mówiąc, objaśnia to M ", V" i C "w ASP.NET MVC.

Po przeczytaniu tego samouczka należy zrozumieć, w jaki sposób różne części aplikacji ASP.NET MVC współpracują ze sobą. Należy również zrozumieć, jak architektura aplikacji ASP.NET MVC różni się od aplikacji ASP.NET Web Forms lub aplikacji Active Server Pages.

## <a name="the-sample-aspnet-mvc-application"></a>Przykładowa aplikacja MVC ASP.NET

Domyślny szablon programu Visual Studio służący do tworzenia aplikacji sieci Web ASP.NET MVC zawiera bardzo prostą przykładową aplikację, której można użyć do zrozumienia różnych części aplikacji ASP.NET MVC. Korzystamy z tej prostej aplikacji w tym samouczku.

Tworzenie nowej aplikacji ASP.NET MVC z szablonem MVC przez uruchomienie programu Visual Studio 2008 i wybranie pliku opcji menu, nowy projekt (patrz rysunek 1). W oknie dialogowym Nowy projekt Wybierz ulubiony język programowania w obszarze typy projektu (Visual Basic lub C#), a następnie wybierz pozycję **aplikacja sieci Web ASP.NET MVC** w obszarze Szablony. Kliknij przycisk OK.

[Okno dialogowe ![nowego projektu](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)

**Ilustracja 01**. okno dialogowe Nowy projekt ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-models-views-and-controllers-cs/_static/image2.png))

Podczas tworzenia nowej aplikacji ASP.NET MVC zostanie wyświetlone okno dialogowe **Tworzenie projektu testu jednostkowego** (patrz rysunek 2). To okno dialogowe pozwala utworzyć oddzielny projekt w rozwiązaniu do testowania aplikacji ASP.NET MVC. Wybierz opcję **nie, nie twórz projektu testów jednostkowych** i kliknij przycisk **OK** .

[Okno dialogowe Tworzenie testu jednostkowego ![](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)

**Ilustracja 02**. okno dialogowe Tworzenie testu jednostkowego ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-models-views-and-controllers-cs/_static/image4.png))

Po utworzeniu nowej aplikacji ASP.NET MVC. W oknie Eksplorator rozwiązań zobaczysz kilka folderów i plików. W szczególności zobaczysz trzy foldery o nazwie modele, widoki i kontrolery. Ponieważ nazwy folderów mogą być odgadnąć, te foldery zawierają pliki do implementowania modeli, widoków i kontrolerów.

Po rozszerzeniu folderu controllers powinien zostać wyświetlony plik o nazwie AccountController.cs i plik o nazwie HomeController.cs. W przypadku rozszerzenia folderu widoki powinny zostać wyświetlone trzy podfoldery o nazwie Account, Home i Shared. Po rozszerzeniu folderu macierzystego zobaczysz dwa dodatkowe pliki o nazwie about. aspx i index. aspx (patrz rysunek 3). Te pliki składają się z przykładowej aplikacji dołączonej do domyślnego szablonu ASP.NET MVC.

[![okno Eksplorator rozwiązań](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)

**Ilustracja 03**: okno Eksplorator rozwiązań ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-models-views-and-controllers-cs/_static/image6.png))

Możesz uruchomić przykładową aplikację, wybierając opcję menu **Debuguj, Rozpocznij debugowanie**. Alternatywnie możesz nacisnąć klawisz F5.

Po pierwszym uruchomieniu aplikacji ASP.NET zostanie wyświetlone okno dialogowe z rysunkiem 4, które zaleca włączenie trybu debugowania. Kliknij przycisk OK, a aplikacja zostanie uruchomiona.

[okno dialogowe niewłączone debugowanie ![](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)

**Rysunek 04**: okno dialogowe debugowanie nie jest włączone ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-models-views-and-controllers-cs/_static/image8.png))

Po uruchomieniu aplikacji ASP.NET MVC program Visual Studio uruchamia aplikację w przeglądarce sieci Web. Przykładowa aplikacja składa się tylko z dwóch stron: strony indeks oraz strony informacje. Po pierwszym uruchomieniu aplikacji zostanie wyświetlona strona indeks (patrz rysunek 5). Możesz przejść do strony informacje, klikając łącze menu w prawym górnym rogu aplikacji.

[![strony indeksu](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)

**Ilustracja 05**: Strona indeks ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-cs/_static/image11.png))

Zwróć uwagę na adresy URL na pasku adresu przeglądarki. Na przykład po kliknięciu łącza do menu informacje adres URL na pasku adresu przeglądarki zmieni się na **/Home/about**.

Jeśli zamkniesz okno przeglądarki i wrócisz do programu Visual Studio, nie będzie można znaleźć pliku z ścieżką Home/about. Pliki nie istnieją. Jak to możliwe?

## <a name="a-url-does-not-equal-a-page"></a>Adres URL nie jest równy stronie

Podczas kompilowania tradycyjnej aplikacji ASP.NET Web Forms lub aplikacji Active Server Pages istnieje taka sama zgodność między adresem URL a stroną. Jeśli zażądasz strony o nazwie SomePage. aspx z serwera, na dysku o nazwie SomePage. aspx powinna być lepsza strona. Jeśli plik SomePage. aspx nie istnieje, otrzymasz komunikat o błędzie nieładnego **404 — nie znaleziono strony** .

W przypadku kompilowania aplikacji ASP.NET MVC nie ma żadnej zgodności między adresem URL wpisanym na pasku adresu przeglądarki a plikami, które znajdują się w aplikacji. W aplikacji ASP.NET MVC adres URL odpowiada akcji kontrolera zamiast strony na dysku.

W tradycyjnej aplikacji ASP.NET lub ASP żądania przeglądarki są mapowane na strony. W aplikacji ASP.NET MVC z kolei żądania przeglądarki są mapowane na akcje kontrolera. Aplikacja formularzy sieci Web ASP.NET jest skoncentrowana na zawartości. Aplikacja ASP.NET MVC, w przeciwieństwie, jest skoncentrowana na logice aplikacji.

## <a name="understanding-aspnet-routing"></a>Zrozumienie routingu ASP.NET

Żądanie przeglądarki jest zamapowane na akcję kontrolera przez funkcję ASP.NET Framework o nazwie *ASP.NET routing*. Routing ASP.NET jest używany przez strukturę ASP.NET MVC do *kierowania* żądań przychodzących do akcji kontrolera.

Routing ASP.NET używa tabeli tras do obsługi żądań przychodzących. Ta tabela tras jest tworzona podczas pierwszego uruchomienia aplikacji sieci Web. Tabela tras jest konfiguracją w pliku Global. asax. Domyślny plik Global. asax MVC jest zawarty w liście 1.

**Lista 1 — Global. asax**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

Po pierwszym uruchomieniu aplikacji ASP.NET zostaje wywołana metoda Start ()\_aplikacji. W przypadku list 1 Metoda ta wywołuje metodę RegisterRoutes (), a metoda RegisterRoutes () tworzy domyślną tabelę tras.

Domyślna tabela tras składa się z jednej trasy. Ta trasa domyślna przerywa wszystkie żądania przychodzące do trzech segmentów (segment adresu URL zawiera cokolwiek między ukośnikami). Pierwszy segment jest mapowany na nazwę kontrolera, drugi segment jest mapowany na nazwę akcji, a końcowy segment jest mapowany do parametru przesłanego do akcji o nazwie ID.

Rozważmy na przykład następujący adres URL:

/Product/Details/3

Ten adres URL jest analizowany do trzech parametrów, takich jak:

Kontroler = produkt

Akcja = szczegóły

Identyfikator = 3

Trasa domyślna zdefiniowana w pliku Global. asax zawiera wartości domyślne dla wszystkich trzech parametrów. Domyślnym kontrolerem jest Strona główna, domyślna akcja to indeks, a domyślny identyfikator jest ciągiem pustym. Z tymi wartościami domyślnymi należy wziąć pod uwagę sposób analizowania następującego adresu URL:

/Employee

Ten adres URL jest analizowany do trzech parametrów, takich jak:

Kontroler = pracownik

Akcja = indeks

Identyfikator =

Na koniec, jeśli otworzysz aplikację ASP.NET MVC bez podawania żadnego adresu URL (na przykład `http://localhost`), adres URL jest analizowany w następujący sposób:

Kontroler = Strona główna

Akcja = indeks

Identyfikator =

Żądanie jest kierowane do akcji index () w klasie HomeController.

## <a name="understanding-controllers"></a>Omówienie kontrolerów

Kontroler jest odpowiedzialny za kontrolowanie sposobu, w jaki użytkownik współdziała z aplikacją MVC. Kontroler zawiera logikę sterowania przepływem dla aplikacji ASP.NET MVC. Kontroler określa, która odpowiedź ma zostać wysłana z powrotem do użytkownika, gdy użytkownik wykonuje żądanie przeglądarki.

Kontroler jest tylko klasą (na przykład Visual Basic lub C# klasy). Przykładowa aplikacja ASP.NET MVC obejmuje kontroler o nazwie HomeController.cs znajdujący się w folderze controllers. Zawartość pliku HomeController.cs zostanie wyświetlona na liście 2.

**Lista 2 — HomeController.cs**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

Należy zauważyć, że HomeController ma dwie metody o nazwie index () i About (). Te dwie metody odpowiadają dwóm akcjom uwidocznionym przez kontroler. /Home/Index URL wywołuje metodę HomeController. index (), a adres URL/Home/About wywołuje metodę HomeController. about ().

Każda metoda publiczna w kontrolerze jest udostępniana jako akcja kontrolera. Należy zachować ostrożność. Oznacza to, że każda metoda publiczna znajdująca się w kontrolerze może być wywoływana przez każdą osobę mającą dostęp do Internetu, wprowadzając prawidłowy adres URL do przeglądarki.

## <a name="understanding-views"></a>Informacje o widokach

Dwa akcje kontrolera uwidocznione przez klasy HomeController, index () i About () zwracają widok. Widok zawiera oznaczenie HTML i zawartość, która jest wysyłana do przeglądarki. Widok jest odpowiednikiem strony podczas pracy z aplikacją ASP.NET MVC.

Musisz utworzyć widoki w odpowiedniej lokalizacji. Akcja HomeController. index () zwraca widok znajdujący się w następującej ścieżce:

\Views\Home\Index.aspx

Akcja HomeController. about () zwraca widok znajdujący się w następującej ścieżce:

\Views\Home\About.aspx

Ogólnie rzecz biorąc, jeśli chcesz zwrócić widok dla akcji kontrolera, należy utworzyć podfolder w folderze widoki o takiej samej nazwie jak kontroler. W podfolderze należy utworzyć plik. aspx o tej samej nazwie co akcja kontrolera.

Plik w liście 3 zawiera widok informacje o. aspx.

**Lista 3 — informacje. aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

Jeśli zignorujesz pierwszy wiersz na liście 3, większość reszty widoku składa się z standardowego kodu HTML. Zawartość widoku można zmodyfikować, wprowadzając kod HTML, który chcesz umieścić w tym miejscu.

Widok jest bardzo podobny do strony w Active Server stronach lub formularzach sieci Web ASP.NET. Widok może zawierać zawartość HTML i skrypty. Skrypty można napisać w ulubionym języku programowania .NET (na przykład C# lub w Visual Basic .NET). Za pomocą skryptów można wyświetlać zawartość dynamiczną, taką jak dane bazy danych.

## <a name="understanding-models"></a>Informacje o modelach

Omawiamy kontrolery i omawiamy widoki. Ostatnim tematem, który musimy omówić, są modele. Co to jest model MVC?

Model MVC zawiera całą logikę aplikacji, która nie jest zawarta w widoku lub kontrolerze. Model powinien zawierać wszystkie aplikacje logiki biznesowej, logiki walidacji i logiki dostępu do bazy danych. Na przykład jeśli używasz Entity Framework firmy Microsoft w celu uzyskania dostępu do bazy danych, utworzysz klasy Entity Framework (plik. edmx) w folderze modele.

Widok powinien zawierać tylko logikę powiązaną z generowaniem interfejsu użytkownika. Kontroler powinien zawierać tylko wartość minimum od zera wymaganą do zwrócenia odpowiedniego widoku lub przekierować użytkownika do innej akcji (sterowanie przepływem). Wszystkie inne elementy powinny być zawarte w modelu.

Ogólnie rzecz biorąc, należy dążyć do modeli FAT i kontrolerów Skinny. Metody kontrolera powinny zawierać tylko kilka wierszy kodu. Jeśli akcja kontrolera jest zbyt za tłuszczem, należy rozważyć przeniesienie logiki do nowej klasy w folderze modele.

## <a name="summary"></a>Podsumowanie

Ten samouczek zapewnia ogólne omówienie różnych części aplikacji sieci Web ASP.NET MVC. Wiesz już, jak ASP.NET routing mapuje przychodzące żądania przeglądarki do określonych akcji kontrolera. Wiesz już, jak kontrolery organizują sposób zwrócenia widoków do przeglądarki. Na koniec przedstawiono sposób, w jaki modele zawierają logikę dostępu do bazy danych i aplikacji.

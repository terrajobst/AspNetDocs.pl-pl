---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: Objaśnienie modeli, widoków i kontrolerów (C#) | Dokumentacja firmy Microsoft
author: StephenWalther
description: 'Masz wątpliwości dotyczące modeli, widoków i kontrolerów? W tym samouczku Walther Autor: Stephen poznasz różne części aplikacji ASP.NET MVC.'
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: 57dc82d02d38adc2514aa2c02c6f156ed0fb88a6
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65122054"
---
# <a name="understanding-models-views-and-controllers-c"></a>Objaśnienie modeli, widoków i kontrolerów (C#)

przez [Walther Autor: Stephen](https://github.com/StephenWalther)

> Masz wątpliwości dotyczące modeli, widoków i kontrolerów? W tym samouczku Walther Autor: Stephen poznasz różne części aplikacji ASP.NET MVC.

Ten samouczek umożliwia ogólne omówienie platformy ASP.NET MVC modeli, widoków i kontrolerów. Innymi słowy, wyjaśniono M ", V", a C "we wzorcu ASP.NET MVC.

Po przeczytaniu tego samouczka, należy zrozumieć, jak różne części aplikacji ASP.NET MVC współpracują ze sobą. Należy również zrozumieć, jak Architektura aplikacji ASP.NET MVC różni się od aplikacji formularzy sieci Web ASP.NET lub aplikacji Active Server Pages.

## <a name="the-sample-aspnet-mvc-application"></a>Przykładowa aplikacja platformy ASP.NET MVC

Domyślny szablon programu Visual Studio do tworzenia aplikacji sieci Web programu ASP.NET MVC zawiera bardzo prostą przykładowej aplikacji, która może służyć do zrozumienia różne części aplikacji ASP.NET MVC. Firma Microsoft może korzystać z tej prostej aplikacji, w tym samouczku.

Tworzenie nowej aplikacji platformy ASP.NET MVC za pomocą szablonu MVC, uruchamiając program Visual Studio 2008 i wybranie opcji menu Plik, nowy projekt (patrz rysunek 1). W oknie dialogowym Nowy projekt, wybierz ulubionym języku programowania w ramach typów projektu (Visual Basic lub C#) i ustaw **aplikacji sieci Web programu ASP.NET MVC** w obszarze Szablony. Kliknij przycisk OK.

[![Okno dialogowe nowego projektu](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)

**Rysunek 01**: Okno dialogowe nowego projektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-cs/_static/image2.png))

Podczas tworzenia nowej aplikacji platformy ASP.NET MVC, **Tworzenie projektu testu jednostkowego** zostanie wyświetlone okno dialogowe (patrz rysunek 2). To okno dialogowe umożliwia utworzenie oddzielnego projektu w rozwiązaniu do testowania aplikacji ASP.NET MVC. Wybierz opcję **nie twórz projektu testu jednostkowego** i kliknij przycisk **OK** przycisku.

[![Tworzenie testów jednostkowych w oknie dialogowym](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)

**Rysunek 02**: Tworzenie okna dialogowego testów jednostkowych ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-cs/_static/image4.png))

Aplikacja zostanie utworzona po nowe platformy ASP.NET MVC. Pojawi się kilka folderów i plików w oknie Eksploratora rozwiązań. W szczególności zostaną wyświetlone trzy foldery o nazwie modeli, widoków i kontrolerów. Jak może odgadnięcia nazw folderów te foldery zawierają pliki dotyczące wdrażania modeli, widoków i kontrolerów.

Po rozwinięciu folderze kontrolery powinien zostać wyświetlony plik o nazwie AccountController.cs i plik o nazwie HomeController.cs. Po rozwinięciu folderze Widoki powinny być widoczne trzy podfoldery o nazwie konto Home i udostępnione. Po rozwinięciu folderu macierzystego zobaczysz dwa dodatkowe pliki o nazwie About.aspx i Index.aspx (zobacz rysunek 3). Te pliki składają się przykładowa aplikacja uwzględniony przy użyciu domyślnego szablonu platformy ASP.NET MVC.

[![Okno Eksploratora rozwiązań](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)

**Rysunek 03**: Okno Eksploratora rozwiązań ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-cs/_static/image6.png))

Można uruchomić przykładową aplikację, wybierając opcję menu **debugowania i Rozpocznij debugowanie**. Alternatywnie można naciśnij klawisz F5.

Przy pierwszym uruchomieniu aplikacji ASP.NET, zostanie wyświetlone okno dialogowe na rysunku 4 zaleca, aby włączyć tryb debugowania. Kliknij przycisk OK, a aplikacja zostanie uruchomiona.

[![Debugowanie nie jest włączone okna dialogowego](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)

**Rysunek 04**: Debugowanie nie jest włączone okna dialogowego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-cs/_static/image8.png))

Podczas uruchamiania aplikacji ASP.NET MVC programu Visual Studio uruchamia aplikację w przeglądarce sieci web. Przykładowa aplikacja składa się z tylko dwoma stronami: strony indeksu i na stronie informacje. Po pierwszym uruchomieniu aplikacji zostanie wyświetlona strona indeksu, (zobacz rysunek 5). Można przejść do strony informacje, klikając link menu w prawym górnym rogu aplikacji.

[![Strony indeksu](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)

**Rysunek 05**: Na stronie indeksu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](understanding-models-views-and-controllers-cs/_static/image11.png))

Zwróć uwagę, adresy URL w pasku adresu przeglądarki. Na przykład po kliknięciu łącza menu informacje, adres URL w pasku adresu przeglądarki zmienia się **/Home/About**.

Jeśli Zamknij okno przeglądarki i wróć do programu Visual Studio, nie można odnaleźć pliku przy użyciu główna ścieżka/informacje. Pliki nie istnieją. Jak to możliwe?

## <a name="a-url-does-not-equal-a-page"></a>Adres URL strony nie równa się

W przypadku tworzenia tradycyjnych aplikacji formularzy sieci Web ASP.NET lub aplikacji Active Server Pages, ma relację między adresu URL i strony. Jeśli zażądasz stronę o nazwie SomePage.aspx z serwera, następnie miał lepiej istnieć strony na dysku o nazwie SomePage.aspx. Jeśli plik SomePage.aspx nie istnieje, otrzymasz fałszywych **404 — Nie można odnaleźć strony** błędu.

Podczas tworzenia aplikacji ASP.NET MVC, z kolei brak zgodności między adres URL, który można wpisać w pasku adresu przeglądarki i pliki, które możesz znaleźć w aplikacji. Adres URL w aplikacji ASP.NET MVC, odnosi się do akcji kontrolera, zamiast strony na dysku.

W tradycyjnych aplikacji ASP.NET i ASP żądania przeglądarki są mapowane na stronach. W aplikacji ASP.NET MVC, natomiast w przeglądarce żądania są mapowane do akcji kontrolera. Aplikacja ASP.NET Web Forms jest skoncentrowane na zawartość. Aplikacji ASP.NET MVC, z kolei jest skoncentrowane na logice aplikacji.

## <a name="understanding-aspnet-routing"></a>Opis routingu platformy ASP.NET

Żądanie przeglądarki na pobiera mapowany do akcji kontrolera za pośrednictwem funkcji struktury programu ASP.NET o nazwie *routingu platformy ASP.NET*. Routingu platformy ASP.NET jest używany przez platformę ASP.NET MVC w celu *trasy* żądania przychodzące do akcji kontrolera.

Routingu platformy ASP.NET używa tabeli tras do obsługi żądań przychodzących. Ta tabela tras jest tworzony podczas pierwszego uruchomienia aplikacji sieci web. Tabela tras jest skonfigurowana w pliku Global.asax. Domyślny plik Global.asax platformy MVC znajduje się w ofercie 1.

**Wyświetlanie listy 1 — w pliku Global.asax**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

Gdy aplikacja ASP.NET pierwszym uruchomieniu, aplikacja\_wywołanie metody Start(). W ofercie 1 Ta metoda wywołuje metodę RegisterRoutes() i metoda RegisterRoutes() tworzy tabela routingu domyślnego.

Tabela routingu domyślnego obejmuje jedną trasę. Ta trasa domyślna przerywa wszystkie żądania przychodzące na trzy segmenty (segment adresu URL to wszystko między ukośniki). Pierwszy segment jest mapowany na nazwę kontrolera, drugi segment jest mapowany na nazwę akcji i końcowego segmentu jest mapowany na parametr przekazywany do akcji o nazwie identyfikatora.

Na przykład rozważmy następujący adres URL:

/Product/Details/3

Ten adres URL jest przekształcany do trzech parametrów następująco:

Kontroler = produkt

Akcja = szczegóły

Identyfikator = 3

Trasa domyślna zdefiniowane w pliku Global.asax zawiera wartości domyślne dla wszystkich trzech parametrów. Domyślnie kontrolera jest strona główna, domyślna akcja jest indeksem i domyślny identyfikator jest ciągiem pustym. W tych wartości domyślnych, pamiętając weź pod uwagę sposób następujący adres URL jest analizowany:

/Employee

Ten adres URL jest przekształcany do trzech parametrów następująco:

Kontroler = pracownik

Akcja = indeks

Id = ��

Na koniec możesz otworzyć aplikację ASP.NET MVC bez podawania dowolnego adresu URL (na przykład `http://localhost`), a następnie adres URL jest analizowany w następujący sposób:

Kontroler = strona główna

Akcja = indeks

Id = ��

Żądanie jest kierowane do akcji indeks() klasy HomeController.

## <a name="understanding-controllers"></a>Opis kontrolerów

Kontroler jest odpowiedzialny za kontrolowanie sposobu, w jaki użytkownik wchodzi w interakcję z aplikacją MVC. Kontroler zawiera logikę kontroli przepływu dla aplikacji ASP.NET MVC. Kontroler Określa, jakie odpowiedzi do odesłania do użytkownika, gdy użytkownik wysyła żądanie przeglądarki.

Kontroler to po prostu klasy (na przykład klasy Visual Basic lub C#). Przykład aplikacji platformy ASP.NET MVC obejmuje kontroler o nazwie HomeController.cs znajduje się w folderze kontrolerów. Zawartość pliku HomeController.cs jest przedstawiony w ofercie 2.

**Wyświetlanie listy 2 - HomeController.cs**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

Należy zauważyć, że HomeController ma dwie metody o nazwie indeks() i About(). Te dwie metody odpowiadają dwie akcje udostępnianych przez kontrolera. Adres URL /Home/Index wywołuje metodę HomeController.Index() i /Home adresu URL/o wywołuje metodę HomeController.About().

Wszystkie metody publicznej w kontrolerze jest ujawniona jako akcji kontrolera. Należy zachować ostrożność to. Oznacza to, że dowolnej metody publiczne, zawarte w kontrolerze może być wywoływany przez każdy z dostępem do Internetu, wprowadzając właściwego adresu URL do przeglądarki.

## <a name="understanding-views"></a>Objaśnienie widoków

Akcje kontrolera dwóch udostępnianych przez klasę HomeController indeks() i About(), oba zwracają widoku. Widok zawiera kod znaczników HTML i zawartości, który jest wysyłany do przeglądarki. Widok jest odpowiednikiem stroną, podczas pracy z aplikacją ASP.NET MVC.

Należy utworzyć widoków we właściwym miejscu. Akcja HomeController.Index() zwraca Widok znajduje się w następującej ścieżce:

\Views\Home\Index.aspx

Akcja HomeController.About() zwraca Widok znajduje się w następującej ścieżce:

\Views\Home\About.aspx

Ogólnie rzecz biorąc Jeśli chcesz powrócić do widoku dla akcji kontrolera, następnie należy utworzyć podfolder w folderze widoków z taką samą nazwę jak kontroler. W ramach tego podfolderu należy utworzyć plik .aspx z taką samą nazwę jak akcji kontrolera.

Plik w ofercie 3 zawiera widok About.aspx.

**Wyświetlanie listy 3 - About.aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

Jeśli zignorujesz pierwszy wiersz w ofercie 3, większość pozostałych Widok składa się z standardowego kodu HTML. Możesz zmodyfikować zawartość widoku, wprowadzając kod HTML, chcesz, aby w tym miejscu.

Widok jest bardzo podobne do strony Active Server Pages lub formularzy sieci Web ASP.NET. Widok może zawierać zawartość HTML i skryptów. Skrypty można pisać w ulubionej platformy .NET, Programowanie w języku (na przykład C# lub Visual Basic .NET). Skrypty służy do wyświetlania zawartości dynamicznej, takich jak dane z bazy danych.

## <a name="understanding-models"></a>Objaśnienie modeli

Omówiliśmy kontrolery i Omówiliśmy widoków. Ostatni temat, którego potrzebujemy do omówienia jest modeli. Co to jest model MVC?

Modelu MVC zawiera wszystkie logika aplikacji, który nie jest zawarty w widoku lub kontrolera. Model powinien zawierać wszystkie aplikacji logiki biznesowej, logikę weryfikacji i logiką dostępu do bazy danych. Na przykład jeśli używasz programu Entity Framework Microsoft dostęp do bazy danych, następnie w należy utworzyć Twoich zajęciach Entity Framework (pliku edmx) folderu modeli.

Widok może zawierać tylko logiki związane z generowaniem interfejsu użytkownika. Kontroler powinien zawierać tylko podstawowe czynności logikę wymaganą do zwrotu prawego widoku lub przekierować użytkownika do innej akcji (Sterowanie przepływem). Wszystkie inne elementy powinny być zawarte w modelu.

Ogólnie rzecz biorąc należy dążyć dla modeli fat i szczupły kontrolerów. Metody kontrolera może zawierać tylko kilka wierszy kodu. Jeśli akcja kontrolera pobiera zbyt fat, następnie należy rozważyć przenoszenia logikę do nowej klasy, w tym folderze modeli.

## <a name="summary"></a>Podsumowanie

W tym samouczku został udostępniony Przegląd wysokiego poziomu różnych części platformy ASP.NET MVC aplikacji sieci web. Przedstawiono sposób routingu platformy ASP.NET mapowania przychodzących żądań przeglądarki określony kontroler akcji. Pokazaliśmy ci, jak kontrolery organizowanie, jak widoki są zwracane do przeglądarki. Na koniec pokazaliśmy ci, jak modele zawierają aplikacji biznesowych, weryfikacji i logiką dostępu do bazy danych.

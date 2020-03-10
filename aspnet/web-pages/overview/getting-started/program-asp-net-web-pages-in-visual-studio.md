---
uid: aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
title: Programowanie ASP.NET stron sieci Web (Razor) przy użyciu programu Visual Studio | Microsoft Docs
author: Rick-Anderson
description: W tym dodatku wyjaśniono, jak można użyć programu Visual Studio 2010 lub Visual Web Developer 2010 Express do programu ASP.NET Web Pages with the składnia Razor.
ms.author: riande
ms.date: 02/13/2014
ms.assetid: 0acfec5a-48f2-4766-a801-a0f426966f0a
msc.legacyurl: /web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: 1a76098779d05912bf7bdf2de5fdce024770752c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633512"
---
# <a name="programming-aspnet-web-pages-razor-using-visual-studio"></a>Programowanie ASP.NET stron sieci Web (Razor) przy użyciu programu Visual Studio

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak można użyć programu Visual Studio lub Visual Web Developer Express do programu program ASP.NET Web Pages (Razor) websites.
>
> Zawartość
>
> - Co jest potrzebne do pracy z ASP.NET stronami sieci Web w używanej wersji programu Visual Studio.
> - Jak dodać obsługę stron sieci Web ASP.NET do programu Visual Web Developer 2010 Express.
> - Jak używać funkcji programu Visual Studio do pracy ze stronami ASP.NET Razor, w tym IntelliSense i debugerem.
>
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
>
> - ASP.NET strony sieci Web (Razor) 3
> - Program Visual Studio 2013
> - WebMatrix 3
>
>
> Ten samouczek działa również z ASP.NET Web Pages 2, Visual Studio 2012, Visual Studio 2010 i WebMatrix 2.

Można programować ASP.NET składnia Razor stron sieci Web za pomocą programu WebMatrix lub wielu innych edytorów kodu. Możesz również użyć Microsoft Visual Studio, który jest w pełni funkcjonalnym zintegrowanym środowiskiem programistycznym (IDE), który oferuje zaawansowany zestaw narzędzi do tworzenia wielu typów aplikacji (a nie tylko witryn sieci Web). Aby współpracować ze stronami ASP.NET Razor, możesz użyć [programu Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).

Dwie szczególnie przydatne funkcje, które program Visual Studio oferuje do programowania przy użyciu stron sieci Web ASP.NET Razor:

- Funkcja *IntelliSense*. Funkcja IntelliSense wbudowana w program Visual Studio jest bardziej kompleksowa niż technologia IntelliSense w programie WebMatrix.
- *Debuger*. Debuger umożliwia rozwiązywanie problemów z kodem przez zatrzymywanie programu, gdy jest on uruchomiony, badanie zmiennych i przechodzenie do kolejnych wierszy kodu.

## <a name="using-visual-studio-with-different-versions-of-aspnet-web-pages"></a>Korzystanie z programu Visual Studio z różnymi wersjami stron sieci Web ASP.NET

Aby opracowywać aplikacje sieci Web ASP.NET w programie Visual Studio 2017, zainstaluj obciążenie narzędzia **ASP.NET i programowanie dla sieci Web** .

Program Visual Studio 2012 i Visual Studio 2013 obejmują obsługę stron sieci Web ASP.NET. (Pakiety wymagane w celu obsługi stron sieci Web ASP.NET są instalowane podczas instalowania programu Visual Studio).

Program Visual Studio 2010 nie obejmuje domyślnie obsługi stron sieci Web ASP.NET. Aby korzystać ze stron sieci Web ASP.NET w programie Visual Studio 2010, należy zainstalować pakiet ASP.NET MVC. Aby uzyskać ASP.NET Web Pages 2, zainstaluj ASP.NET MVC 4.

Poniższa tabela zawiera podsumowanie obsługi stron sieci Web ASP.NET w różnych wersjach programu Visual Studio.

|  | Visual Studio 2010 | Visual Studio 2012 | Program Visual Studio 2013 |
| --- | --- | --- | --- |
| **ASP.NET Web Pages 2** | Zainstaluj ASP.NET MVC 4 | Uwzględnione | Uwzględnione |
| **ASP.NET Web Pages 3** |  | Aktualizacja programu ASP.NET Web Pages 3 za poorednictwem narzędzia NuGet | Uwzględnione |

Aby współpracować z programem Visual Studio 2010, zobacz [Instalowanie obsługi stron sieci Web ASP.NET w programie Visual Studio 2010](#vs2010support).

## <a name="launching-visual-studio-from-webmatrix"></a>Uruchamianie programu Visual Studio z programu WebMatrix

Jeśli rozpoczęto projekt w programie WebMatrix i chcesz przełączyć się do programu Visual Studio, Program WebMatrix udostępnia przycisk umożliwiający łatwe otwieranie projektu w programie Visual Studio. Aby ten przycisk został włączony, na komputerze musi być zainstalowany program Visual Studio. Na poniższej ilustracji przedstawiono przycisk w programie WebMatrix.

![Uruchom program Visual Studio](program-asp-net-web-pages-in-visual-studio/_static/image1.png)

Po kliknięciu przycisku projekt zostanie otwarty w programie Visual Studio. Możesz przełączać się między programem WebMatrix i programem Visual Studio bez żadnych problemów. Użytkownik zostanie powiadomiony, czy jakiekolwiek pliki uległy zmianie w innym środowisku i muszą zostać ponownie załadowane w celu uzyskania najnowszych zmian.

## <a name="creating-aspnet-razor-site-in-visual-studio"></a>Tworzenie witryny ASP.NET Razor w programie Visual Studio

Aby utworzyć witrynę sieci Web ASP.NET Razor w programie Visual Studio:

1. Otwórz program Visual Studio.
2. W menu **plik** kliknij pozycję **Nowa witryna sieci Web**.

    ![Utwórz nową witrynę sieci Web](program-asp-net-web-pages-in-visual-studio/_static/image2.png)
3. W **nowej witrynie sieci Web** okno dialogowe, wybierz język, który ma być używany C# (Visual lub Visual Basic).
4. Wybierz szablon **witryny sieci Web ASP.NET (Razor)** .

    ![Witryna Razor](program-asp-net-web-pages-in-visual-studio/_static/image3.png)
5. Kliknij przycisk **OK**.

Nowy projekt istnieje i zawiera kilka domyślnych stron sieci Web, które ułatwiają rozpoczęcie pracy.

### <a name="using-intellisense"></a>Korzystanie z IntelliSense

Po utworzeniu lokacji można zobaczyć, jak działa technologia IntelliSense w programie Visual Studio.

1. W właśnie utworzonej witrynie sieci Web otwórz stronę *default. cshtml* .
2. Po `<h3>` tagów na stronie wpisz `@ServerInfo.` (łącznie z kropką). Zauważ, że technologia IntelliSense wyświetla dostępne metody pomocnika `ServerInfo` na liście rozwijanej.

    ![Technologia](program-asp-net-web-pages-in-visual-studio/_static/image4.png)
3. Wybierz z listy metodę `GetHtml`, a następnie naciśnij klawisz ENTER. Funkcja IntelliSense automatycznie wypełnia metodę. (Podobnie jak w przypadku dowolnej C#metody w, należy dodać `()` znaków po metodzie). Ukończony kod metody `GetHtml` wygląda następująco:

    [!code-cshtml[Main](program-asp-net-web-pages-in-visual-studio/samples/sample1.cshtml)]
4. Naciśnij klawisze CTRL + F5, aby uruchomić stronę. Oto jak wygląda strona, gdy jest wyświetlana w przeglądarce:

    ![Strona domyślna w przeglądarce](program-asp-net-web-pages-in-visual-studio/_static/image5.png)
5. Zamknij okno przeglądarki.

### <a name="using-the-debugger"></a>Korzystanie z debugera

1. W górnej części strony *default. cshtml* po wierszu rozpoczynającym się od `Page.Title`Dodaj następujący wiersz kodu:

    [!code-csharp[Main](program-asp-net-web-pages-in-visual-studio/samples/sample2.cs)]
2. Na szarym marginesie edytora z lewej strony kodu kliknij przycisk Dalej w tym nowym wierszu, aby dodać *punkt przerwania*. Punkt przerwania to znacznik informujący debugera o zatrzymaniu działania programu w tym momencie, aby zobaczyć, co się dzieje.

    ![Ustaw punkt przerwania](program-asp-net-web-pages-in-visual-studio/_static/image6.png)
3. Usuń wywołanie metody `ServerInfo.GetHtml` i Dodaj wywołanie do zmiennej `@myTime` w jej miejscu. To wywołanie wyświetla bieżącą wartość czasu zwracaną przez nowy wiersz kodu.
4. Naciśnij klawisz F5, aby uruchomić stronę w debugerze. Strona zostanie zatrzymana w ustawionym punkcie przerwania. Na poniższej ilustracji pokazano, jak wygląda strona w edytorze z punktem przerwania (żółtym).

    ![Debuguj punkt przerwania](program-asp-net-web-pages-in-visual-studio/_static/image7.png)
5. Na pasku narzędzi debugowania kliknij przycisk **Wkrocz** (lub naciśnij klawisz F11), aby uruchomić następny wiersz kodu. Za każdym razem, gdy klikniesz ten przycisk, przechodzenie do następnego wiersza kodu.

    ![Przycisk Wkrocz do](program-asp-net-web-pages-in-visual-studio/_static/image8.png)
6. Sprawdź wartość zmiennej `myTime`, trzymając wskaźnik myszy nad nią lub sprawdzając wartości wyświetlane w oknach **zmiennych lokalnych** i **stosu wywołań** . Program Visual Studio Wyświetla wartość zmiennej.

    ![Pokaż wartość czasu](program-asp-net-web-pages-in-visual-studio/_static/image9.png)
7. Gdy skończysz badanie zmiennej i przechodzenie przez kod, naciśnij klawisz F5, aby kontynuować uruchamianie strony bez zatrzymywania w każdym wierszu. Gdy skończysz przechodzenie przez cały kod, przeglądarka wyświetli stronę.

Aby dowiedzieć się więcej o debugerze i sposobach debugowania kodu w programie Visual Studio, zobacz [Przewodnik: debugowanie stron sieci Web w programie Visual Web Developer](https://msdn.microsoft.com/library/z9e7w6cs.aspx).

## <a name="using-razor-in-aspnet-mvc-projects-with-visual-studio"></a>Korzystanie z Razor w projektach ASP.NET MVC w programie Visual Studio

Składnia Razor jest również szeroko używany w projektach ASP.NET MVC. MVC to zaawansowany, oparty na wzorcach sposób tworzenia dynamicznych witryn sieci Web. Jeśli witryna ASP.NET Web Pages będzie trudna do utrzymania, warto rozważyć jej konwersję do aplikacji ASP.NET MVC. Przykład tworzenia aplikacji MVC można znaleźć w [wprowadzenie z ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

<a id="vs2010support"></a>
## <a name="installing-support-for-aspnet-web-pages-in-visual-studio-2010"></a>Instalowanie obsługi stron sieci Web ASP.NET w programie Visual Studio 2010

W tej sekcji przedstawiono sposób instalowania programu Visual Web Developer Express 2010 i narzędzi ASP.NET Web Pages for Visual Studio.

1. Jeśli nie masz jeszcze Instalatora platformy sieci Web, Pobierz go z następującego adresu URL:

    [https://www.microsoft.com/web/downloads/platform.aspx](https://www.microsoft.com/web/downloads/platform.aspx)
2. Uruchom Instalatora platformy sieci Web.
3. Kliknij kartę **produkty** .

    ![Karta produkty Instalatora WebPI](program-asp-net-web-pages-in-visual-studio/_static/image10.png)
4. Wyszukaj **ASP.NET MVC 4** (dla ASP.NET Web Pages 2), a następnie kliknij przycisk **Dodaj**. Te produkty zawierają narzędzia Visual Studio Tools do tworzenia witryn sieci Web ASP.NET Razor.

    ![Opcje instalacji Instalatora WebPI](program-asp-net-web-pages-in-visual-studio/_static/image11.png)
5. Kliknij przycisk **Instaluj** , aby zakończyć instalację.

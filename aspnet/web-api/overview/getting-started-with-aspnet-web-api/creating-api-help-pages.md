---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Tworzenie stron pomocy dla ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Ten samouczek z kodem pokazuje, jak utworzyć strony pomocy dla interfejsu API sieci Web ASP.NET w ASP.NET 4. x.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8308dab8bd66aa8f5a3c5fb4133fc7a3df78f671
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556876"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a>Tworzenie stron pomocy dla interfejsu API sieci Web ASP.NET

według [Jan Wasson](https://github.com/MikeWasson)

Ten samouczek z kodem pokazuje, jak utworzyć strony pomocy dla interfejsu API sieci Web ASP.NET w ASP.NET 4. x.

Podczas tworzenia interfejsu API sieci Web często warto utworzyć stronę pomocy, aby inni deweloperzy wiedzieli, jak wywołać interfejs API. Wszystkie te dokumenty można utworzyć ręcznie, ale lepiej jest generować automatyczne generowanie tak dużo jak to możliwe. Aby to zadanie było prostsze, interfejs API sieci Web ASP.NET udostępnia bibliotekę do autogeneracji stron pomocy w czasie wykonywania.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Tworzenie stron pomocy interfejsu API

Zainstaluj [aktualizację ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650). Ta aktualizacja integruje strony pomocy z szablonem projektu interfejsu API sieci Web.

Następnie utwórz nowy projekt ASP.NET MVC 4 i wybierz szablon projektu interfejsu API sieci Web. Szablon projektu tworzy przykładowy kontroler interfejsu API o nazwie `ValuesController`. Szablon tworzy również strony pomocy interfejsu API. Wszystkie pliki kodu dla strony pomocy są umieszczane w folderze obszarów projektu.

![](creating-api-help-pages/_static/image2.png)

Po uruchomieniu aplikacji Strona główna zawiera link do strony pomocy interfejsu API. Ze strony głównej ścieżką względną jest/help.

![](creating-api-help-pages/_static/image3.png)

Ten link umożliwia przełączenie do strony podsumowania interfejsu API.

![](creating-api-help-pages/_static/image4.png)

Widok MVC dla tej strony jest zdefiniowany w obszarze Areas/HelpPage/Viewss/help/index. cshtml. Możesz edytować Tę stronę, aby modyfikować układ, wprowadzenie, tytuł, style i tak dalej.

Główna część strony jest tabelą interfejsów API pogrupowanych według kontrolera. Wpisy tabeli są generowane dynamicznie przy użyciu interfejsu **IApiExplorer** . (Dowiesz się więcej o tym interfejsie później). W przypadku dodania nowego kontrolera interfejsu API tabela zostanie automatycznie zaktualizowana w czasie wykonywania.

Kolumna "API" zawiera listę metod HTTP i względnego identyfikatora URI. Kolumna "Description" zawiera dokumentację dla każdego interfejsu API. Początkowo dokumentacja jest tylko tekstem zastępczym. W następnej sekcji pokażemy, jak dodać dokumentację z komentarzy do kodu XML.

Każdy interfejs API ma link do strony z bardziej szczegółowymi informacjami, w tym przykładowe treści żądania i odpowiedzi.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Dodawanie stron pomocy do istniejącego projektu

Możesz dodawać strony pomocy do istniejącego projektu interfejsu API sieci Web za pomocą Menedżera pakietów NuGet. Ta opcja jest przydatna, rozpoczynając od innego szablonu projektu niż szablon "Web API".

W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**. W oknie [konsola Menedżera pakietów](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) wpisz jedno z następujących poleceń:

Dla **C#** aplikacji: `Install-Package Microsoft.AspNet.WebApi.HelpPage`

W przypadku aplikacji **Visual Basic** : `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Istnieją dwa pakiety, jeden dla C# i jeden dla Visual Basic. Upewnij się, że używasz tego, który jest zgodny z projektem.

To polecenie instaluje niezbędne zestawy i dodaje widoki MVC dla stron pomocy (znajdujących się w folderze Areas/HelpPage). Musisz ręcznie dodać link do strony pomocy. Identyfikator URI to/help. Aby utworzyć łącze w widoku Razor, Dodaj następujące elementy:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Upewnij się również, że zarejestrowano obszary. W pliku Global. asax Dodaj następujący kod do **aplikacji\_Start** , jeśli jeszcze nie istnieje:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Dodawanie dokumentacji interfejsu API

Domyślnie strony pomocy mają ciągi zastępcze dla dokumentacji. Możesz użyć [komentarzy dokumentacji XML](https://msdn.microsoft.com/library/b2s063f7.aspx) , aby utworzyć dokumentację. Aby włączyć tę funkcję, Otwórz obszary pliku/HelpPage/App\_Start/HelpPageConfig. cs i Usuń komentarz z następującego wiersza:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Teraz Włącz dokumentację XML. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Właściwości**. Wybierz stronę **kompilacja** .

![](creating-api-help-pages/_static/image6.png)

W obszarze **dane wyjściowe**Sprawdź **plik dokumentacji XML**. W polu edycji wpisz "App\_Data/XmlDocument. xml".

![](creating-api-help-pages/_static/image7.png)

Następnie otwórz kod dla `ValuesController` kontrolera interfejsu API, który jest zdefiniowany w/Controllers/ValuesController.cs. Dodaj komentarze dokumentacji do metod kontrolera. Na przykład:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Porada: Jeśli umieścisz karetkę nad linią powyżej metody i wpiszesz trzy ukośniki, program Visual Studio automatycznie wstawi elementy XML. Następnie można wypełnić puste.

Teraz Skompiluj i ponownie uruchom aplikację, a następnie przejdź do stron pomocy. Ciągi dokumentacji powinny znajdować się w tabeli interfejsów API.

![](creating-api-help-pages/_static/image8.png)

Strona pomoc odczytuje ciągi z pliku XML w czasie wykonywania. (Podczas wdrażania aplikacji upewnij się, że plik XML został wdrożony).

## <a name="under-the-hood"></a>Pod okapem

Strony pomocy są zbudowane na podstawie klasy **ApiExplorer** , która jest częścią struktury internetowego interfejsu API. Klasa **ApiExplorer** dostarcza surowy materiał do tworzenia strony pomocy. Dla każdego interfejsu API **ApiExplorer** zawiera **ApiDescription** , który opisuje interfejs API. W tym celu interfejs "API" jest definiowany jako kombinacja metody HTTP i względnego identyfikatora URI. Na przykład Oto kilka różnych interfejsów API:

- Pobierz/api/Products
- Pobierz/api/Products/{id}
- Opublikuj/api/Products

Jeśli akcja kontrolera obsługuje wiele metod HTTP, **ApiExplorer** traktuje każdą metodę jako odrębny interfejs API.

Aby ukryć interfejs API z **ApiExplorer**, Dodaj atrybut **ApiExplorerSettings** do akcji i ustaw *IgnoreApi* na true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Możesz również dodać ten atrybut do kontrolera, aby wykluczyć cały kontroler.

Klasa ApiExplorer pobiera ciągi dokumentacji z interfejsu **IDocumentationProvider** . Jak pokazano wcześniej, biblioteka stron pomocy zawiera **IDocumentationProvider** , który pobiera dokumentację z CIĄGÓW dokumentacji XML. Kod znajduje się w/Areas/HelpPage/XmlDocumentationProvider.cs. Możesz uzyskać dokumentację z innego źródła, pisząc własne **IDocumentationProvider**. Aby je połączyć, wywołaj metodę rozszerzenia **SetDocumentationProvider** , która została zdefiniowana w **HelpPageConfigurationExtensions**

**ApiExplorer** automatycznie wywołuje interfejs **IDocumentationProvider** w celu pobrania ciągów dokumentacji dla każdego interfejsu API. Przechowuje je we właściwości **Dokumentacja** obiektów **ApiDescription** i **ApiParameterDescription** .

## <a name="next-steps"></a>Następne kroki

Nie masz ograniczeń do stron pomocy widocznych tutaj. W rzeczywistości **ApiExplorer** nie jest ograniczony do tworzenia stron pomocy. Yao Huang, który zapisał kilka doskonałych wpisów w blogu, aby dowiedzieć się, jak to zrobić:

- [Dodawanie prostego klienta testowego do strony pomocy interfejsu API sieci Web ASP.NET](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Tworzenie strony pomocy interfejsu API sieci Web ASP.NET w usługach samoobsługowych](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Generowanie w czasie projektowania strony pomocy (lub klienta) dla interfejsu API sieci Web ASP.NET](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Zaawansowane dostosowania stron pomocy](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)

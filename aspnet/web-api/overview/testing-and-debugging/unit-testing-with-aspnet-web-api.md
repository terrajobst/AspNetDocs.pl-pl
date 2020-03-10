---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Testowanie jednostkowe ASP.NET Web API 2 | Microsoft Docs
author: Rick-Anderson
description: Te wskazówki i aplikacje pokazują, jak utworzyć proste testy jednostkowe dla aplikacji Web API 2. W tym samouczku przedstawiono sposób dołączania projektu testów jednostkowych...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f2d60b977475e048a3a74aabff4adc768ee22baf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554972"
---
# <a name="unit-testing-aspnet-web-api-2"></a>Testowanie jednostkowe ASP.NET Web API 2

Autor [FitzMacken](https://github.com/tfitzmac)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Te wskazówki i aplikacje pokazują, jak utworzyć proste testy jednostkowe dla aplikacji Web API 2. W tym samouczku pokazano, jak uwzględnić projekt testu jednostkowego w rozwiązaniu i pisać metody testowe, które sprawdzają zwrócone wartości z metody kontrolera.
>
> W tym samouczku założono, że znasz podstawowe pojęcia związane z interfejsem API sieci Web ASP.NET. Aby zapoznać się z samouczkiem wprowadzającym, zobacz [wprowadzenie with Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
>
> Testy jednostkowe w tym temacie są celowo ograniczone do prostych scenariuszy danych. Aby przetestować jednostkowe bardziej zaawansowane scenariusze danych, zobacz [imitacja Entity Framework podczas testowania jednostkowego ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Internetowy interfejs API 2

## <a name="in-this-topic"></a>W tym temacie

Ten temat zawiera następujące sekcje:

- [Wymagania wstępne](#prereqs)
- [Pobierz kod](#download)
- [Tworzenie aplikacji przy użyciu projektu testów jednostkowych](#appwithunittest)
    - [Dodaj projekt testu jednostkowego podczas tworzenia aplikacji](#whencreate)
    - [Dodaj projekt testu jednostkowego do istniejącej aplikacji](#addtoexisting)
- [Konfigurowanie aplikacji Web API 2](#setupproject)
- [Zainstaluj pakiety NuGet w projekcie testowym](#testpackages)
- [Utwórz testy](#tests)
- [Uruchom testy](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Wymagania wstępne

[Program Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional lub Enterprise Edition

<a id="download"></a>
## <a name="download-code"></a>Pobieranie kodu

Pobierz [ukończony projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Projekt do pobrania zawiera kod testu jednostkowego dla tego tematu oraz dla [Entity Framework imitacji podczas testowania jednostek w sieci Web API ASP.NET](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) .

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Tworzenie aplikacji przy użyciu projektu testów jednostkowych

Możesz utworzyć projekt testu jednostkowego podczas tworzenia aplikacji lub dodać projekt testu jednostkowego do istniejącej aplikacji. W tym samouczku przedstawiono obie metody tworzenia projektu testów jednostkowych. Aby wykonać czynności opisane w tym samouczku, można użyć dowolnego z tych rozwiązań.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Dodaj projekt testu jednostkowego podczas tworzenia aplikacji

Utwórz nową aplikację sieci Web ASP.NET o nazwie **StoreApp**.

![Utwórz projekt](unit-testing-with-aspnet-web-api/_static/image1.png)

W oknie Nowy projekt ASP.NET okna, wybierz **pusty** szablon i Dodaj foldery i podstawowe odwołania dla internetowego interfejsu API. Wybierz opcję **Dodaj testy jednostkowe** . Projekt testu jednostkowego jest automatycznie nazwany **StoreApp. Tests**. Możesz zachować tę nazwę.

![Utwórz projekt testu jednostkowego](unit-testing-with-aspnet-web-api/_static/image2.png)

Po utworzeniu aplikacji zobaczysz, że zawiera ona dwa projekty.

![dwa projekty](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Dodaj projekt testu jednostkowego do istniejącej aplikacji

Jeśli projekt testu jednostkowego nie został utworzony podczas tworzenia aplikacji, możesz dodać go w dowolnym momencie. Załóżmy na przykład, że masz już aplikację o nazwie StoreApp i chcesz dodać testy jednostkowe. Aby dodać projekt testu jednostkowego, kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz pozycję **Dodaj** i **Nowy projekt**.

![Dodaj nowy projekt do rozwiązania](unit-testing-with-aspnet-web-api/_static/image4.png)

W lewym okienku wybierz pozycję **test** , a następnie wybierz pozycję **projekt testu jednostkowego** dla typu projektu. Nazwij projekt **StoreApp. Tests**.

![Dodaj projekt testu jednostkowego](unit-testing-with-aspnet-web-api/_static/image5.png)

W rozwiązaniu zobaczysz projekt testu jednostkowego.

W projekcie testów jednostkowych Dodaj odwołanie do projektu do oryginalnego projektu.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Konfigurowanie aplikacji Web API 2

W projekcie StoreApp Dodaj plik klasy do folderu **models** o nazwie **Product.cs**. Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Skompiluj rozwiązanie.

Kliknij prawym przyciskiem myszy folder controllers, a następnie wybierz polecenie **Dodaj** i **nowy element szkieletowy**. Wybierz **kontroler Web API 2 — pusty**.

![Dodaj nowy kontroler](unit-testing-with-aspnet-web-api/_static/image6.png)

Ustaw nazwę kontrolera na **SimpleProductController**, a następnie kliknij przycisk **Dodaj**.

![Określ kontroler](unit-testing-with-aspnet-web-api/_static/image7.png)

Zastąp istniejący kod następującym kodem. Aby uprościć ten przykład, dane są przechowywane na liście, a nie w bazie danych. Lista zdefiniowana w tej klasie reprezentuje dane produkcyjne. Należy zauważyć, że kontroler zawiera konstruktora, który przyjmuje jako parametr listę obiektów produktów. Ten konstruktor umożliwia przekazywanie danych testowych podczas testowania jednostkowego. Kontroler zawiera również dwie metody **asynchroniczne** do zilustrowania metod asynchronicznych testów jednostkowych. Te metody asynchroniczne zostały zaimplementowane przez wywołanie **Task. FromResult** , aby zminimalizować nadmiarowy kod, ale zazwyczaj metody obejmują operacje intensywnie korzystające z zasobów.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

Metoda getproduct zwraca wystąpienie interfejsu **IHttpActionResult** . IHttpActionResult to jedna z nowych funkcji w interfejsie Web API 2, która upraszcza proces tworzenia testów jednostkowych. Klasy implementujące interfejs IHttpActionResult są dostępne w przestrzeni nazw [System. Web. http. results](https://msdn.microsoft.com/library/system.web.http.results.aspx) . Te klasy reprezentują możliwe odpowiedzi z żądania akcji i odpowiadają kodów stanu HTTP.

Skompiluj rozwiązanie.

Teraz można przystąpić do konfigurowania projektu testowego.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Zainstaluj pakiety NuGet w projekcie testowym

W przypadku tworzenia aplikacji za pomocą pustego szablonu projekt testu jednostkowego (StoreApp. Tests) nie zawiera żadnych zainstalowanych pakietów NuGet. Inne szablony, takie jak szablon internetowego interfejsu API, zawierają niektóre pakiety NuGet w projekcie testów jednostkowych. W tym samouczku należy uwzględnić pakiet Microsoft ASP.NET Web API 2 Core w projekcie testowym.

Kliknij prawym przyciskiem myszy projekt StoreApp. Tests i wybierz pozycję **Zarządzaj pakietami NuGet**. Musisz wybrać projekt StoreApp. Tests, aby dodać pakiety do tego projektu.

![Zarządzanie pakietami](unit-testing-with-aspnet-web-api/_static/image8.png)

Znajdź i zainstaluj pakiet Microsoft ASP.NET Web API 2 Core.

![Zainstaluj pakiet Core interfejsu API sieci Web](unit-testing-with-aspnet-web-api/_static/image9.png)

Zamknij okno Zarządzaj pakietami NuGet.

<a id="tests"></a>
## <a name="create-tests"></a>Utwórz testy

Domyślnie projekt testowy zawiera pusty plik testowy o nazwie UnitTest1.cs. Ten plik zawiera atrybuty używane do tworzenia metod testowych. W przypadku testów jednostkowych możesz użyć tego pliku lub utworzyć własny plik.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

Na potrzeby tego samouczka utworzysz własną klasę testową. Można usunąć plik UnitTest1.cs. Dodaj klasę o nazwie **TestSimpleProductController.cs**i Zastąp kod następującym kodem.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Uruchom testy

Teraz można przystąpić do uruchamiania testów. Zostanie przetestowana cała metoda oznaczona atrybutem **TestMethod** . Z elementu menu **testowego** Uruchom testy.

![uruchom testy](unit-testing-with-aspnet-web-api/_static/image11.png)

Otwórz okno **Eksplorator testów** i zwróć uwagę na wyniki testów.

![wyniki testu](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Podsumowanie

Ten samouczek został ukończony. Dane w tym samouczku zostały celowo uproszczone, aby skoncentrować się na warunkach testowania jednostkowego. Aby przetestować jednostkowe bardziej zaawansowane scenariusze danych, zobacz [imitacja Entity Framework podczas testowania jednostkowego ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).

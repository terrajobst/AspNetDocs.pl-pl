---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Imitacja Entity Framework, gdy testy jednostkowe ASP.NET Web API 2 | Microsoft Docs
author: Rick-Anderson
description: Te wskazówki i aplikacje pokazują, jak utworzyć testy jednostkowe dla aplikacji Web API 2, która używa Entity Framework. Pokazuje, jak zmodyfikować...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 258450107ee7443c4efd43a3b8e4851249745227
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555091"
---
# <a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Imitacja Entity Framework, gdy testy jednostkowe ASP.NET Web API 2

Autor [FitzMacken](https://github.com/tfitzmac)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> Te wskazówki i aplikacje pokazują, jak utworzyć testy jednostkowe dla aplikacji Web API 2, która używa Entity Framework. Przedstawiono w nim sposób modyfikacji kontrolera szkieletowego w celu umożliwienia przekazywania obiektu kontekstu do testowania oraz tworzenia obiektów testowych, które współpracują z Entity Framework.
>
> Aby zapoznać się z wprowadzeniem do testów jednostkowych za pomocą interfejsu API sieci Web ASP.NET, zobacz [testowanie jednostkowe za pomocą ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).
>
> W tym samouczku założono, że znasz podstawowe pojęcia związane z interfejsem API sieci Web ASP.NET. Aby zapoznać się z samouczkiem wprowadzającym, zobacz [wprowadzenie with Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
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
- [Tworzenie klasy modelu](#modelclass)
- [Dodawanie kontrolera](#controller)
- [Dodaj iniekcję zależności](#dependency)
- [Zainstaluj pakiety NuGet w projekcie testowym](#testpackages)
- [Utwórz kontekst testu](#testcontext)
- [Utwórz testy](#tests)
- [Uruchom testy](#runtests)

Jeśli wykonano już kroki [testowania jednostkowego w programie ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), możesz przejść do sekcji [Dodaj kontroler](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Wymagania wstępne

Visual Studio 2017 Community, Professional lub Enterprise Edition

<a id="download"></a>
## <a name="download-code"></a>Pobieranie kodu

Pobierz [ukończony projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Projekt do pobrania zawiera kod testu jednostkowego dla tego tematu oraz dla [testów jednostkowych ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) .

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Tworzenie aplikacji przy użyciu projektu testów jednostkowych

Możesz utworzyć projekt testu jednostkowego podczas tworzenia aplikacji lub dodać projekt testu jednostkowego do istniejącej aplikacji. W tym samouczku przedstawiono tworzenie projektu testu jednostkowego podczas tworzenia aplikacji.

Utwórz nową aplikację sieci Web ASP.NET o nazwie **StoreApp**.

W oknie Nowy projekt ASP.NET okna, wybierz **pusty** szablon i Dodaj foldery i podstawowe odwołania dla internetowego interfejsu API. Wybierz opcję **Dodaj testy jednostkowe** . Projekt testu jednostkowego jest automatycznie nazwany **StoreApp. Tests**. Możesz zachować tę nazwę.

![Utwórz projekt testu jednostkowego](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Po utworzeniu aplikacji zobaczysz, że będzie ona zawierać dwa projekty — **StoreApp** i **StoreApp. Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Tworzenie klasy modelu

W projekcie StoreApp Dodaj plik klasy do folderu **models** o nazwie **Product.cs**. Zastąp zawartość pliku następującym kodem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Skompiluj rozwiązanie.

<a id="controller"></a>
## <a name="add-the-controller"></a>Dodawanie kontrolera

Kliknij prawym przyciskiem myszy folder controllers, a następnie wybierz polecenie **Dodaj** i **nowy element szkieletowy**. Wybierz kontroler Web API 2 z akcjami przy użyciu Entity Framework.

![Dodaj nowy kontroler](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Ustaw następujące wartości:

- Nazwa kontrolera: **ProductController**
- Klasa modelu: **produkt**
- Klasa kontekstu danych: [Wybierz **Nowy przycisk kontekstu danych** , który wypełnia wartości widoczne poniżej]

![Określ kontroler](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Kliknij przycisk **Dodaj** , aby utworzyć kontroler z automatycznie generowanym kodem. Kod zawiera metody tworzenia, pobierania, aktualizowania i usuwania wystąpień klasy produktu. Poniższy kod przedstawia metodę dodawania produktu. Zwróć uwagę, że metoda zwraca wystąpienie elementu **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult to jedna z nowych funkcji w interfejsie Web API 2, która upraszcza proces tworzenia testów jednostkowych.

W następnej sekcji zostanie dostosowany wygenerowany kod, aby ułatwić przekazywanie obiektów testowych do kontrolera.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Dodaj iniekcję zależności

Obecnie Klasa ProductController jest zakodowana w taki sposób, aby korzystała z wystąpienia klasy StoreAppContext. Użyjesz wzorca zwanego iniekcją zależności, aby zmodyfikować aplikację i usunąć tę sztywną zależność. Przerywając Tę zależność, można przekazać obiekt makiety podczas testowania.

Kliknij prawym przyciskiem myszy folder **modele** i Dodaj nowy interfejs o nazwie **IStoreAppContext**.

Zastąp kod następującym kodem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Otwórz plik StoreAppContext.cs i wprowadź następujące wyróżnione zmiany. Ważne zmiany należy zwrócić na następujące:

- Klasa StoreAppContext implementuje interfejs IStoreAppContext
- Zaimplementowano metodę MarkAsModified

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Otwórz plik ProductController.cs. Zmień istniejący kod w taki sposób, aby był zgodny z wyróżnionym kodem. Te zmiany przerywają zależność od StoreAppContext i umożliwiają innym klasom przekazywanie w innym obiekcie dla klasy kontekstu. Ta zmiana umożliwi przekazanie kontekstu testu podczas testów jednostkowych.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Istnieje jeszcze jedna zmiana, którą trzeba wykonać w ProductController. W metodzie **PutProduct** Zastąp wiersz, który ustawia stan jednostki na zmodyfikowane przy użyciu wywołania metody MarkAsModified.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Skompiluj rozwiązanie.

Teraz można przystąpić do konfigurowania projektu testowego.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Zainstaluj pakiety NuGet w projekcie testowym

W przypadku tworzenia aplikacji za pomocą pustego szablonu projekt testu jednostkowego (StoreApp. Tests) nie zawiera żadnych zainstalowanych pakietów NuGet. Inne szablony, takie jak szablon internetowego interfejsu API, zawierają niektóre pakiety NuGet w projekcie testów jednostkowych. W tym samouczku należy uwzględnić pakiet Entity Framework i pakiet Microsoft ASP.NET Web API 2 Core w projekcie testowym.

Kliknij prawym przyciskiem myszy projekt StoreApp. Tests i wybierz pozycję **Zarządzaj pakietami NuGet**. Musisz wybrać projekt StoreApp. Tests, aby dodać pakiety do tego projektu.

![Zarządzanie pakietami](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Z pakietów online Znajdź i zainstaluj pakiet EntityFramework (wersja 6,0 lub nowsza). Jeśli zostanie wyświetlona informacja, że pakiet EntityFramework jest już zainstalowany, prawdopodobnie zamiast projektu StoreApp. Tests został wybrany projekt StoreApp.

![Dodaj Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Znajdź i zainstaluj pakiet Microsoft ASP.NET Web API 2 Core.

![Zainstaluj pakiet Core interfejsu API sieci Web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Zamknij okno Zarządzaj pakietami NuGet.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Utwórz kontekst testu

Dodaj klasę o nazwie **TestDbSet** do projektu testowego. Ta klasa służy jako klasa bazowa dla zestawu danych testowych. Zastąp kod następującym kodem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Dodaj klasę o nazwie **TestProductDbSet** do projektu testowego, który zawiera poniższy kod.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Dodaj klasę o nazwie **TestStoreAppContext** i Zastąp istniejący kod następującym kodem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Utwórz testy

Domyślnie projekt testowy zawiera pusty plik testowy o nazwie **UnitTest1.cs**. Ten plik zawiera atrybuty używane do tworzenia metod testowych. Na potrzeby tego samouczka możesz usunąć ten plik, ponieważ dodasz nową klasę testową.

Dodaj klasę o nazwie **TestProductController** do projektu testowego. Zastąp kod następującym kodem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Uruchom testy

Teraz można przystąpić do uruchamiania testów. Zostanie przetestowana cała metoda oznaczona atrybutem **TestMethod** . Z elementu menu **testowego** Uruchom testy.

![uruchom testy](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Otwórz okno **Eksplorator testów** i zwróć uwagę na wyniki testów.

![wyniki testu](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)

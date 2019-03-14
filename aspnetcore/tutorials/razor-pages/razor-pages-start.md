---
title: 'Samouczek: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core'
author: rick-anderson
description: W tej serii samouczków pokazano, jak używać stron Razor w programie ASP.NET Core. Dowiedz się, jak utworzyć model, generowanie kodu dla stron Razor, platformy Entity Framework Core i SQL Server na użytek dostęp do danych, dodać funkcje wyszukiwania, dodać sprawdzanie poprawności danych wejściowych i użyć migracje do aktualizacji modelu.
ms.author: riande
ms.date: 12/5/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 81a2a76fc1cecc78b69226fe714d7c9272b04bf7
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57072455"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a>Samouczek: Rozpoczynanie pracy ze stronami Razor w programie ASP.NET Core

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

To jest pierwszy samouczek serii. [Seria](xref:tutorials/razor-pages/index) uczy podstaw tworzenia aplikacji platformy ASP.NET Core Razor strony sieci web.

[!INCLUDE[](~/includes/advancedRP.md)]

Na końcu serii będziesz mieć aplikację, która zarządza bazy danych filmów.  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Tworzenie aplikacji internetowej stron Razor.
> * Uruchom aplikację.
> * Sprawdź pliki projektu.

Na końcu tego samouczka będziesz mieć działającą aplikację sieci web stron Razor, które utworzysz w kolejnych samouczkach.

![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-razor-pages-web-app"></a>Tworzenie aplikacji sieci web stron Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* W programie Visual Studio **pliku** menu, wybierz opcję **New** > **projektu**.

* Tworzenie nowej aplikacji sieci Web platformy ASP.NET Core. Nadaj projektowi nazwę **RazorPagesMovie**. Ważne jest, aby nadaj projektowi nazwę *RazorPagesMovie* , przestrzenie nazw będą zgodne po skopiuj i Wklej kod.

  ![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* Wybierz **platformy ASP.NET Core 2.2** w listy rozwijanej, a następnie wybierz pozycję **aplikacji sieci Web**.

  ![Nowa aplikacja internetowa ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  Utworzono następujący projekt startowy:

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).

* Zmień katalog (`cd`) do folderu, który będzie zawierać projekt.

* Uruchom następujące polecenia:

  ```console
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * `dotnet new` Polecenie tworzy nowy projekt strony Razor w *RazorPagesMovie* folderu.
  * `code` Polecenia otwiera *RazorPagesMovie* folderu w nowym wystąpieniu programu Visual Studio Code.

  Zostanie wyświetlone okno dialogowe z **"RazorPagesMovie" brakuje wymagane zasoby do tworzenia i debugowania. Dodaj je?**

* Wybierz **tak**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

W terminalu uruchom następujące polecenia:

<!-- TODO: update these instruction once mac support 2.2 projects -->

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
```

Poprzednie polecenia użyj [interfejsu wiersza polecenia platformy .NET Core](/dotnet/core/tools/dotnet) do utworzenia projektu stron Razor.

## <a name="open-the-project"></a>Otwórz projekt

Z programu Visual Studio, wybierz **Plik > Otwórz**, a następnie wybierz pozycję *RazorPagesMovie.csproj* pliku.

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a>Uruchamianie aplikacji

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Naciśnij klawisze Ctrl + F5, aby uruchomić bez debugowania.

  [!INCLUDE[](~/includes/trustCertVS.md)]

  Program Visual Studio uruchamia [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomienie aplikacji. Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta komputera lokalnego. Localhost obsługują tylko żądania sieci web z komputera lokalnego. Gdy program Visual Studio tworzy projekt sieci web, losowy port jest używany dla serwera sieci web.
  
# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Naciśnij klawisz **Ctrl-F5** do uruchomienia bez debugera.

  [!INCLUDE[](~/includes/trustCertVSC.md)]

  Uruchamia programu Visual Studio Code [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `http://localhost:5001`. Przedstawia pasek adresu `localhost:port#` i nie mielibyśmy mieć czegoś podobnego `example.com`. To dlatego, że `localhost` jest standardowa nazwa hosta na komputerze lokalnym. Localhost obsługują tylko żądania sieci web z komputera lokalnego.
  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

Wybierz **Uruchom > Uruchom bez debugowania** do uruchomienia aplikacji. Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel)otworzy w przeglądarce i przechodzi do `http://localhost:5001`.

[!INCLUDE[](~/includes/trustCertMac.md)]

<!-- End of VS tabs -->

---

* Na stronie głównej aplikacji, wybierz **Akceptuj** do wyrażenia zgody na śledzenie.

  Ta aplikacja nie może śledzić informacje osobiste, ale szablonu projektu obejmuje funkcję zgody, w przypadku, gdy będą potrzebne do wykonania w Unii Europejskiej [ogólne rozporządzenie o ochronie danych (RODO)](xref:security/gdpr).

  ![Strona główna lub indeks](razor-pages-start/_static/homeGDPR2.2.png)

  Na poniższej ilustracji przedstawiono aplikację po zgody śledzenia:

  ![Strona główna lub indeks](razor-pages-start/_static/home2.2.png)

## <a name="examine-the-project-files"></a>Przejrzyj pliki projektu

Poniżej przedstawiono omówienie folderów głównego projektu i plików, które będziesz pracować w kolejnych samouczkach.

### <a name="pages-folder"></a>Folder stron

Zawiera stronami Razor i pliki pomocnicze. Każda strona Razor jest parę plików:

* A *.cshtml* pliku, który zawiera kod znaczników HTML za pomocą C# kodu przy użyciu składni Razor.
* A *. cshtml.cs* pliku, który zawiera C# kod, który obsługuje zdarzenia strony.

Pliki obsługi mają nazwy rozpoczynające się od znaku podkreślenia. Na przykład *_Layout.cshtml* plik konfiguruje elementy interfejsu użytkownika dla wszystkich stron. Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony. Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.


### <a name="wwwroot-folder"></a>Wwwroot folder

Zawiera pliki statyczne, takie jak pliki HTML, plików JavaScript i plików CSS. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.

### <a name="appsettingsjson"></a>appSettings.json

Zawiera dane konfiguracyjne, takie jak parametry połączenia. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.

### <a name="programcs"></a>Program.cs

Zawiera punkt wejścia programu. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/web-host>.

### <a name="startupcs"></a>Startup.cs

Zawiera kod, który konfiguruje zachowania aplikacji, na przykład tego, czy wymaga zgody na pliki cookie. Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.

## <a name="next-steps"></a>Następne kroki

W ramach tego samouczka możesz:

> [!div class="checklist"]
> * Utworzona aplikacja internetowa ze stronami Razor.
> * Uruchomienia aplikacji.
> * Zbadane plików projektu.

Przejdź do następnego samouczka w serii:

> [!div class="step-by-step"]
> [Dodawanie modelu](xref:tutorials/razor-pages/model)

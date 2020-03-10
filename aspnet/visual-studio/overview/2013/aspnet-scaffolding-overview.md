---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Tworzenie szkieletu ASP.NET w Visual Studio 2013 | Microsoft Docs
author: Rick-Anderson
description: Tworzenie szkieletu ASP.NET to nowa funkcja, która jest zawarta w Visual Studio 2013.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: cf4669b769cee28475e2dd6a6ddf07ea1434d04d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557982"
---
# <a name="aspnet-scaffolding-in-visual-studio-2013"></a>Funkcja tworzenia szkieletu ASP.NET w programie Visual Studio 2013

Autor [FitzMacken](https://github.com/tfitzmac)

> Tworzenie szkieletu ASP.NET to nowa funkcja, która jest zawarta w Visual Studio 2013.

## <a name="overview"></a>Omówienie

Tworzenie szkieletów ASP.NET jest strukturą generowania kodu dla aplikacji sieci Web ASP.NET. Visual Studio 2013 obejmuje wstępnie zainstalowane generatory kodu dla projektów MVC i Web API. Dodawanie szkieletu do projektu, gdy chcesz szybko dodać kod, który współdziała z modelami danych. Użycie szkieletu może skrócić czas projektowania standardowych operacji na danych w projekcie.

Domyślnie Visual Studio 2013 nie obsługuje generowania kodu dla projektu formularzy sieci Web, ale można użyć szkieletów z formularzami sieci Web przez dodanie zależności MVC do projektu lub zainstalowanie rozszerzenia. Poniżej przedstawiono oba podejścia.

Visual Studio 2013 Update 2 (obecnie RC) zapewnia możliwość rozbudowania szkieletu ASP.NET w celu spełnienia wymagań danego scenariusza. Za pomocą tej funkcji można utworzyć dostosowany szablon tworzenia szkieletu i dodać go do okna dialogowego Dodawanie nowej szkieletu. W ramach niestandardowego szablonu należy określić kod, który jest generowany podczas dodawania szkieletu elementu. Aby uzyskać więcej informacji, zobacz [Tworzenie niestandardowego szkieletu dla programu Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Wymagania wstępne

Aby korzystać z szkieletu ASP.NET, musisz mieć:

- Microsoft Visual Studio 2013
- Narzędzia deweloperskie sieci Web (część domyślnej instalacji Visual Studio 2013)
- ASP.NET Web Frameworks and Tools 2013 (część domyślnej instalacji Visual Studio 2013)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Dodaj element szkieletowy do MVC lub interfejsu API sieci Web

Aby dodać szkielet, kliknij prawym przyciskiem myszy projekt lub folder w projekcie, a następnie wybierz polecenie **Dodaj** — **nowy element szkieletowy**, jak pokazano na poniższej ilustracji.

![Dodaj element szkieletu](aspnet-scaffolding-overview/_static/image1.png)

W oknie **Dodawanie szkieletu** wybierz typ szkieletu do dodania.

![Wybierz typ szkieletu](aspnet-scaffolding-overview/_static/image2.png)

Okno **Dodawanie kontrolera** umożliwia wybranie opcji związanych z generowaniem kontrolera, w tym o tym, czy mają być używane nowe funkcje asynchroniczne z Entity Framework 6.

![Dodaj kontroler](aspnet-scaffolding-overview/_static/image3.png)

Dla danego scenariusza tworzone są odpowiednie klasy i strony. Na przykład na poniższej ilustracji przedstawiono kontroler MVC i widoki, które zostały utworzone za pomocą szkieletów dla klasy modelu o nazwie filmy.

![Utworzone pliki](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Dodawanie elementu szkieletowego do formularzy sieci Web

Aby dodać szkielet, który generuje kod formularzy sieci Web, należy zainstalować rozszerzenie programu Visual Studio lub dodać zależności MVC. Poniżej przedstawiono oba podejścia, ale należy wykonać tylko jedną z tych metod.

### <a name="web-forms-scaffolding-extension"></a>Rozszerzenie szkieletu formularzy sieci Web

Można zainstalować rozszerzenie programu Visual Studio, które umożliwia korzystanie z szkieletów z projektem formularzy sieci Web. W programie Visual Studio wybierz pozycje **Narzędzia** , a następnie **rozszerzenia i aktualizacje**. Z tego okna dialogowego Przeszukaj galerię programu Visual Studio dla **szkieletu formularzy sieci Web**.

![Instalowanie szkieletu formularzy sieci Web](aspnet-scaffolding-overview/_static/image5.png)

Aby uzyskać więcej informacji, zobacz Tworzenie [szkieletu formularzy sieci Web](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>Zależności MVC

Aby dodać zależności MVC, wybierz pozycję **dodaj** - **nowy element szkieletowy**. W oknie Dodawanie szkieletu wybierz pozycję **zależności MVC**, jak pokazano poniżej.

![Dodawanie zależności MVC](aspnet-scaffolding-overview/_static/image6.png)

Dostępne są dwie opcje tworzenia szkieletów MVC; Minimalny i pełny. W przypadku wybrania opcji minimalny tylko pakiety NuGet i odwołania dla ASP.NET MVC zostaną dodane do projektu. W przypadku wybrania opcji Pełna są dodawane minimalne zależności, a także wymagane pliki zawartości dla projektu MVC. Aby łatwo korzystać z szkieletów, wybierz pozycję pełne zależności.

![Wybierz pełne zależności](aspnet-scaffolding-overview/_static/image7.png)

Po dodaniu zależności zobaczysz plik **README. txt** . Uważnie postępuj zgodnie z instrukcjami w tym pliku, aby upewnić się, że projekt działa prawidłowo.

Po wykonaniu kroków opisanych w pliku Readme. txt można dodać nowy element szkieletowy, jak pokazano w poprzedniej sekcji dotyczącej składnika MVC i interfejsu Web API. Automatycznie generowane widoki i kontroler będą działać prawidłowo w ramach projektu.

## <a name="tutorials"></a>Samouczki

Aby utworzyć dostosowany szkielet, zobacz [Tworzenie niestandardowego szkieletu dla programu Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Aby dostosować wygenerowane pliki, zobacz [How to Dostosowywanie wygenerowanych plików z okna dialogowego Nowy element szkieletowy](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Aby zapoznać się z przykładem korzystania z szkieletów z **programowaniem Database First**, zobacz [EF Database First z ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Aby zapoznać się z przykładem użycia szkieletu w projekcie **MVC** , zobacz [wprowadzenie with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Aby zapoznać się z przykładem użycia szkieletu w projekcie **interfejsu API sieci Web** , zobacz [Tworzenie interfejsu API REST z routingiem atrybutów w interfejsie Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).

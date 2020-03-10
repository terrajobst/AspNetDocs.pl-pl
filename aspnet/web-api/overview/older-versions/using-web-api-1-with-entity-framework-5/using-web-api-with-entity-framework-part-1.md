---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Część 1: omówienie i tworzenie projektu | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: a76a18f2bd95969358452085ef342fdca8a386e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556078"
---
# <a name="part-1-overview-and-creating-the-project"></a>Część 1: przegląd i tworzenie projektu

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework to struktura mapowania obiektów/obiektu. Mapuje obiekty domeny w kodzie na jednostki w relacyjnej bazie danych. W największej części nie trzeba martwić się o warstwę bazy danych, ponieważ Entity Framework ją zajmie. Kod operuje na obiektach, a zmiany są utrwalane w bazie danych.

## <a name="about-the-tutorial"></a>Informacje o samouczku

W tym samouczku utworzysz prostą aplikację ze sklepu. Istnieją dwa główne elementy aplikacji. Normalni użytkownicy mogą wyświetlać produkty i tworzyć zamówienia:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Administratorzy mogą tworzyć, usuwać lub edytować produkty:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Posiadane umiejętności

Oto, co uzyskasz:

- Jak używać Entity Framework z interfejsem API sieci Web ASP.NET.
- Jak używać narzędzia separowania. js do tworzenia interfejsu użytkownika klienta dynamicznego.
- Jak uwierzytelniać użytkowników przy użyciu interfejsu API sieci Web.

Mimo że ten samouczek jest samodzielny, warto najpierw przeczytać następujące samouczki:

- [Twój pierwszy ASP.NET internetowy interfejs API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Tworzenie internetowego interfejsu API obsługującego operacje CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Znajomość [ASP.NET MVC](../../../../mvc/index.md) jest również przydatna.

## <a name="overview"></a>Omówienie

Na wysokim poziomie poniżej przedstawiono architekturę aplikacji:

- ASP.NET MVC generuje strony HTML dla klienta.
- Interfejs API sieci Web ASP.NET uwidacznia operacje CRUD na danych (produkty i zamówienia).
- Entity Framework tłumaczy C# modele używane przez internetowy interfejs API do jednostek bazy danych.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

Na poniższym diagramie przedstawiono sposób reprezentowania obiektów domeny w różnych warstwach aplikacji: warstwy bazy danych, modelu obiektów i na końcu formatu sieci, który jest używany do przesyłania danych do klienta za pośrednictwem protokołu HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio

Możesz utworzyć projekt samouczka przy użyciu programu Visual Web Developer Express lub pełnej wersji programu Visual Studio.

Na stronie **startowej** kliknij pozycję **Nowy projekt**.

W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  . W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**. Na liście szablonów projektu wybierz pozycję **aplikacja sieci Web MVC 4 ASP.NET**. Nadaj projektowi nazwę "ProductStore" i kliknij przycisk **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

W oknie dialogowym **Nowy projekt ASP.NET MVC 4** wybierz pozycję **aplikacja internetowa** , a następnie kliknij przycisk **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

Szablon "aplikacja internetowa" tworzy aplikację ASP.NET MVC, która obsługuje uwierzytelnianie formularzy. Jeśli aplikacja zostanie uruchomiona teraz, ma już pewne funkcje:

- Nowi użytkownicy mogą zarejestrować się, klikając link "Register" w prawym górnym rogu.
- Zarejestrowani użytkownicy mogą się zalogować, klikając łącze "Zaloguj się".

Informacje o członkostwie są utrwalane w bazie danych, która jest tworzona automatycznie. Aby uzyskać więcej informacji na temat uwierzytelniania formularzy w ASP.NET MVC, zobacz [Przewodnik: używanie uwierzytelniania formularzy w ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Aktualizowanie pliku CSS

Ten krok to element kosmetyczny, ale strony będą renderowane jak wcześniejsze zrzuty ekranu.

W Eksplorator rozwiązań rozwiń folder Content (zawartość) i Otwórz plik o nazwie Site. css. Dodaj następujące style CSS:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [Dalej](using-web-api-with-entity-framework-part-2.md)

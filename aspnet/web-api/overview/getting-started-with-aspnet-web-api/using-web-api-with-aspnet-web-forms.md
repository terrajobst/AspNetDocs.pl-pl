---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Za pomocą interfejsu API sieci Web przy użyciu wzorca ASP.NET Web Forms — ASP.NET 4.x
author: MikeWasson
description: Samouczek z kodu krok po kroku, aby dodać interfejs API sieci Web do aplikacji formularzy programu ASP.NET dla aplikacji ASP.NET 4.x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59422580"
---
# <a name="using-web-api-with-aspnet-web-forms"></a>Korzystanie z wzorca Web API we wzorcu ASP.NET Web Forms

przez [Mike Wasson](https://github.com/MikeWasson)

Ten samouczek przeprowadzi Cię przez kroki Aby dodać interfejs API sieci Web do tradycyjnych aplikacji formularzy sieci Web platformy ASP.NET na platformie ASP.NET 4.x. 

## <a name="overview"></a>Omówienie

Chociaż interfejs API sieci Web platformy ASP.NET jest dostarczana z platformą ASP.NET MVC, jest łatwe do dodania interfejsu API sieci Web do tradycyjnych aplikacji formularzy sieci Web ASP.NET.

Aby użyć interfejsu API sieci Web w aplikacji formularzy sieci Web, istnieją dwa podstawowe kroki:

- Dodawanie kontrolera interfejsu API sieci Web, która pochodzi od klasy **klasy ApiController** klasy.
- Dodaj tabelę tras w celu **aplikacji\_Start** metody.

## <a name="create-a-web-forms-project"></a>Utwórz projekt formularzy sieci Web

Uruchom program Visual Studio i wybierz pozycję **Nowy projekt** na stronie **Start**. Możesz również z menu **Plik** wybrać pozycję **Nowy**, a następnie **Projekt**.

W okienku **Szablony** wybierz pozycję **Zainstalowane szablony** i rozwiń węzeł **Visual C#**. W obszarze **Visual C#**, wybierz pozycję **Sieć Web**. Na liście szablonów projektu wybierz **aplikacji formularzy sieci Web ASP.NET**. Wprowadź nazwę dla projektu, a następnie kliknij przycisk **OK**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Tworzenie modelu i kontrolera

W tym samouczku korzysta z tej samej klasy modelu i kontrolera jako [wprowadzenie](tutorial-your-first-web-api.md) samouczka.

Najpierw Dodaj klasę modelu. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt i wybierz **Dodaj klasę**. Nazwa klasy produktu i dodaj następującą implementacją:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Następnie dodaj Kontroler interfejsu API sieci Web do projektu., A *kontrolera* jest obiektem, który obsługuje żądania HTTP dla interfejsu API sieci Web.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy projekt. Wybierz **Dodaj nowy element**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

W obszarze **zainstalowane szablony**, rozwiń węzeł **Visual C#** i wybierz **Web**. Z listy szablonów wybierz **klasa formantu API sieci Web**. Nazwa kontrolera "ProductsController", a następnie kliknij przycisk **Dodaj**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

**Dodaj nowy element** Kreator utworzy plik o nazwie ProductsController.cs. Usunięcie metod, które uwzględnione kreatora i dodaj następujące metody:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Aby uzyskać więcej informacji o kodzie, w tym kontrolerze, zobacz [wprowadzenie](tutorial-your-first-web-api.md) samouczka.

## <a name="add-routing-information"></a>Dodawanie informacji o routingu

Teraz, dlatego dodamy trasy identyfikator URI tego identyfikatory URI w postaci &quot;/interfejsAPI/produkty/&quot; są kierowane do kontrolera.

W **Eksploratora rozwiązań**, kliknij dwukrotnie plik Global.asax można otworzyć pliku związanego z kodem Global.asax.cs. Dodaj następujący kod **przy użyciu** instrukcji.

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Następnie dodaj następujący kod do **aplikacji\_Start** metody:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Aby uzyskać więcej informacji o tabelach routingu, zobacz [routingu ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Dodaj AJAX po stronie klienta

To wszystko, czego potrzebujesz do tworzenia internetowego interfejsu API, które klienci mogą uzyskać dostęp. Teraz możemy dodać stronę HTML, która używa technologii jQuery do wywołania interfejsu API.

Upewnić się, że strona wzorcowa (na przykład *Site.Master*) obejmuje `ContentPlaceHolder` z `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Otwórz plik Default.aspx. Zastąp standardowy tekst, który znajduje się w sekcji głównej zawartości, jak pokazano:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

Następnie dodaj odwołanie do pliku źródłowego jQuery w `HeaderContent` sekcji:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Uwaga: Możesz łatwo dodać odwołanie do skryptu, przeciągając i upuszczając go z **Eksploratora rozwiązań** do okna edytora kodu.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Poniżej jQuery tag skryptu Dodaj następujący blok skryptu:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Podczas ładowania dokumentu, ten skrypt tworzy żądanie AJAX &quot;interfejsu api/produktów&quot;. Żądanie zwraca listę produktów, w formacie JSON. Skrypt ten dodaje informacje o produkcie do tabeli HTML.

Po uruchomieniu aplikacji jej powinien wyglądać następująco:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)

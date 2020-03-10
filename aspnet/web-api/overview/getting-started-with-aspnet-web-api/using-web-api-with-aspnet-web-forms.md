---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Korzystanie z interfejsu API sieci Web z ASP.NET Web Forms — ASP.NET 4. x
author: MikeWasson
description: Samouczek z kodem krok po kroku, aby dodać internetowy interfejs API do aplikacji ASP.NET Forms dla ASP.NET 4. x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556778"
---
# <a name="using-web-api-with-aspnet-web-forms"></a>Używanie składnika Web API z formularzami sieci Web platformy ASP.NET

według [Jan Wasson](https://github.com/MikeWasson)

Ten samouczek przeprowadzi Cię przez procedurę dodawania internetowego interfejsu API do tradycyjnej aplikacji ASP.NET Web Forms w ASP.NET 4. x. 

## <a name="overview"></a>Omówienie

Mimo że interfejs API sieci Web ASP.NET jest spakowany przy użyciu ASP.NET MVC, można łatwo dodać internetowy interfejs API do tradycyjnej aplikacji formularzy sieci Web ASP.NET.

Aby korzystać z interfejsu API sieci Web w aplikacji formularzy sieci Web, należy wykonać dwa główne kroki:

- Dodaj kontroler interfejsu API sieci Web pochodzący z klasy **ApiController** .
- Dodaj tabelę tras do metody **Start\_aplikacji** .

## <a name="create-a-web-forms-project"></a>Tworzenie projektu formularzy sieci Web

Uruchom program Visual Studio i wybierz pozycję **Nowy projekt** na stronie **startowej** . Lub w menu **plik** wybierz polecenie **Nowy** , a następnie pozycję **projekt**.

W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  . W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**. Na liście szablonów projektu wybierz pozycję **aplikacja formularzy sieci Web ASP.NET**. Wprowadź nazwę projektu i kliknij przycisk **OK**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Tworzenie modelu i kontrolera

Ten samouczek używa tych samych klas modelu i kontrolera co samouczek [wprowadzenie](tutorial-your-first-web-api.md) .

Najpierw Dodaj klasę modelu. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj klasę**. Nadaj klasie produktowi i Dodaj następującą implementację:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Następnie Dodaj kontroler interfejsu API sieci Web do projektu. *kontroler* jest obiektem, który obsługuje żądania HTTP dla internetowego interfejsu API.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt. Wybierz pozycję **Dodaj nowy element**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

W obszarze **zainstalowane szablony**rozwiń **pozycję C# Wizualizacja** i wybierz pozycję **Sieć Web**. Następnie z listy szablonów wybierz pozycję **Klasa kontrolera interfejsu API sieci Web**. Nazwij kontroler "ProductsController" i kliknij przycisk **Dodaj**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

Kreator **dodawania nowego elementu** utworzy plik o nazwie ProductsController.cs. Usuń metody dołączone do kreatora i Dodaj następujące metody:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Aby uzyskać więcej informacji na temat kodu w tym kontrolerze, zobacz samouczek [wprowadzenie](tutorial-your-first-web-api.md) .

## <a name="add-routing-information"></a>Dodawanie informacji o routingu

Następnie dodamy trasę identyfikatora URI, aby identyfikatory URI formularza &quot;/API/Products/&quot; są kierowane do kontrolera.

W **Eksplorator rozwiązań**kliknij dwukrotnie pozycję Global. asax, aby otworzyć plik związany z kodem Global.asax.cs. Dodaj następującą instrukcję **using** .

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Następnie Dodaj następujący kod do **aplikacji\_Start** :

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Aby uzyskać więcej informacji na temat tabel routingu, zobacz [Routing in Web API ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Dodawanie technologii AJAX po stronie klienta

To wszystko, co jest potrzebne do utworzenia internetowego interfejsu API, do którego klienci mogą uzyskać dostęp. Teraz dodamy do wywołania interfejsu API stronę HTML, która używa technologii jQuery.

Upewnij się, że strona wzorcowa (na przykład *site. Master*) zawiera `ContentPlaceHolder` z `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Otwórz plik default. aspx. Zastąp tekst standardowy, który znajduje się w sekcji głównej zawartości, jak pokazano poniżej:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

Następnie Dodaj odwołanie do pliku źródłowego jQuery w sekcji `HeaderContent`:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Uwaga: możesz łatwo dodać odwołanie do skryptu, przeciągając i upuszczając plik z **Eksplorator rozwiązań** do okna edytora kodu.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Poniżej tagu skryptu jQuery Dodaj następujący blok skryptu:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Po załadowaniu dokumentu ten skrypt wykonuje żądanie AJAX, aby &quot;&quot;API/produkty. Żądanie zwraca listę produktów w formacie JSON. Skrypt dodaje informacje o produkcie do tabeli HTML.

Po uruchomieniu aplikacji powinna wyglądać następująco:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)

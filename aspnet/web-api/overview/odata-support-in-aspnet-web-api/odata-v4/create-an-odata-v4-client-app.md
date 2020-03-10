---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Tworzenie aplikacji klienckiej OData v4 (C#) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: a0016cf2cc7bffe6268664395ccb38e140090310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556295"
---
# <a name="create-an-odata-v4-client-app-c"></a>Tworzenie aplikacji klienckiej OData 4 (C#)

według [Jan Wasson](https://github.com/MikeWasson)

W poprzednim samouczku utworzono podstawową usługę OData, która obsługuje operacje CRUD. Teraz Utwórzmy klienta usługi.

Uruchom nowe wystąpienie programu Visual Studio i Utwórz nowy projekt aplikacji konsolowej. W oknie dialogowym **Nowy projekt** wybierz pozycję **zainstalowane** &gt; **Szablony** &gt; **Visual C#**  &gt; **Windows Desktop**, a następnie wybierz szablon **aplikacja konsoli** . Nazwij projekt &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> Możesz również dodać aplikację konsolową do tego samego rozwiązania programu Visual Studio, które zawiera usługę OData.

## <a name="install-the-odata-client-code-generator"></a>Instalowanie generatora kodów klienta OData

W menu **Narzędzia** wybierz pozycję **rozszerzenia i aktualizacje**. Wybierz pozycję **Online** &gt; **Visual Studio Gallery**. W polu wyszukiwania Wyszukaj &quot;&quot;generatora kodu klienta OData. Kliknij przycisk **Pobierz** , aby zainstalować VSIX. Może zostać wyświetlony monit o ponowne uruchomienie programu Visual Studio.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Uruchom lokalnie usługę OData

Uruchom projekt ProductService z programu Visual Studio. Domyślnie program Visual Studio uruchamia przeglądarkę w katalogu głównym aplikacji. Zanotuj identyfikator URI; będzie ona potrzebna w następnym kroku. Pozostaw uruchomioną aplikację.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> W przypadku umieszczenia obu projektów w tym samym rozwiązaniu upewnij się, że projekt ProductService został uruchomiony bez debugowania. W następnym kroku należy zachować uruchomioną usługę podczas modyfikowania projektu aplikacji konsolowej.

## <a name="generate-the-service-proxy"></a>Generuj serwer proxy usługi

Serwer proxy usługi jest klasą .NET, która definiuje metody uzyskiwania dostępu do usługi OData. Serwer proxy tłumaczy wywołania metod na żądania HTTP. Klasa proxy zostanie utworzona przez uruchomienie [szablonu T4](https://msdn.microsoft.com/library/bb126445.aspx).

Kliknij prawym przyciskiem myszy projekt. Wybierz pozycję **dodaj** &gt; **nowy element**.

![](create-an-odata-v4-client-app/_static/image5.png)

W oknie dialogowym **Dodaj nowy element** wybierz pozycję **elementy C# wizualne** &gt; **kod** &gt; **klienta OData**. Nazwij szablon &quot;ProductClient.tt&quot;. Kliknij przycisk **Dodaj** i kliknij ostrzeżenie o zabezpieczeniach.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

W tym momencie zostanie wyświetlony komunikat o błędzie, który można zignorować. Program Visual Studio automatycznie uruchamia szablon, ale w pierwszej kolejności szablon wymaga pewnych ustawień konfiguracji.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Otwórz plik ProductClient. OData. config. W elemencie `Parameter` Wklej w identyfikatorze URI z projektu ProductService (poprzedni krok). Na przykład:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Uruchom ponownie szablon. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy plik ProductClient.tt i wybierz polecenie **Uruchom narzędzie niestandardowe**.

Szablon tworzy plik kodu o nazwie ProductClient.cs, który definiuje serwer proxy. Podczas tworzenia aplikacji, jeśli zmienisz punkt końcowy OData, uruchom ponownie szablon, aby zaktualizować serwer proxy.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Używanie serwera proxy usługi do wywoływania usługi OData

Otwórz plik Program.cs i Zastąp kod standardowy następującym.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Zastąp wartość *SERVICEURI* identyfikatorem URI usługi wcześniejszym.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Po uruchomieniu aplikacji należy wykonać następujące czynności:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]

---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Część 6: ASP.NET Membership | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 6 dodaje członkostwo ASP.NET.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: b0caa89dc9ffb5bb7451fa2d9d346c7db2bf1466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564184"
---
# <a name="part-6-aspnet-membership"></a>Część 6. Członkostwo platformy ASP.NET

Jan [Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.
> 
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 6 dodaje członkostwo ASP.NET.

## <a id="_Toc260221672"></a>Praca z członkostwem ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Kliknij pozycję Zabezpieczenia

![](tailspin-spyworks-part-6/_static/image1.jpg)

Upewnij się, że korzystamy z uwierzytelniania formularzy.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Użyj linku "Utwórz użytkownika", aby utworzyć kilku użytkowników.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Po zakończeniu zapoznaj się z oknem Eksplorator rozwiązań i Odśwież widok.

![](tailspin-spyworks-part-6/_static/image2.png)

Należy pamiętać, że ASPNETDB. Została utworzona dobra MDF. Ten plik zawiera tabele, które obsługują podstawowe usługi ASP.NET, takie jak członkostwo.

Teraz możemy zacząć implementować proces wyewidencjonowania.

Zacznij od utworzenia strony wyewidencjonowywania. aspx.

Strona wyewidencjonowywanie. aspx powinna być dostępna tylko dla użytkowników, którzy są zalogowani, aby ograniczyć dostęp do zalogowanych użytkowników i przekierować użytkowników, którzy nie są zalogowani na stronie logowania.

W tym celu dodamy następujący plik do sekcji konfiguracji naszego pliku Web. config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Szablon aplikacji formularzy sieci Web ASP.NET automatycznie dodaje sekcję uwierzytelniania do pliku Web. config i ustanowił domyślną stronę logowania.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Przed zalogowaniem się użytkownika należy zmodyfikować kod login. aspx związany z plikiem. Zmień\_zdarzeń ładowania strony w następujący sposób.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Następnie Dodaj procedurę obsługi zdarzeń "zalogowany", taką jak ta, aby ustawić nazwę sesji dla nowo zalogowanego użytkownika i zmienić tymczasowy identyfikator sesji w koszyku na użytkownika przez wywołanie metody MigrateCart w naszej klasie MyShoppingCart. (Zaimplementowane w pliku CS)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Zaimplementuj metodę MigrateCart () w następujący sposób.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

W obszarze wyewidencjonowywanie. aspx będziemy używać obiektu EntityDataSource i widoku GridView na naszej stronie wyewidencjonowywania, podobnie jak na naszej stronie koszyka zakupów.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Należy zauważyć, że nasz formant GridView określa procedurę obsługi zdarzeń "OnDataBound" o nazwie Moje listy\_RowDataBound, aby umożliwić wdrożenie tego programu obsługi zdarzeń, takiego jak to.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Ta metoda przechowuje sumę uruchamiania koszyka, ponieważ każdy wiersz jest powiązany, i aktualizuje dolny wiersz widoku GridView.

Na tym etapie wprowadziliśmy prezentację "przegląd" kolejności, w której ma zostać wykonane.

Aby obsłużyć pusty scenariusz, Dodaj kilka wierszy kodu do naszej strony,\_zdarzenie ładowania:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Gdy użytkownik kliknie przycisk "Prześlij", zostanie wykonany następujący kod na przycisku Prześlij kliknij procedurę obsługi zdarzeń.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

"Mięso" procesu przekazywania zamówienia należy zaimplementować w metodzie SubmitOrder () klasy MyShoppingCart.

SubmitOrder:

- Wypełnij wszystkie elementy wiersza w koszyku i używaj ich do tworzenia nowego rekordu zamówienia i skojarzonych rekordów OrderDetails.
- Oblicz datę wysyłki.
- Wyczyść koszyk.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Na potrzeby tej przykładowej aplikacji obliczą datę wysyłki przez dodanie dwóch dni do bieżącej daty.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Uruchomienie aplikacji umożliwi nam przetestowanie procesu zakupów od początku do końca.

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-5.md)
> [dalej](tailspin-spyworks-part-7.md)

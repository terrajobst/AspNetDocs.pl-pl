---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Mapowanie użytkowników sygnalizujących do połączeń | Microsoft Docs
author: bradygaster
description: W tym temacie pokazano, jak zachować informacje o użytkownikach i ich połączeniach. W tym temacie pomogła Ci zanotować ten temat. Wersje oprogramowania używane w tym temacie...
ms.author: bradyg
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: d55d40848e1e9d40570850c3552b225235c5e814
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536779"
---
# <a name="mapping-signalr-users-to-connections"></a>Mapowanie użytkowników usługi SignalR na połączenia

Autor [FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym temacie pokazano, jak zachować informacje o użytkownikach i ich połączeniach.
>
> W tym temacie pomogła Ci zanotować ten temat.
>
> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używane w tym temacie
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sygnalizujący wersja 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Poprzednie wersje tego tematu
>
> Aby uzyskać informacje o wcześniejszych wersjach programu sygnalizującego, zobacz sekcję [sygnalizujące starsze wersje](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Pytania i Komentarze
>
> Prosimy o opinię na temat sposobu, w jaki lubię ten samouczek, i co możemy ulepszyć w komentarzach w dolnej części strony. Jeśli masz pytania, które nie są bezpośrednio związane z samouczkiem, możesz je ogłosić na [forum ASP.NET](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).

## <a name="introduction"></a>Wprowadzenie

Każdy Klient nawiązujący połączenie z centrum przekazuje unikatowy identyfikator połączenia. Tę wartość można pobrać we właściwości `Context.ConnectionId` kontekstu centrum. Jeśli aplikacja musi zmapować użytkownika na identyfikator połączenia i zachować to mapowanie, można użyć jednej z następujących czynności:

- [Dostawca identyfikatora użytkownika (sygnalizujący 2)](#IUserIdProvider)
- [Magazyn w pamięci](#inmemory), taki jak słownik
- [Grupa sygnałów dla każdego użytkownika](#groups)
- [Trwały, zewnętrzny magazyn](#database), taki jak tabela bazy danych lub usługa Azure Table Storage

Każda z tych implementacji jest przedstawiona w tym temacie. Możesz użyć metod `OnConnected`, `OnDisconnected`i `OnReconnected` klasy `Hub` do śledzenia stanu połączenia użytkownika.

Najlepsze podejście do aplikacji zależy od następujących:

- Liczba serwerów sieci Web, które obsługują aplikację.
- Czy musisz uzyskać listę aktualnie połączonych użytkowników.
- Czy po ponownym uruchomieniu aplikacji lub serwera należy zachować informacje o grupach i użytkownikach.
- Czy opóźnienie wywołania serwera zewnętrznego jest problemem.

W poniższej tabeli przedstawiono podejście, które ma zastosowanie do tych zagadnień.

|  | Więcej niż jeden serwer | Pobierz listę aktualnie połączonych użytkowników | Utrwalaj informacje po ponownym uruchomieniu | Optymalna wydajność |
| --- | --- | --- | --- | --- |
| Dostawca UserID | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| W pamięci |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Grupy pojedynczego użytkownika | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Stałe, zewnętrzne | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>Dostawca IUserID

Ta funkcja umożliwia użytkownikom określenie, jakie elementy userId bazują na IRequest za pośrednictwem nowego interfejsu IUserIdProvider.

**IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

Domyślnie zostanie zaimplementowana implementacja, która używa `IPrincipal.Identity.Name` użytkownika jako nazwy użytkownika. Aby to zmienić, zarejestruj implementację `IUserIdProvider` z hostem globalnym podczas uruchamiania aplikacji:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

Z poziomu centrum będziesz w stanie wysyłać komunikaty do tych użytkowników za pośrednictwem następującego interfejsu API:

**Wysyłanie komunikatu do określonego użytkownika**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Magazyn w pamięci

W poniższych przykładach pokazano, jak zachować połączenie i informacje o użytkowniku w słowniku, który jest przechowywany w pamięci. Słownik używa `HashSet` do przechowywania identyfikatora połączenia. W dowolnym momencie użytkownik może mieć więcej niż jedno połączenie z aplikacją sygnalizującej. Na przykład użytkownik połączony za pomocą wielu urządzeń lub więcej niż jedna karta przeglądarki ma więcej niż jeden identyfikator połączenia.

Jeśli aplikacja zostanie ZAMKNIĘTA, wszystkie informacje zostaną utracone, ale zostanie ponownie wypełnione, gdy użytkownicy ponownie ustalą swoje połączenia. Magazyn w pamięci nie działa, jeśli środowisko zawiera więcej niż jeden serwer sieci Web, ponieważ każdy serwer będzie miał osobną kolekcję połączeń.

Pierwszy przykład przedstawia klasę, która zarządza mapowaniem użytkowników do połączeń. Kluczem dla HashSet — będzie nazwa użytkownika.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

W następnym przykładzie pokazano, jak używać klasy mapowania połączeń z poziomu centrum. Wystąpienie klasy jest przechowywane w nazwie zmiennej `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Grupy pojedynczego użytkownika

Możesz utworzyć grupę dla każdego użytkownika, a następnie wysłać wiadomość do tej grupy, gdy chcesz uzyskać dostęp tylko do tego użytkownika. Nazwa każdej grupy to nazwa użytkownika. Jeśli użytkownik ma więcej niż jedno połączenie, każdy identyfikator połączenia zostanie dodany do grupy użytkownika.

Nie należy ręcznie usuwać użytkownika z grupy, gdy nastąpi odłączenie użytkownika. Ta akcja jest wykonywana automatycznie przez platformę sygnalizującą.

Poniższy przykład pokazuje, jak zaimplementować grupy pojedynczego użytkownika.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Trwały magazyn zewnętrzny

W tym temacie przedstawiono sposób korzystania z bazy danych lub magazynu tabel platformy Azure do przechowywania informacji o połączeniu. Takie podejście działa, gdy istnieje wiele serwerów sieci Web, ponieważ każdy serwer sieci Web może współdziałać z tym samym repozytorium danych. Jeśli serwery sieci Web przestaną działać lub aplikacja zostanie uruchomiona ponownie, Metoda `OnDisconnected` nie zostanie wywołana. W związku z tym, istnieje możliwość, że repozytorium danych będzie zawierało rekordy identyfikatorów połączeń, które nie są już prawidłowe. Aby wyczyścić te oddzielone rekordy, możesz unieważnić wszelkie połączenia, które zostały utworzone poza przedziałem czasu, który jest istotny dla aplikacji. Przykłady w tej sekcji zawierają wartość śledzenia podczas tworzenia połączenia, ale nie pokazują, jak oczyścić stare rekordy, ponieważ warto to zrobić jako proces w tle.

### <a name="database"></a>Baza danych

W poniższych przykładach pokazano, jak zachować połączenie i informacje o użytkowniku w bazie danych. Możesz użyć dowolnej technologii dostępu do danych. w poniższym przykładzie pokazano, jak definiować modele przy użyciu Entity Framework. Te modele jednostek odpowiadają tabelom i polom bazy danych. Struktura danych może się znacznie różnić w zależności od wymagań aplikacji.

Pierwszy przykład pokazuje, jak zdefiniować jednostkę użytkownika, która może być skojarzona z wieloma jednostkami połączenia.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

Następnie z poziomu centrum można śledzić stan każdego połączenia z kodem pokazanym poniżej.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Azure Table Storage

Poniższy przykład usługi Azure Table Storage jest podobny do przykładu bazy danych. Nie obejmuje to wszystkich informacji, które należy rozpocząć od usługi Azure Table Storage. Aby uzyskać więcej informacji, zobacz [jak używać magazynu tabel z platformy .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

Poniższy przykład przedstawia jednostkę tabeli służącą do przechowywania informacji o połączeniu. Umożliwia ona Partycjonowanie danych według nazwy użytkownika i identyfikuje poszczególne jednostki według identyfikatora połączenia, dzięki czemu użytkownik może mieć wiele połączeń w dowolnym momencie.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

W centrum jest śledzony stan połączenia poszczególnych użytkowników.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]

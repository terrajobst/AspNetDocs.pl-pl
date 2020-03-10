---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Praca z grupami w sygnalizacji | Microsoft Docs
author: bradygaster
description: W tym temacie opisano sposób utrwalania informacji o członkostwie w grupie za pomocą interfejsu API centrum.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 46dd952fc4902b37c981a111dfa344dad79bb668
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558584"
---
# <a name="working-with-groups-in-signalr"></a>Praca z grupami w usłudze SignalR

[Fletcher Patryk](https://github.com/pfletcher), [Tomasz FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym temacie opisano sposób dodawania użytkowników do grup i utrwalania informacji o członkostwie w grupach.
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

## <a name="overview"></a>Omówienie

Grupy w sygnalizacji zapewniają metodę rozgłaszania komunikatów do określonych podzestawów połączonych klientów. Grupa może mieć dowolną liczbę klientów, a klient może być członkiem dowolnej liczby grup. Nie musisz jawnie tworzyć grup. W efekcie Grupa jest tworzona automatycznie przy pierwszym określeniu jej nazwy w wywołaniu groups. Add i jest usuwana po usunięciu ostatniego połączenia z członkostwa w nim. Aby zapoznać się z wprowadzeniem do korzystania z grup, zobacz [jak zarządzać członkostwem w grupie z klasy Hub](hubs-api-guide-server.md#groupsfromhub) w podręczniku API Hubs-Server.

Brak interfejsu API do uzyskiwania listy członkostwa w grupie lub listy grup. Program sygnalizujący wysyła komunikaty do klientów i grup na podstawie modelu pub/sub, a serwer nie zachowuje list grup ani członkostw w grupach. Pozwala to zmaksymalizować skalowalność, ponieważ po dodaniu węzła do kolektywu serwerów sieci Web, każdy stan, który utrzymuje usługa sygnalizująca, musi być propagowany do nowego węzła.

Po dodaniu użytkownika do grupy przy użyciu metody `Groups.Add` użytkownik otrzymuje komunikaty skierowane do tej grupy na czas trwania bieżącego połączenia, ale członkostwo użytkownika w tej grupie nie jest utrwalane poza bieżącym połączeniem. Jeśli chcesz trwale zachować informacje o grupach i członkostwie w grupie, musisz przechowywać te dane w repozytorium, takim jak baza danych lub Azure Table Storage. Następnie za każdym razem, gdy użytkownik nawiązuje połączenie z aplikacją, pobiera z repozytorium, do którego należy Grupa, i ręcznie Dodaj tego użytkownika do tych grup.

Po ponownym nawiązaniu połączenia po tymczasowym zakłóceniu użytkownik automatycznie ponownie przyłącza do wcześniej przypisanych grup. Automatyczne ponowne sprzęganie grupy ma zastosowanie tylko w przypadku ponownego połączenia, a nie podczas ustanawiania nowego połączenia. Token podpisany cyfrowo jest przesyłany z klienta, który zawiera listę wcześniej przypisanych grup. Jeśli chcesz sprawdzić, czy użytkownik należy do żądanych grup, można zastąpić zachowanie domyślne.

Ten temat zawiera następujące sekcje:

- [Dodawanie i usuwanie użytkowników](#add)
- [Wywoływanie elementów członkowskich grupy](#call)
- [Przechowywanie członkostwa w grupie w bazie danych](#storedatabase)
- [Przechowywanie członkostwa w grupie w usłudze Azure Table Storage](#storeazuretable)
- [Weryfikowanie członkostwa w grupie podczas ponownego nawiązywania połączenia](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Dodawanie i usuwanie użytkowników

Aby dodać lub usunąć użytkowników z grupy, należy wywołać metody [dodawania](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) lub [usuwania](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) , a także przekazać identyfikator połączenia użytkownika i nazwę grupy jako parametry. Po zakończeniu połączenia nie trzeba ręcznie usuwać użytkownika z grupy.

W poniższym przykładzie przedstawiono metody `Groups.Add` i `Groups.Remove` używane w metodach centrum.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

Metody `Groups.Add` i `Groups.Remove` wykonują asynchronicznie.

Aby dodać klienta do grupy i natychmiast wysłać komunikat do klienta przy użyciu grupy, należy się upewnić, że metoda Groups. Add została najpierw ukończona. Poniższe przykłady kodu pokazują, jak to zrobić.

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

Ogólnie rzecz biorąc nie należy uwzględniać `await` podczas wywoływania metody `Groups.Remove`, ponieważ identyfikator połączenia, który próbujesz usunąć, nie jest już dostępny. W takim przypadku `TaskCanceledException` jest generowany po upływie limitu czasu żądania. Jeśli aplikacja musi upewnić się, że użytkownik został usunięty z grupy przed wysłaniem komunikatu do grupy, można dodać `await` przed `Groups.Remove`, a następnie przechwytywać `TaskCanceledException` wyjątek, który może zostać wygenerowany.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Wywoływanie elementów członkowskich grupy

Można wysyłać wiadomości do wszystkich członków grupy lub tylko do określonych członków grupy, jak pokazano w poniższych przykładach.

- **Wszyscy** połączeni klienci w określonej grupie.

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- Wszyscy połączeni klienci w określonej grupie **z wyjątkiem określonych klientów**identyfikowane przez identyfikator połączenia.

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- Wszyscy połączeni klienci w określonej grupie **z wyjątkiem klienta wywołującego**.

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Przechowywanie członkostwa w grupie w bazie danych

W poniższych przykładach pokazano, jak zachować informacje o grupach i użytkownikach w bazie danych. Możesz użyć dowolnej technologii dostępu do danych. w poniższym przykładzie pokazano, jak definiować modele przy użyciu Entity Framework. Te modele jednostek odpowiadają tabelom i polom bazy danych. Struktura danych może się znacznie różnić w zależności od wymagań aplikacji. Ten przykład zawiera klasę o nazwie `ConversationRoom`, która może być unikatowa dla aplikacji, która umożliwia użytkownikom dołączanie do konwersacji dotyczących różnych tematów, takich jak sport lub ogród. Ten przykład zawiera również klasę dla połączeń. Klasa połączenia nie jest absolutnie wymagana do śledzenia członkostwa w grupach, ale jest często częścią niezawodnego rozwiązania do śledzenia użytkowników.

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

Następnie w centrum można pobrać informacje o grupach i użytkownikach z bazy danych i ręcznie dodać użytkownika do odpowiednich grup. Przykład nie zawiera kodu do śledzenia połączeń użytkownika. W tym przykładzie słowo kluczowe `await` nie jest stosowane przed `Groups.Add`, ponieważ komunikat nie jest natychmiast wysyłany do członków grupy. Jeśli chcesz wysłać wiadomość do wszystkich członków grupy natychmiast po dodaniu nowego elementu członkowskiego, należy zastosować słowo kluczowe `await`, aby upewnić się, że operacja asynchroniczna została ukończona.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Przechowywanie członkostwa w grupie w usłudze Azure Table Storage

Korzystanie z usługi Azure Table Storage do przechowywania informacji o grupach i użytkownikach jest podobne do korzystania z bazy danych. Poniższy przykład pokazuje jednostkę tabeli przechowującą nazwę użytkownika i nazwę grupy.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

W centrum należy pobrać przypisane grupy, gdy użytkownik nawiązuje połączenie.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Weryfikowanie członkostwa w grupie podczas ponownego nawiązywania połączenia

Domyślnie program sygnalizujący automatycznie ponownie przypisuje użytkownika do odpowiednich grup podczas ponownego nawiązywania połączenia po tymczasowym przerwaniu, na przykład w przypadku porzucenia i ponownego ustanowienia połączenia przed upływem limitu czasu. Informacje o grupie użytkownika są przesyłane w tokenie podczas ponownego nawiązywania połączenia, a ten token jest weryfikowany na serwerze. Aby uzyskać informacje na temat procesu weryfikacji dla ponownych dołączania użytkowników do grup, zobacz [ponowne łączenie grup po ponownym połączeniu](../security/introduction-to-security.md#rejoingroup).

Ogólnie rzecz biorąc, należy użyć domyślnego zachowania automatycznego ponownego przyłączania grup przy ponownym połączeniu. Grupy sygnałów nie stanowią mechanizmu zabezpieczeń w celu ograniczenia dostępu do poufnych danych. Jeśli jednak aplikacja musi mieć podwójne sprawdzenie członkostwa w grupie użytkownika podczas ponownego nawiązywania połączenia, można zastąpić zachowanie domyślne. Zmiana domyślnego zachowania może zwiększyć obciążenie bazy danych, ponieważ członkostwo w grupie użytkownika musi być pobrane dla każdego ponownego połączenia, a nie tylko wtedy, gdy użytkownik nawiązuje połączenie.

Jeśli konieczne jest zweryfikowanie członkostwa w grupie przy ponownym nawiązaniu połączenia, Utwórz nowy moduł potoku centrum, który zwraca listę przypisanych grup, jak pokazano poniżej.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

Następnie Dodaj ten moduł do potoku centrum, jak pokazano poniżej.

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]

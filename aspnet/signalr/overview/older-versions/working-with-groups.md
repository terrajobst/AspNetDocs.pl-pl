---
uid: signalr/overview/older-versions/working-with-groups
title: Praca z grupami w SignalR 1.x | Dokumentacja firmy Microsoft
author: bradygaster
description: W tym temacie opisano, jak można utrwalić informacje o członkostwie w grupie przy użyciu interfejsu API Centrum.
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: a606f74ee97c92b89e0137e2c9600a3424115d5e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59418823"
---
# <a name="working-with-groups-in-signalr-1x"></a>Praca z grupami w usłudze SignalR 1.x

przez [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> W tym temacie opisano, jak dodać użytkowników do grup i zachować informacje o członkostwie w grupie.


## <a name="overview"></a>Omówienie

Grupami w SignalR udostępnia metody emisji komunikatów określony podzbiór połączonych klientów. Grupa może zawierać dowolną liczbę klientów, a klient może należeć do dowolnej liczby grup. Nie trzeba jawnie tworzyć grupy. W efekcie grupa zostanie utworzona automatycznie określić jego nazwę w wywołaniu Groups.Add po raz pierwszy, a zostanie usunięta po usunięciu ostatniego połączenia z członkostwa w nim. Wprowadzenie do korzystania z grup, zobacz [sposób zarządzania członkostwa w grupie z klasy koncentratora](index.md) w interfejsie API centrów — serwer przewodnik.

Nie ma żadnych interfejsów API w celu uzyskania listy członkostwa grupy lub Podaj listę grup. SignalR wysyła wiadomości do klientów i grup oparty na modelu publikowania/subskrybowania, a serwer nie przechowuje listę grup lub członkostwa w grupach. Pozwala to zmaksymalizować skalowalność, ponieważ po każdym dodaniu węzła w farmie sieci web, stan, który przechowuje SignalR są propagowane do nowego węzła.

Po dodaniu użytkownika do grupy przy użyciu `Groups.Add` metody, użytkownik otrzymuje wiadomości kierowane do tej grupy na czas trwania bieżącego połączenia, ale członkostwo użytkownika w tej grupie nie jest trwały poza bieżącym połączeniu. Jeśli chcesz trwale przechowywane informacje o grupach oraz członkostwa w grupie, dane muszą być przechowywane w repozytorium, takich jak bazy danych lub usługi Azure table storage. Następnie każdorazowo, gdy użytkownik łączy się z aplikacją, możesz pobrać z repozytorium użytkownik należy do grupy i ręcznie dodać tego użytkownika do tych grup.

Podczas ponownego łączenia po przerwaniu tymczasowe, użytkownik automatycznie ponownie łączy wcześniej przypisane grupy. Automatyczne ponowne przyłączanie grup tylko wtedy, gdy ponowne nawiązywanie połączenia, nie ustanawiania nowego połączenia. Token podpisane cyfrowo są przekazywane z klienta, który zawiera listy uprzednio przypisanych grup. Jeśli chcesz sprawdzić, czy użytkownik należy do żądanej grupy, można zastąpić domyślne zachowanie.

Ten temat zawiera następujące sekcje:

- [Dodawanie i usuwanie użytkowników](#add)
- [Wywoływanie elementów członkowskich grupy](#call)
- [Przechowywanie członkostwa w grupie w bazie danych](#storedatabase)
- [Zapisywanie członkostwa w grupach w usłudze Azure table storage](#storeazuretable)
- [Sprawdzanie członkostwa w grupie przy ponownym łączeniu](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Dodawanie i usuwanie użytkowników

Aby dodać lub usunąć użytkowników z grupy, należy wywołać [Dodaj](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) lub [Usuń](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) metod i przekaż identyfikator połączenia użytkownika oraz nazwę grupy jako parametry. Nie trzeba ręcznie usunąć użytkownika z grupy, po zakończeniu połączenia.

W poniższym przykładzie przedstawiono `Groups.Add` i `Groups.Remove` metody używane w metodach koncentratora.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

`Groups.Add` i `Groups.Remove` metody są wykonywane asynchronicznie.

Jeśli chcesz dodać do grupy klienta i natychmiast wysłać wiadomość do klienta przy użyciu grupy, należy upewnić się, że metoda Groups.Add zakończy się pierwsza. W poniższych przykładach kodu pokazano, jak to zrobić, za pomocą kodu, który działa w .NET 4.5 i za pomocą kodu, który działa w programie .NET 4.

#### <a name="asynchronous-net-45-example"></a>Asynchroniczne .NET 4.5 przykład

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a>Asynchroniczne .NET 4 przykład

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

Ogólnie rzecz biorąc, nie należy używać `await` podczas wywoływania `Groups.Remove` metody, ponieważ identyfikator połączenia, który próbujesz usunąć przestaną być dostępne. W takim przypadku `TaskCanceledException` jest zgłaszany po upłynie limit czasu żądania. Jeśli aplikacja musi zapewnić, że użytkownik został usunięty z grupy przed wysłaniem wiadomości do grupy, możesz dodać `await` przed Groups.Remove, a następnie catch `TaskCanceledException` wyjątek, który może zostać wygenerowany.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Wywoływanie elementów członkowskich grupy

Wiadomości można wysyłać do wszystkich członków grupy lub tylko członkowie określonej grupy, jak pokazano w poniższych przykładach.

- **Wszystkie** połączonych klientów w określonej grupie. 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- Wszyscy połączeni klienci w określonej grupie **oprócz określonych klientów**, zidentyfikowane przez identyfikator połączenia. 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- Wszyscy połączeni klienci w określonej grupie **oprócz klienta wywołującego**. 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Przechowywanie członkostwa w grupie w bazie danych

Następujące przykłady przedstawiają sposób przechowywania informacji grupy i użytkownika w bazie danych. Możesz użyć dowolnej technologii dostępu do danych; Jednak w poniższym przykładzie pokazano sposób definiowania modeli za pomocą platformy Entity Framework. Modele te jednostki odpowiadają tabel bazy danych i pola. Do struktury danych może być bardzo zróżnicowana w zależności od wymagań aplikacji. Ten przykład zawiera klasę o nazwie `ConversationRoom` będzie unikatowy dla aplikacji, która pozwala użytkownikom na dołączanie do rozmowy dotyczące różnych tematów, takich jak sportu lub ogród. W tym przykładzie zawiera również klasy dla połączeń. Klasa połączenia nie jest bezwzględnie wymagane dla śledzenia członkostwa w grupie, ale często jest częścią niezawodne rozwiązanie do śledzenia użytkowników.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

Następnie w piaście, możesz pobrać grupy i użytkownika informacji z bazy danych i ręcznie dodać użytkownika do odpowiednich grup. Przykład zawiera kod śledzenia połączeń użytkowników. W tym przykładzie `await` — słowo kluczowe nie jest stosowane przed `Groups.Add` ponieważ wiadomość nie są natychmiast wysyłane do członków grupy. Jeśli chcesz wysłać wiadomość do wszystkich elementów członkowskich grupy bezpośrednio po dodaniu nowego elementu członkowskiego, będzie chciała zastosować `await` — słowo kluczowe, aby upewnić się, operacja asynchroniczna została zakończona.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Zapisywanie członkostwa w grupach w usłudze Azure table storage

Za pomocą usługi Azure table storage do przechowywania informacji o grupy i użytkownika jest podobne do bazy danych. Poniższy przykład pokazuje jednostkę tabeli, która przechowuje nazwę użytkownika i nazwę grupy.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

W Centrum możesz pobrać przypisanych grup, gdy użytkownik nawiązuje połączenie.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Sprawdzanie członkostwa w grupie przy ponownym łączeniu

Domyślnie SignalR automatycznie ponownie przypisuje użytkownika do odpowiednich grup przy ponownym łączeniu zakłóceniach tymczasowej, np. podczas połączenia jest usunięty i ponownie nawiązane przed upływem limitu czasu połączenia. Informacje o grupie użytkowników jest przekazywany w tokenie, gdy ponowne nawiązywanie połączenia, a ten token jest weryfikowany na serwerze. Aby dowiedzieć się, proces weryfikacji przystąpienie użytkowników do grup, zobacz [ponowne przyłączanie grup przy ponownym łączeniu](index.md).

Ogólnie rzecz biorąc należy użyć domyślne zachowanie automatyczne ponowne przyłączanie, ponowne łączenie grup na. SignalR grup nie jest przeznaczona mechanizmu zabezpieczeń w celu ograniczania dostępu do poufnych danych. Jednak w przypadku aplikacji należy dokładnie sprawdzić członkostwa w grupie użytkowników, przy ponownym łączeniu, zachowanie domyślne można przesłonić. Zmiana domyślnego zachowania można dodać obciążenie do bazy danych, ponieważ członkostwo w grupie użytkownika musi zostać pobrany dla każdego ponowne nawiązanie połączenia, a nie tylko wtedy, gdy użytkownik połączy się.

Jeśli musisz sprawdzić członkostwa w grupie na ponowne łączenie, utworzyć nowy moduł potoku koncentratora, zwracającej listę przypisanych grup, jak pokazano poniżej.

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

Następnie dodaj ten moduł do potoku koncentratora, jak wyróżniono poniżej.

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]

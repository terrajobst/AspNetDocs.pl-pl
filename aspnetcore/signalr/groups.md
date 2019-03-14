---
title: Zarządzanie użytkownikami i grupami w SignalR
author: bradygaster
description: Omówienie platformy ASP.NET Core SignalR użytkowników i grup zarządzania.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 45f2bb44e03a586b7fc186525fdd3a2645c820d5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57067676"
---
# <a name="manage-users-and-groups-in-signalr"></a>Zarządzanie użytkownikami i grupami w SignalR

Przez [Brennan Conroy](https://github.com/BrennanConroy)

SignalR umożliwia wiadomości do wysłania do wszystkie połączenia skojarzone z określonym użytkownikiem, a także do nazwanych grup połączeń.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [(jak pobrać)](xref:index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>Użytkownicy w SignalR

SignalR umożliwia wysyłanie komunikatów do wszystkich połączeń, skojarzone z określonym użytkownikiem. Domyślnie używa SignalR `ClaimTypes.NameIdentifier` z `ClaimsPrincipal` skojarzonych z tym połączeniem jako identyfikator użytkownika. Jeden użytkownik może mieć wiele połączeń aplikacji SignalR. Można na przykład połączenia użytkownika, na pulpicie, a także swojego telefonu. Każde urządzenie ma oddzielne połączenia SignalR, ale są one wszystkie skojarzone z tym użytkownikiem. Jeśli komunikat jest wysyłany do użytkownika, wszystkie połączenia skojarzone z tym użytkownikiem komunikat. Identyfikator użytkownika dla połączenia może zostać oceniony przez `Context.UserIdentifier` właściwości Centrum.

Wyślij wiadomość do określonego użytkownika, przekazując identyfikator użytkownika, aby `User` działać w metodzie Centrum, jak pokazano w poniższym przykładzie:

> [!NOTE]
> Identyfikator użytkownika jest rozróżniana wielkość liter.

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-signalr"></a>Grupami w SignalR

Grupy to zbiór połączenia skojarzone z nazwą. Komunikaty mogą być wysyłane do wszystkich połączeń w grupie. Grupy są zalecanym sposobem wysyłania do połączenia lub wielu połączeń, ponieważ te grupy są zarządzane przez aplikację. Połączenie może należeć do wielu grup. Dzięki temu grupy idealne podobny aplikację do obsługi rozmów, gdzie każdego pomieszczenia mogą być reprezentowane jako grupa. Połączenia, które mogą być dodawane do lub usunięte z grup za pośrednictwem `AddToGroupAsync` i `RemoveFromGroupAsync` metody.

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

Członkostwo w grupie nie są zachowywane podczas ponownego nawiązania połączenia. Musi ponownie dołączyć do grupy po ponownym nawiązaniu połączenia. Nie jest możliwe do zliczenia członkowie danej grupy, ponieważ te informacje nie są dostępne, jeśli aplikacja będzie skalowana na wielu serwerach.

Aby chronić dostęp do zasobów podczas korzystania z grup, użyj [uwierzytelnianie i autoryzacja](xref:signalr/authn-and-authz) funkcji w programie ASP.NET Core. Tylko dodanie użytkowników do grupy, jeśli poświadczenia są prawidłowe dla tej grupy, komunikaty wysyłane do tej grupy zaczną się tylko dla autoryzowanych użytkowników. Jednak grupy nie są funkcją zabezpieczeń. Uwierzytelniania oświadczeń ma funkcje, których nie grupy, na przykład wygaśnięcia i odwoływania praw dostępu. Uprawnienia użytkownika, aby uzyskać dostęp do grupy jest odwołany, trzeba ręcznie wykryje to i usunąć je z niej.

> [!NOTE]
> Nazwy grup jest rozróżniana wielkość liter.

## <a name="related-resources"></a>Powiązane zasoby

* [Wprowadzenie](xref:tutorials/signalr)
* [Centra](xref:signalr/hubs)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)

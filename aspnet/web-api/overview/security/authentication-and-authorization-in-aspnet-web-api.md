---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Uwierzytelnianie i autoryzacja w interfejsie API sieci Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Zawiera ogólne omówienie uwierzytelniania i autoryzacji w interfejsie API sieci Web ASP.NET.
ms.author: riande
ms.date: 11/27/2012
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 368d2b9456d12b2bb4063a23333e5c8837faa3b8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598645"
---
# <a name="authentication-and-authorization-in-aspnet-web-api"></a>Uwierzytelnianie i autoryzacja w składniku Web API platformy ASP.NET

według [Jan Wasson](https://github.com/MikeWasson)

Utworzono internetowy interfejs API, ale teraz chcesz kontrolować dostęp do niego. W tej serii artykułów zawarto kilka opcji zabezpieczania internetowego interfejsu API przed nieautoryzowanymi użytkownikami. Ta seria obejmuje zarówno uwierzytelnianie, jak i autoryzację.

- *Uwierzytelnianie* polega na Poznaniu tożsamości użytkownika. Na przykład Alicja loguje się przy użyciu nazwy użytkownika i hasła, a serwer używa hasła do uwierzytelniania Alicja.
- *Autoryzacja* decyduje o tym, czy użytkownik może wykonać akcję. Na przykład Alicja ma uprawnienia do pobierania zasobu, ale nie tworzenia zasobu.

Pierwszy artykuł z serii zawiera ogólne omówienie uwierzytelniania i autoryzacji w usłudze ASP.NET Web API. Inne tematy opisują typowe scenariusze uwierzytelniania dla internetowego interfejsu API.

> [!NOTE]
> Poinformuj osoby, które przejrzały tę serię i dostarczyły cenną opinię: Rick Anderson, opłaty Broderick, Marcin Dorrans, Tomasz Dykstra, Hongmei GE, David Matson, Daniel Roth, Tim Teebken.

## <a name="authentication"></a>Uwierzytelnianie

Interfejs API sieci Web zakłada, że uwierzytelnianie odbywa się na hoście. W przypadku hostingu w sieci Web Host jest IIS, który używa modułów HTTP do uwierzytelniania. Można skonfigurować projekt tak, aby korzystał z dowolnego modułu uwierzytelniania wbudowanego w program IIS lub ASP.NET, albo napisać własny moduł HTTP w celu przeprowadzenia niestandardowego uwierzytelniania.

Gdy host uwierzytelnia użytkownika, tworzy *podmiot*zabezpieczeń, który jest obiektem [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) , który reprezentuje kontekst zabezpieczenia, pod którym działa kod. Host dołącza podmiot zabezpieczeń do bieżącego wątku przez ustawienie **Thread. CurrentPrincipal**. Podmiot zabezpieczeń zawiera skojarzony obiekt **tożsamości** , który zawiera informacje o użytkowniku. Jeśli użytkownik jest uwierzytelniony, właściwość **Identity. isauthenticate** zwraca **wartość true**. W przypadku żądań anonimowych **IsAuthenticated** zwraca **wartość false**. Aby uzyskać więcej informacji na temat podmiotów zabezpieczeń, zobacz [zabezpieczenia oparte na rolach](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Procedury obsługi komunikatów HTTP na potrzeby uwierzytelniania

Zamiast korzystać z hosta do uwierzytelniania, można umieścić logikę uwierzytelniania w programie [obsługi komunikatów http](../advanced/http-message-handlers.md). W takim przypadku program obsługi komunikatów bada żądanie HTTP i ustawia podmiot zabezpieczeń.

Kiedy należy używać programów obsługi komunikatów do uwierzytelniania? Oto kilka kompromisów:

- Moduł HTTP widzi wszystkie żądania, które przechodzą przez potok ASP.NET. Obsługa komunikatów widzi tylko żądania, które są kierowane do internetowego interfejsu API.
- Można ustawić procedury obsługi komunikatów dla trasy, które umożliwiają zastosowanie schematu uwierzytelniania do określonej trasy.
- Moduły HTTP są specyficzne dla usług IIS. Procedury obsługi komunikatów to host-niezależny od, dzięki czemu mogą być używane zarówno w przypadku hostingu przez Internet, jak i samoobsługowego udostępniania.
- Moduły HTTP uczestniczą w rejestrowaniu, inspekcji i tak dalej.
- Moduły HTTP działają wcześniej w potoku. Jeśli obsługujesz uwierzytelnianie w programie obsługi komunikatów, podmiot zabezpieczeń nie zostanie ustawiony do momentu uruchomienia programu obsługi. Ponadto podmiot zabezpieczeń powraca do poprzedniego podmiotu zabezpieczeń, gdy odpowiedź opuści procedurę obsługi komunikatów.

Ogólnie rzecz biorąc, jeśli nie musisz obsługiwać samodzielnego hostingu, moduł HTTP jest lepszym rozwiązaniem. Jeśli musisz obsługiwać samohosting, weź pod uwagę procedurę obsługi komunikatów.

### <a name="setting-the-principal"></a>Ustawianie podmiotu zabezpieczeń

Jeśli aplikacja wykonuje dowolną niestandardową logikę uwierzytelniania, należy ustawić podmiot zabezpieczeń w dwóch miejscach:

- **Thread. CurrentPrincipal**. Ta właściwość jest standardowym sposobem ustawiania podmiotu zabezpieczeń wątku w programie .NET.
- **HttpContext. Current. User**. Ta właściwość jest specyficzna dla ASP.NET.

Poniższy kod przedstawia sposób ustawienia podmiotu zabezpieczeń:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

W przypadku hostingu w sieci Web należy ustawić podmiot zabezpieczeń w obu miejscach: w przeciwnym razie kontekst zabezpieczeń może stać się niespójny. Jednak w przypadku samoobsługowego, **Właściwość HttpContext. Current** ma wartość null. Aby upewnić się, że Twój kod jest host-niezależny od, w związku z tym przed przypisaniem do obiektu **HttpContext. Current**Sprawdź wartość null.

## <a name="authorization"></a>Autoryzacja

Autoryzacja odbywa się później w potoku, bliżej kontrolera. Umożliwia wprowadzenie bardziej szczegółowych informacji w przypadku udzielenia dostępu do zasobów.

- *Filtry autoryzacji* są uruchamiane przed akcją kontrolera. Jeśli żądanie nie jest autoryzowane, filtr zwróci odpowiedź na błąd i akcja nie zostanie wywołana.
- W ramach akcji kontrolera można uzyskać aktualnego podmiotu zabezpieczeń z właściwości **ApiController. User** . Na przykład można filtrować listę zasobów na podstawie nazwy użytkownika, zwracając tylko te zasoby, które należą do danego użytkownika.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Korzystanie z atrybutu [autoryzuje]

Interfejs API sieci Web udostępnia wbudowany filtr autoryzacji, [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Ten filtr sprawdza, czy użytkownik jest uwierzytelniony. W przeciwnym razie zwraca kod stanu HTTP 401 (nieautoryzowany) bez wywoływania akcji.

Filtr można zastosować globalnie, na poziomie kontrolera lub na poziomie poszczególnych akcji.

**Globalnie**: aby ograniczyć dostęp dla każdego kontrolera internetowego interfejsu API, Dodaj filtr **AuthorizeAttribute** do listy filtrów globalnych:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Kontroler**: aby ograniczyć dostęp dla określonego kontrolera, Dodaj filtr jako atrybut do kontrolera:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Akcja**: aby ograniczyć dostęp do określonych akcji, Dodaj atrybut do metody akcji:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Alternatywnie można ograniczyć kontroler, a następnie zezwolić na dostęp anonimowy do określonych akcji przy użyciu atrybutu `[AllowAnonymous]`. W poniższym przykładzie dostęp dla metody `Post` jest ograniczony, ale metoda `Get` zezwala na dostęp anonimowy.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

W poprzednich przykładach filtr zezwala każdemu uwierzytelnionemu użytkownikowi na dostęp do ograniczonych metod; tylko użytkownicy anonimowi są przechowywani. Możesz również ograniczyć dostęp do konkretnych użytkowników lub użytkowników w określonych rolach:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> Filtr **AuthorizeAttribute** dla kontrolerów interfejsu API sieci Web znajduje się w przestrzeni nazw **System. Web. http** . W przestrzeni nazw **System. Web. MVC** istnieje podobny filtr dotyczący kontrolerów MVC, który nie jest zgodny z KONTROLERAMI interfejsu API sieci Web.

### <a name="custom-authorization-filters"></a>Niestandardowe filtry autoryzacji

Aby napisać niestandardowy filtr autoryzacji, należy utworzyć jeden z następujących typów:

- **AuthorizeAttribute**. Rozszerzając tę klasę, aby wykonać logikę autoryzacji na podstawie bieżącego użytkownika i ról użytkownika.
- **AuthorizationFilterAttribute**. Rozszerzając tę klasę, aby wykonać synchroniczną logikę autoryzacji, która nie musi być oparta na bieżącym użytkowniku lub roli.
- **IAuthorizationFilter**. Zaimplementuj ten interfejs, aby wykonać asynchroniczną logikę autoryzacji; na przykład, jeśli logika autoryzacji wykonuje asynchroniczne operacje we/wy lub połączenia sieciowe. (Jeśli logika autoryzacji jest powiązana z PROCESORem, łatwiej jest utworzyć ją z **AuthorizationFilterAttribute**, ponieważ nie trzeba pisać metody asynchronicznej).

Na poniższym diagramie przedstawiono hierarchię klas klasy **AuthorizeAttribute** .

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autoryzacja wewnątrz akcji kontrolera

W niektórych przypadkach można zezwolić na dalsze żądanie, ale zmienić zachowanie na podstawie podmiotu zabezpieczeń. Na przykład zwracane informacje mogą ulec zmianie w zależności od roli użytkownika. W ramach metody kontrolera można uzyskać bieżący podmiot zabezpieczeń z właściwości **ApiController. User** .

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]

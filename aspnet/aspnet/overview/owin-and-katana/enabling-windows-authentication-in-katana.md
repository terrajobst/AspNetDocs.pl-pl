---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Włączanie uwierzytelniania systemu Windows w Katana | Microsoft Docs
author: MikeWasson
description: 'W tym artykule pokazano, jak włączyć uwierzytelnianie systemu Windows w Katana. Obejmuje dwa scenariusze: Używanie usług IIS do hostowania Katana i używanie odbiornika HttpListener do samoobsługowego kat —...'
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 3d81e7e1bf13ab63417378fba0c5ab80213f404b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617181"
---
# <a name="enabling-windows-authentication-in-katana"></a>Włączanie uwierzytelniania systemu Windows w projekcie Katana

według [Jan Wasson](https://github.com/MikeWasson)

> W tym artykule pokazano, jak włączyć uwierzytelnianie systemu Windows w Katana. Obejmuje dwa scenariusze: użycie usług IIS do hostowania Katana i użycie odbiornika HttpListener do samodzielnego hostowania Katana w procesie niestandardowym. Dziękujemy za Dorrans, David Matson i Krzysztof Ross w celu przejrzenia tego artykułu.

Katana jest implementacją firmy Microsoft [Owin](http://owin.org/), otwartym interfejsem sieci Web dla platformy .NET. Wprowadzenie do OWIN i Katana można przeczytać [tutaj](an-overview-of-project-katana.md). Architektura OWIN ma kilka warstw:

- Host: zarządza procesem, w którym działa potok OWIN.
- Serwer: otwiera gniazdo sieciowe i nasłuchuje żądań.
- Oprogramowanie pośredniczące: przetwarza żądanie HTTP i odpowiedź.

Katana obecnie udostępnia dwa serwery, z których oba obsługują zintegrowane uwierzytelnianie systemu Windows:

- **Microsoft. Owin. host. SystemWeb**. Używa usług IIS z potokiem ASP.NET.
- **Microsoft. Owin. host. odbiornika HttpListener**. Używa [systemu .NET. odbiornika HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Ten serwer jest obecnie opcją domyślną w przypadku samoobsługowego Katana.

> [!NOTE]
> Usługa Katana obecnie nie udostępnia oprogramowania pośredniczącego OWIN do uwierzytelniania systemu Windows, ponieważ ta funkcja jest już dostępna na serwerach.

## <a name="windows-authentication-in-iis"></a>Uwierzytelnianie systemu Windows w usługach IIS

Korzystając z Microsoft. Owin. host. SystemWeb, można po prostu włączyć uwierzytelnianie systemu Windows w usługach IIS.

Zacznijmy od utworzenia nowej aplikacji ASP.NET przy użyciu szablonu projektu "ASP.NET Empty Web Application".

![](enabling-windows-authentication-in-katana/_static/image1.png)

Następnie Dodaj pakiety NuGet. W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

Teraz Dodaj klasę o nazwie `Startup` przy użyciu następującego kodu:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

To wszystko, co należy zrobić, aby utworzyć aplikację "Hello World" dla OWIN, działającą w usługach IIS. Naciśnij klawisz F5, aby debugować aplikację. Powinna zostać wyświetlona wartość "Hello world!" w oknie przeglądarki.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Następnie włączysz uwierzytelnianie systemu Windows w IIS Express. Z menu **Widok** wybierz pozycję **Właściwości**. Kliknij nazwę projektu w Eksplorator rozwiązań, aby wyświetlić właściwości projektu.

W oknie **Właściwości** ustaw opcję **uwierzytelnianie anonimowe** na **wyłączone** i ustaw opcję **uwierzytelnianie systemu Windows** na **włączone**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Po uruchomieniu aplikacji z programu Visual Studio, IIS Express będą wymagały poświadczeń systemu Windows użytkownika. Można to sprawdzić za pomocą [programu Fiddler](http://fiddler2.com/home) lub innego narzędzia debugowania http. Oto przykładowa odpowiedź HTTP:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Nagłówki WWW-Authenticate w tej odpowiedzi wskazują, że serwer obsługuje protokół [negocjowania](http://www.ietf.org/rfc/rfc4559.txt) , który korzysta z protokołu Kerberos lub NTLM.

Później podczas wdrażania aplikacji na serwerze wykonaj [następujące kroki](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) , aby włączyć uwierzytelnianie systemu Windows w usługach IIS na tym serwerze.

## <a name="windows-authentication-in-httplistener"></a>Uwierzytelnianie systemu Windows w odbiornika HttpListener

Jeśli używasz programu Microsoft. Owin. host. odbiornika HttpListener do samoobsługowego Katana, możesz włączyć uwierzytelnianie systemu Windows bezpośrednio w wystąpieniu **odbiornika HttpListener** .

Najpierw utwórz nową aplikację konsolową. Następnie Dodaj pakiety NuGet. W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie wybierz pozycję **konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

Teraz Dodaj klasę o nazwie `Startup` przy użyciu następującego kodu:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Ta klasa implementuje ten sam przykład "Hello World" z wcześniej, ale ustawia również uwierzytelnianie systemu Windows jako schemat uwierzytelniania.

W funkcji `Main` Uruchom potok OWIN:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

Można wysłać żądanie w programu Fiddler, aby potwierdzić, że aplikacja korzysta z uwierzytelniania systemu Windows:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Tematy pokrewne

[Omówienie projektu Katana](an-overview-of-project-katana.md)

[System .NET. odbiornika HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Informacje o uwierzytelnianiu formularzy OWIN w MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)

---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Praca z protokołem SSL w interfejsie API sieci Web | Microsoft Docs
author: MikeWasson
description: Pokazuje, jak używać protokołu SSL z interfejsem API sieci Web ASP.NET, w tym przy użyciu certyfikatów klienta SSL.
ms.author: riande
ms.date: 02/22/2019
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 31589b3713b1f1a9b98d12906bfef81f8bf5e3f9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598638"
---
# <a name="working-with-ssl-in-web-api"></a>Praca z protokołem SSL w składniku Web API

według [Jan Wasson](https://github.com/MikeWasson)

Kilka typowych schematów uwierzytelniania nie jest zabezpieczonych za pośrednictwem zwykłego protokołu HTTP. W szczególności uwierzytelnianie podstawowe i uwierzytelnianie formularzy wysyłają nieszyfrowane poświadczenia. Aby zapewnić bezpieczeństwo, te schematy uwierzytelniania *muszą* używać protokołu SSL. Ponadto certyfikaty klienta SSL mogą służyć do uwierzytelniania klientów.

## <a name="enabling-ssl-on-the-server"></a>Włączanie protokołu SSL na serwerze

Aby skonfigurować protokół SSL w usługach IIS 7 lub nowszym:

- Utwórz lub Pobierz certyfikat. W celu przetestowania można utworzyć certyfikat z podpisem własnym.
- Dodaj powiązanie HTTPS.

Aby uzyskać szczegółowe informacje, zobacz [jak skonfigurować protokół SSL w usługach IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

W przypadku testowania lokalnego można włączyć protokół SSL w IIS Express z programu Visual Studio. W okno Właściwości należy ustawić **wartość true**dla **protokołu SSL** . Zwróć uwagę na wartość **adresu URL protokołu SSL**; Użyj tego adresu URL do testowania połączeń HTTPS.

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Wymuszanie protokołu SSL w kontrolerze internetowego interfejsu API

Jeśli masz powiązanie HTTPS i HTTP, klienci mogą nadal korzystać z protokołu HTTP w celu uzyskania dostępu do lokacji. Niektóre zasoby mogą być dostępne za pośrednictwem protokołu HTTP, podczas gdy inne zasoby wymagają protokołu SSL. W takim przypadku należy użyć filtru akcji, aby wymagać protokołu SSL dla chronionych zasobów. Poniższy kod przedstawia filtr uwierzytelniania interfejsu API sieci Web, który sprawdza, czy protokół SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

Dodaj ten filtr do wszystkich akcji internetowego interfejsu API, które wymagają protokołu SSL:

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>Certyfikaty klienta SSL

Protokół SSL zapewnia uwierzytelnianie przy użyciu certyfikatów infrastruktury kluczy publicznych. Serwer musi dostarczyć certyfikat uwierzytelniający serwer dla klienta. Klient nie może dostarczyć certyfikatu do serwera, ale jest to jedna z opcji uwierzytelniania klientów. Aby można było używać certyfikatów klienta z protokołem SSL, musisz mieć możliwość dystrybucji podpisanych certyfikatów do użytkowników. W przypadku wielu typów aplikacji nie jest to dobre środowisko użytkownika, ale w niektórych środowiskach (na przykład Enterprise) może być możliwe.

| Zalety | Wady |
| --- | --- |
| -Poświadczenia certyfikatu są takie same, jak nazwa użytkownika i hasło. — Protokół SSL zapewnia kompletny bezpieczny kanał, z uwierzytelnianiem, integralnością komunikatów i szyfrowaniem komunikatów. | — Należy uzyskać certyfikaty PKI i zarządzać nimi. -Platforma kliencka musi obsługiwać certyfikaty klienta SSL. |

Aby skonfigurować usługi IIS do akceptowania certyfikatów klienta, Otwórz Menedżera usług IIS i wykonaj następujące czynności:

1. Kliknij węzeł lokacja w widoku drzewa.
2. Kliknij dwukrotnie funkcję **Ustawienia protokołu SSL** w środkowym okienku.
3. W obszarze **Certyfikaty klienta**wybierz jedną z następujących opcji: 

    - **Akceptuj**: program IIS zaakceptuje certyfikat od klienta, ale nie wymaga takiego certyfikatu.
    - **Wymagaj**: Wymagaj certyfikatu klienta. (Aby włączyć tę opcję, należy również wybrać opcję "Wymagaj protokołu SSL").

Możesz również ustawić te opcje w pliku ApplicationHost. config:

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

Flaga **SslNegotiateCert** oznacza, że usługi IIS będą akceptować certyfikat od klienta, ale nie wymagają one (odpowiednik opcji "Akceptuj" w Menedżerze usług IIS). Aby wymagać certyfikatu, Ustaw flagę **SslRequireCert** . W celu przetestowania można również ustawić te opcje w IIS Express w lokalnym hoście aplikacji. Plik konfiguracji znajdujący się w lokalizacji "Documents\IISExpress\config".

### <a name="creating-a-client-certificate-for-testing"></a>Tworzenie certyfikatu klienta do testowania

Do celów testowych można użyć programu [Makecert. exe](/windows/desktop/SecCrypto/makecert) , aby utworzyć certyfikat klienta. Najpierw utwórz testowy główny urząd certyfikacji:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

MakeCert wyświetli monit o wprowadzenie hasła dla klucza prywatnego.

Następnie Dodaj certyfikat do magazynu "Zaufane główne urzędy certyfikacji" serwera testowego w następujący sposób:

1. Otwórz program MMC.
2. W obszarze **plik**wybierz pozycję **Dodaj/Usuń przystawkę**.
3. Wybierz pozycję **konto komputera**.
4. Wybierz pozycję **komputer lokalny** i Ukończ pracę kreatora.
5. W okienku nawigacji rozwiń węzeł "Zaufane główne urzędy certyfikacji".
6. W menu **Akcja** wskaż polecenie **wszystkie zadania**, a następnie kliknij przycisk **Importuj** , aby uruchomić Kreatora importu certyfikatów.
7. Przejdź do pliku certyfikatu, TempCA. cer.
8. Kliknij przycisk **Otwórz**, a następnie kliknij przycisk **dalej** i Ukończ pracę kreatora. (Zostanie wyświetlony monit o ponowne wprowadzenie hasła).

Teraz Utwórz certyfikat klienta podpisany przy użyciu pierwszego certyfikatu:

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Używanie certyfikatów klienta w internetowym interfejsie API

Po stronie serwera można uzyskać certyfikat klienta, wywołując [GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx) w komunikacie żądania. Metoda zwraca wartość null, jeśli nie ma certyfikatu klienta. W przeciwnym razie zwraca wystąpienie **X509Certificate2** . Użyj tego obiektu, aby uzyskać informacje z certyfikatu, takie jak wystawca i podmiot. Następnie możesz użyć tych informacji do uwierzytelniania i/lub autoryzacji.

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]

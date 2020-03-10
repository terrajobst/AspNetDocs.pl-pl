---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: Uwierzytelnianie użytkowników przy użyciu uwierzytelniania systemu Windows (VB) | Microsoft Docs
author: microsoft
description: Dowiedz się, jak używać uwierzytelniania systemu Windows w kontekście aplikacji MVC. Dowiesz się, jak włączyć uwierzytelnianie systemu Windows w sieci Web współdziałanie z aplikacją...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: aa64b1f9ef6461a81611ca066310dca2d545baa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624118"
---
# <a name="authenticating-users-with-windows-authentication-vb"></a>Uwierzytelnianie użytkowników za pomocą uwierzytelniania systemu Windows (VB)

przez [firmę Microsoft](https://github.com/microsoft)

> Dowiedz się, jak używać uwierzytelniania systemu Windows w kontekście aplikacji MVC. Dowiesz się, jak włączyć uwierzytelnianie systemu Windows w pliku konfiguracji sieci Web aplikacji i jak skonfigurować uwierzytelnianie za pomocą usług IIS. Na koniec dowiesz się, jak używać atrybutu [autoryzuje], aby ograniczyć dostęp do akcji kontrolera do określonych użytkowników lub grup systemu Windows.

Celem tego samouczka jest wyjaśnienie, w jaki sposób można korzystać z funkcji zabezpieczeń wbudowanych w Internet Information Services do ochrony danych w widokach w aplikacjach MVC. Dowiesz się, jak zezwolić na wywoływanie akcji kontrolera tylko określonym użytkownikom systemu Windows lub użytkownikom należącym do określonych grup systemu Windows.

Korzystanie z uwierzytelniania systemu Windows jest sensowne podczas tworzenia wewnętrznej witryny internetowej firmy (witryny intranetowej) i chcesz, aby użytkownicy mogli używać standardowych nazw użytkowników i haseł systemu Windows podczas uzyskiwania dostępu do witryny sieci Web. Jeśli tworzysz stronę sieci Web na zewnątrz (internetową witrynę sieci Web), zamiast tego Rozważ użycie uwierzytelniania formularzy.

#### <a name="enabling-windows-authentication"></a>Włączanie uwierzytelniania systemu Windows

Podczas tworzenia nowej aplikacji ASP.NET MVC uwierzytelnianie systemu Windows nie jest domyślnie włączone. Uwierzytelnianie formularzy jest domyślnym typem uwierzytelniania włączonych dla aplikacji MVC. Należy włączyć uwierzytelnianie systemu Windows, modyfikując plik konfiguracji sieci Web aplikacji MVC (Web. config). Znajdź sekcję &lt;Authentication&gt; i zmodyfikuj ją tak, aby korzystała z systemu Windows zamiast uwierzytelniania formularzy w następujący sposób:

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

Po włączeniu uwierzytelniania systemu Windows serwer sieci Web będzie odpowiedzialny za uwierzytelnianie użytkowników. Zazwyczaj istnieją dwa różne typy serwerów sieci Web, które są używane podczas tworzenia i wdrażania aplikacji ASP.NET MVC.

Najpierw podczas tworzenia aplikacji MVC należy użyć serwera sieci Web ASP.NET Development dołączonego do programu Visual Studio. Domyślnie serwer ASP.NET Development Web wykonuje wszystkie strony w kontekście bieżącego konta systemu Windows (dowolnego konta użytego do zalogowania się do systemu Windows).

Serwer sieci Web ASP.NET Development obsługuje również uwierzytelnianie NTLM. Uwierzytelnianie NTLM można włączyć, klikając prawym przyciskiem myszy nazwę projektu w oknie Eksplorator rozwiązań i wybierając pozycję Właściwości. Następnie wybierz kartę Sieć Web i zaznacz pole wyboru NTLM (patrz rysunek 1).

**Rysunek 1 — Włączanie uwierzytelniania NTLM dla serwera ASP.NET Development Web**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

W przypadku produkcyjnej aplikacji sieci Web program IIS jest używany jako serwer sieci Web. Usługi IIS obsługują kilka typów uwierzytelniania, w tym:

- Uwierzytelnianie podstawowe — zdefiniowane jako część protokołu HTTP 1,0. Wysyła nazwy użytkowników i hasła w postaci zwykłego tekstu (kodowane algorytmem Base64) przez Internet. — Uwierzytelnianie szyfrowane — wysyła skrót hasła zamiast samego hasła przez Internet. -Zintegrowane uwierzytelnianie systemu Windows (NTLM) — najlepszy typ uwierzytelniania do użycia w środowiskach intranetowych przy użyciu systemu Windows. -Uwierzytelnianie certyfikatu — umożliwia uwierzytelnianie przy użyciu certyfikatu po stronie klienta. Certyfikat jest mapowany na konto użytkownika systemu Windows.

> [!NOTE] 
> 
> Aby zapoznać się z bardziej szczegółowym omówieniem tych różnych typów uwierzytelniania, zobacz [https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).

Aby włączyć określony typ uwierzytelniania, można użyć Menedżera Internet Information Services. Należy pamiętać, że wszystkie typy uwierzytelniania nie są dostępne w przypadku każdego systemu operacyjnego. Ponadto, jeśli używasz usług IIS 7,0 z systemem Windows Vista, musisz włączyć różne typy uwierzytelniania systemu Windows, zanim zostaną one wyświetlone w Menedżerze Internet Information Services. Otwórz **Panel sterowania, programy, programy i funkcje, Włącz lub wyłącz funkcje systemu Windows**, a następnie rozwiń węzeł Internet Information Services (patrz rysunek 2).

**Rysunek 2 — Włączanie funkcji usług IIS systemu Windows**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

Za pomocą Internet Information Services można włączać lub wyłączać różne typy uwierzytelniania. Na przykład rysunek 3 ilustruje wyłączenie uwierzytelniania anonimowego i włączenie zintegrowanego uwierzytelniania systemu Windows (NTLM) w przypadku korzystania z usług IIS 7,0.

**Rysunek 3 — Włączanie zintegrowanego uwierzytelniania systemu Windows**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autoryzowanie użytkowników i grup systemu Windows

Po włączeniu uwierzytelniania systemu Windows można użyć atrybutu &lt;Autoryzuj&gt;, aby kontrolować dostęp do kontrolerów lub akcji kontrolera. Ten atrybut może być stosowany do całego kontrolera MVC lub konkretnej akcji kontrolera.

Na przykład kontroler Home na liście 1 ujawnia trzy akcje o nazwach index (), CompanySecrets () i StephenSecrets (). Każdy może wywoływać akcję index (). Jednak tylko członkowie lokalnej grupy menedżerów systemu Windows mogą wywoływać akcję CompanySecrets (). Na koniec tylko użytkownik domeny systemu Windows o nazwie Stephen (w domenie Redmond) może wywołać akcję StephenSecrets ().

**Lista 1 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> Ze względu na kontrolę konta użytkownika systemu Windows (UAC) podczas pracy z systemem Windows Vista lub Windows Server 2008 lokalna Grupa Administratorzy będzie zachowywać się inaczej niż w przypadku innych grup. Atrybut &lt;Autoryzuj&gt; nie rozpoznaje prawidłowo elementu członkowskiego lokalnej grupy administratorów, chyba że zmienisz ustawienia kontroli konta użytkownika komputera.

Dokładnie to, co się stanie w przypadku próby wywołania akcji kontrolera bez uzyskania odpowiednich uprawnień, zależy od typu włączonego uwierzytelniania. Domyślnie w przypadku korzystania z serwera ASP.NET Development wystarczy uzyskać pustą stronę. Strona jest obsługiwana z **nieautoryzowanym** stanem odpowiedzi HTTP 401.

Jeśli z drugiej strony korzystasz z usług IIS z wyłączonym uwierzytelnianiem anonimowym i włączona jest funkcja uwierzytelnianie podstawowe, podczas każdego żądania chronionej strony będziesz otrzymywać okna dialogowe logowania.

**Rysunek 4 — okno dialogowe logowania do uwierzytelniania podstawowego**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>Podsumowanie

W tym samouczku wyjaśniono, jak można użyć uwierzytelniania systemu Windows w kontekście aplikacji ASP.NET MVC. Wiesz już, jak włączyć uwierzytelnianie systemu Windows w pliku konfiguracji sieci Web aplikacji i jak skonfigurować uwierzytelnianie za pomocą usług IIS. Na koniec wiesz już, jak używać atrybutu &lt;Autoryzuj&gt;, aby ograniczyć dostęp do akcji kontrolera do określonych użytkowników lub grup systemu Windows.

> [!div class="step-by-step"]
> [Poprzednie](authenticating-users-with-forms-authentication-vb.md)
> [dalej](preventing-javascript-injection-attacks-vb.md)

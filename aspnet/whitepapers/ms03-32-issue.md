---
uid: whitepapers/ms03-32-issue
title: Naprawa błędu "niedostępność aplikacji serwera" po zastosowaniu aktualizacji zabezpieczeń dla programu IE | Microsoft Docs
author: rick-anderson
description: W tym dokumencie opisano poprawkę, która rozwiązuje problem z aktualizacją zabezpieczeń MS03-32 dla programu Internet Explorer, która ma wpływ na aplikacje ASP.NET 1,0 działające w sieci Wi...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: e0b6776cbfe22e341ac7105f03daac5074b480fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574187"
---
# <a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Poprawka rozwiązująca problem powodujący błąd „Aplikacja serwera niedostępna” po zastosowaniu aktualizacji zabezpieczeń dla programu Internet Explorer

> W tym dokumencie opisano poprawkę, która rozwiązuje problem z aktualizacją zabezpieczeń MS03-32 dla programu Internet Explorer, która ma wpływ na aplikacje ASP.NET 1,0 działające w systemie Windows XP Professional.
> 
> Dotyczy systemów ASP.NET 1,0 i Windows XP Professional.

Firma Microsoft zidentyfikowała problem z aktualizacją zabezpieczeń MS03-32 dotyczącą poprawek zabezpieczeń programu Internet Explorer i ASP.NET 1,0 działającą w systemie Windows XP. Tę poprawkę można zainstalować ręcznie lub przez uzyskanie najnowszych aktualizacji krytycznych z witryny Windows Update.

Symptomem tego problemu jest to, że po zainstalowaniu poprawki na komputerze z systemem Windows XP wszystkie żądania do ASP.NET aplikacji uruchomionych na lokalnym serwerze sieci Web usług IIS 5,1 powodują pojawienie się komunikatu o błędzie mówiącego "aplikacja serwera niedostępna". Nie dotyczy to żądań do zdalnych serwerów sieci Web.

Ten problem dotyczy tylko instalacji z systemem ASP.NET 1,0 w systemie Windows XP. Nie ma to wpływu na maszyny z systemem Windows 2000 lub Windows Server 2003. Nie ma to również wpływu na maszyny z systemem Windows XP i ASP.NET 1,1.

Należy pamiętać, że ten problem **nie jest** usterką zabezpieczeń z ASP.NET. Nie **otwiera ani** nie zezwala na złośliwe ataki dla aplikacji lub serwera ASP.NET. Zamiast tego jest czysto funkcjonalną usterką powodowaną przez poprawkę.

Pracujemy na stałym rozwiązaniu tego problemu. W międzyczasie można wykonać następujący plik wsadowy jako obejście problemu. Plik wsadowy wykonuje następujące czynności:

1. Powoduje zatrzymanie usług IIS i usługi stanu ASP.NET
2. Usuwa i ponownie tworzy konto ASPNET przy użyciu znanego hasła tymczasowego
3. Używa polecenia `runas` systemu Windows do uruchomienia pliku wykonywalnego, który tworzy profil użytkownika ASPNET
4. Re-registers ASP.NET. Spowoduje to utworzenie nowego hasła losowego dla konta i zastosowanie domyślnych ustawień kontroli dostępu ASP.NET
5. Uruchamia ponownie usługę IIS

Plik wsadowy zawiera stałe hasło tymczasowe "<strong>1pass\@Word</strong>", którego monit o wprowadzenie do polecenia runas zostanie wyświetlony po uruchomieniu pliku wsadowego. Po zakończeniu wykonywania polecenia runas hasło konta ASPNET zostanie odtworzone przy użyciu silnej losowej wartości. Należy pamiętać, że plik wsadowy może się nie powieść, jeśli hasło stałe nie spełnia wymagań dotyczących złożoności haseł w danym środowisku. W takim przypadku można zmienić na inną wartość, która jest odpowiednia dla danego środowiska.

*> [!IMPORTANT]* Jeśli dodano niestandardowe ustawienia kontroli dostępu lub uprawnienia konta bazy danych dla konta ASPNET, należy je ponownie utworzyć po zakończeniu tego pliku wsadowego. Jest to spowodowane tym, że po odtworzeniu konta zostanie wyświetlony nowy identyfikator zabezpieczeń (SID).

*> [!IMPORTANT]* Jeśli uruchamiasz proces roboczy ASP.NET przy użyciu konta niestandardowego innego niż konto ASPNET, nie należy uruchamiać tego pliku wsadowego. Zamiast tego należy zalogować się interaktywnie lub użyć polecenia runas z tym kontem, które spowoduje utworzenie profilu użytkownika dla tego konta.

Plik wsadowy znajduje się w archiwum samowyodrębniającym się poniżej. Aby go użyć:

1. Musisz działać jako konto z uprawnieniami administratora
2. [Pobierz i Otwórz samowyodrębniający się plik wykonywalny](ms03-32-issue/_static/fixup1.exe)
3. Wyodrębnij zawartość do c:\
4. Wybierz pozycję Uruchom... z menu Start i wprowadź `cmd.exe`
5. W oknach polecenia Otwórz wpisz `c:\fixup.cmd`.
6. Po wyświetleniu monitu wprowadź <strong>1pass\@Word</strong> jako hasło.
7. Jeśli wcześniej określono niestandardowe ustawienia kontroli dostępu lub uprawnienia konta bazy danych dla konta ASPNET, należy ponownie zastosować te ustawienia teraz.

Wiele przeprosinami dla niedogodności, które to spowodowało. Będziemy ogłaszać dodatkowe informacje, gdy staną się dostępne.

W poniższej macierzy przedstawiono szczegóły platform i wersji, których dotyczy problem.

| .NET Framework | Platforma | Narażone |
| --- | --- | --- |
| Wersja 1,0 | Windows 2000 Professional | Nie |
| Wersja 1,0 | Windows 2000 Server | Nie |
| Wersja 1,0 | Windows XP Professional | Tak |
| Wersja 1,0 | Windows Server 2003 | Nie |
| Wersja 1,0 | Windows XP Home z Cassini | Nie |
| Wersja 1,1 | Windows 2000 Professional | Nie |
| Wersja 1,1 | Windows 2000 Server | Nie |
| Wersja 1,1 | Windows XP Professional | Nie |
| Wersja 1,1 | Windows Server 2003 | Nie |
| Wersja 1,1 | Windows XP Home z Cassini | Nie |

Dziękujemy,   
 Zespół ASP.NET

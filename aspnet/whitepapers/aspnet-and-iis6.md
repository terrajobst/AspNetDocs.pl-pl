---
uid: whitepapers/aspnet-and-iis6
title: Uruchamianie ASP.NET 1,1 z usługami IIS 6,0 | Microsoft Docs
author: rick-anderson
description: Mimo że system Windows Server 2003 obejmuje zarówno IIS 6,0, jak i ASP.NET 1,1, te składniki są domyślnie wyłączone. W tym dokumencie opisano sposób włączania usług IIS 6,0 a...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 2e7812f34481afe9a71927c0d9ba2a9abc9632e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523311"
---
# <a name="running-aspnet-11-with-iis-60"></a>Uruchamianie platformy ASP.NET 1.1 z usługami IIS 6.0

> Mimo że system Windows Server 2003 obejmuje zarówno IIS 6,0, jak i ASP.NET 1,1, te składniki są domyślnie wyłączone. W tym dokumencie opisano sposób włączania usług IIS 6,0 i ASP.NET 1,1 oraz zalecane jest użycie kilku ustawień konfiguracyjnych w celu uzyskania optymalnej wydajności usług IIS i ASP.NET.
> 
> Dotyczy ASP.NET 1,1 i IIS 6,0.

ASP.NET 1,1 jest dostarczany z systemem Windows Server 2003, który obejmuje również najnowszą wersję programu Internet Information Server (IIS) w wersji 6,0. Usługi IIS 6,0 i ASP.NET 1,1 są przeznaczone do bezproblemowego integrowania i ASP.NET teraz domyślnie nowemu modelowi procesu roboczego usług IIS 6,0.

## <a name="aspnet-11-is-not-installed-by-default"></a>ASP.NET 1,1 nie jest instalowany domyślnie

W przeciwieństwie do poprzednich wersji systemów operacyjnych serwera firmy Microsoft program Internet Information Server (IIS) nie jest domyślnie włączony. nie jest ASP.NET 1,1. Dostępne są dwie opcje włączania usług IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Włączanie usług IIS, opcja #1 — Konfigurowanie serwera

System Windows Server 2003 dostarcza nowego Kreatora konfiguracji serwera, aby pomóc w prawidłowym skonfigurowaniu serwera w żądanym trybie.

Aby uruchomić kreatora, należy zalogować się jako administrator — przejdź do: Start | Programy | Narzędzia administracyjne i wybierz pozycję "Konfiguruj serwer".

Po wybraniu powinien zostać wyświetlony ekran otwieranie Kreatora konfiguracji serwera:

![](aspnet-and-iis6/_static/image1.jpg)

Kliknij przycisk "dalej &gt;":

![](aspnet-and-iis6/_static/image2.jpg)

Kliknij przycisk "dalej &gt;"

![](aspnet-and-iis6/_static/image3.jpg)

Na tym ekranie należy wybrać opcję "serwer aplikacji (IIS, ASP.NET)" jako opcje konfiguracji.

Kliknij przycisk "dalej &gt;".

![](aspnet-and-iis6/_static/image4.jpg)

Po wybraniu opcji skonfigurowania serwera jako serwera aplikacji zostanie wyświetlony monit z pytaniem o to, jakie dodatkowe funkcje należy zainstalować. Żadna opcja nie jest zaznaczona domyślnie. Aby włączyć ASP.NET automatycznie, należy wybrać opcję "Włącz ASP. SIEĆ.

Kliknij przycisk "dalej &gt;".

![](aspnet-and-iis6/_static/image5.jpg)

Na tym ekranie są wyświetlane opcje, które mają zostać zainstalowane.

Kliknij przycisk "dalej &gt;".

![](aspnet-and-iis6/_static/image6.jpg)

Zobaczysz ten ekran podczas instalowania wybranych opcji. Po zainstalowaniu usług są wyświetlane inne okna dialogowe. Może być również wyświetlany monit o lokalizację instalacyjnego dysku CD systemu Windows 2003 Server.

Po zakończeniu kliknij przycisk "dalej &gt;".

![](aspnet-and-iis6/_static/image7.jpg)

Kliknij przycisk "Zakończ" — system Windows Server 2003 jest teraz skonfigurowany do obsługi usług IIS 6,0 i ASP.NET 1,1.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>Włączanie usług IIS, opcja #2 — Ręczne konfigurowanie usług IIS i ASP.NET

Jeśli nie chcesz używać Kreatora konfiguracji serwera, możesz opcjonalnie zainstalować usługi IIS 6,0 i ASP.NET 1,1 przy użyciu apletu "Dodaj lub usuń programy" w panelu sterowania.

Najpierw otwórz Panel sterowania:

![](aspnet-and-iis6/_static/image8.jpg)

Następnie kliknij pozycję "Dodaj/Usuń składniki systemu Windows", co spowoduje otwarcie Kreatora składników systemu Windows:

![](aspnet-and-iis6/_static/image9.jpg)

Zaznacz i zaznacz pole wyboru Serwer aplikacji, a następnie kliknij przycisk Szczegóły? przycisk

![](aspnet-and-iis6/_static/image10.jpg)

Aby zainstalować ASP.NET, zaznacz opcję ASP. SIEĆ.

Kliknij przycisk OK, aby powrócić do Kreatora składników systemu Windows. Kliknij przycisk "dalej &gt;" w Kreatorze składników systemu Windows, aby rozpocząć instalowanie:

![](aspnet-and-iis6/_static/image11.jpg)

Po zainstalowaniu usług są wyświetlane inne okna dialogowe. Może być również wyświetlany monit o lokalizację instalacyjnego dysku CD systemu Windows 2003 Server.

Po zakończeniu instalacji zostanie wyświetlony ostatni ekran Kreatora składników systemu Windows:

![](aspnet-and-iis6/_static/image12.jpg)

Usługi IIS 6,0 i ASP.NET 1,1 są teraz skonfigurowane i dostępne.

## <a name="recommended-settings"></a>Zalecane ustawienia

Podczas pracy z programem ASP.NET 1,1 z usługami IIS 6,0 istnieje kilka ustawień konfiguracji, które są zalecane do uzyskania optymalnej wydajności z ASP.NET:

- Konfigurowanie limitów pamięci procesu roboczego
- Konfigurowanie odtwarzania procesów roboczych

### <a name="configuring-worker-process-memory-limits"></a>Konfigurowanie limitów pamięci procesu roboczego

Domyślnie usługi IIS 6,0 nie ustawiają limitu ilości pamięci, która może być używana przez usługi IIS. ASP.NET. Funkcja pamięci podręcznej sieci polega na ograniczeniu ilości pamięci, aby pamięć podręczna mogła aktywnie usunąć nieużywane elementy z pamięci.

Zaleca się skonfigurowanie funkcji odtwarzania pamięci w usługach IIS 6,0. Aby skonfigurować ten otwarty Internet Information Services Manager (Start | Programy | Narzędzia administracyjne | Internet Information Services). Po otwarciu rozwiń folder pule aplikacji:

Dla każdej puli aplikacji:

![](aspnet-and-iis6/_static/image13.jpg)

1. Kliknij prawym przyciskiem myszy pulę aplikacji, na przykład "domyślna wartość", a następnie wybierz pozycję "właściwości":

![](aspnet-and-iis6/_static/image14.jpg)

2. Następnie Włącz odtwarzanie pamięci, klikając pozycję "Maksymalna użyta pamięć (w megabajtach):". Wartość nie powinna być większa niż ilość pamięci fizycznej (nie wirtualnej) na serwerze, dlatego dobre przybliżenie wynosi 60% pamięci fizycznej, tj. dla serwera z 512 MB pamięci fizycznej wybierz 310. Zaleca się również, aby maksymalna wartość nie przekraczała 800MB przy użyciu przestrzeni adresowej 2 GB. Jeśli przestrzeń adresowa pamięci serwera jest WŁĄCZONĄ, maksymalny limit pamięci dla procesu roboczego może być równy 1, 800MB:

![](aspnet-and-iis6/_static/image15.jpg)

Kliknij przycisk "Zastosuj" i "OK", aby zamknąć okno dialogowe właściwości. Powtórz tę czynność dla wszystkich dostępnych pul aplikacji.

### <a name="configuring-worker-recycling"></a>Konfigurowanie odtwarzania procesów roboczych

Domyślnie usługi IIS 6,0 są skonfigurowane do odtwarzania procesu roboczego co 29 godzin. Jest to bardzo agresywne dla aplikacji z systemem ASP.NET i zaleca się, aby automatyczne odtwarzanie procesów roboczych było wyłączone.

Aby wyłączyć automatyczne odtwarzanie procesów roboczych, najpierw Otwórz Menedżera Internet Information Services (Start | Programy | Narzędzia administracyjne | Internet Information Services). Po otwarciu rozwiń folder pule aplikacji:

![](aspnet-and-iis6/_static/image16.jpg)

Dla każdej puli aplikacji:

1. Kliknij prawym przyciskiem myszy pulę aplikacji, na przykład "domyślna wartość", a następnie wybierz pozycję "właściwości":

![](aspnet-and-iis6/_static/image17.jpg)

2. Usuń zaznaczenie opcji "Odtwórz proces roboczy (w minutach):":

![](aspnet-and-iis6/_static/image18.jpg)

Kliknij przycisk "Zastosuj" i "OK", aby zamknąć okno dialogowe właściwości. Powtórz tę czynność dla wszystkich dostępnych pul aplikacji.

## <a name="granting-write-access-to-the-file-system"></a>Udzielanie dostępu do zapisu w systemie plików

Jeśli aplikacja wymaga dostępu do zapisu w systemie plików i używasz systemu plików NTFS, należy zmodyfikować listę Access Control (ACL) dla folderu lub pliku, aby przyznać ASP.NET dostęp do programu.

Na przykład, aby udzielić ASP.NET dostępu do zapisu do programu c:\Inetpub\Wwwroot, otwórz Eksploratora i przejdź do katalogu:

![](aspnet-and-iis6/_static/image19.jpg)

Następnie kliknij prawym przyciskiem myszy katalog, np. "wwwroot" i wybierz polecenie Właściwości. Po otwarciu okna dialogowego Właściwości wybierz kartę "zabezpieczenia":

![](aspnet-and-iis6/_static/image20.jpg)

Katalog c:\InetPub\Wwwroot\ jest specjalnym katalogiem, w którym Specjalna grupa usług IIS 6,0 "IIS\_WPG" ma już przyznane uprawnienia Odczyt &amp; wykonywanie, wyświetlanie zawartości folderu i odczyt. Aby jednak udzielić uprawnienia do zapisu, należy kliknąć pole wyboru Zezwalaj na zapis:

![](aspnet-and-iis6/_static/image21.jpg)

Usługi IIS 6,0 mają teraz uprawnienie do zapisu w tym folderze. Aby udzielić uprawnień do zapisu w innych folderach, wykonaj następujące kroki — Uwaga. może być konieczne dodanie grupy IIS\_WPG, jeśli jeszcze nie istnieje.

> [!CAUTION]
> Przyznanie uprawnienia do zapisu dla usług IIS\_WPG zezwoli na zapisanie w tym katalogu dowolnej aplikacji ASP.NET.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Obsługa zintegrowanego uwierzytelniania za pomocą SQL Server

Uwierzytelnianie zintegrowane umożliwia SQL Server do sprawdzania poprawności SQL Server kont logowania przy użyciu uwierzytelniania systemu Windows NT. Dzięki temu użytkownik może ominąć standardowy proces logowania SQL Server. Dzięki temu użytkownik sieci może uzyskać dostęp do bazy danych SQL Server bez dostarczania oddzielnej identyfikacji logowania lub hasła, ponieważ SQL Server pobiera informacje o użytkowniku i haśle z procesu zabezpieczeń sieci systemu Windows NT.

Wybór uwierzytelniania zintegrowanego dla aplikacji ASP.NET jest dobrym rozwiązaniem, ponieważ żadne poświadczenia nie są nigdy przechowywane w ciągu połączenia dla aplikacji. Zamiast tego parametry połączenia używane do nawiązania połączenia z serwerem SQL będą wyglądać następująco:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Te parametry połączenia mówią, SQL Server używać poświadczeń systemu Windows aplikacji próbującej uzyskać dostęp do SQL Server. W przypadku ASP.NET/IIS 6 będzie to konto w grupie usługi IIS\_WPG.

Aby włączyć zintegrowane uwierzytelnianie między SQL Server i ASP.NET, należy najpierw upewnić się, że SQL Server jest skonfigurowany do uwierzytelniania zintegrowanego lub uwierzytelniania w trybie mieszanym — Sprawdź swoją administratora usługi, aby to określić. Jeśli SQL Server jest w jednym z tych dwóch trybów, można użyć uwierzytelniania zintegrowanego.

Otwórz Menedżera SQL Server Enterprise (Start | Programy | Microsoft SQL Server | Enterprise Manager), wybierz odpowiedni serwer i rozwiń folder zabezpieczeń:

![](aspnet-and-iis6/_static/image22.jpg)

Jeśli grupa "BUILTINT\IIS\_WPG" nie znajduje się na liście, kliknij prawym przyciskiem myszy pozycję logowania i wybierz pozycję "nowy Login':

![](aspnet-and-iis6/_static/image23.jpg)

W polu tekstowym "Name:" wprowadź wartość "[nazwa serwera/domeny] \IIS\_WPG" lub kliknij przycisk wielokropka, aby otworzyć selektor użytkownika/grupy systemu Windows NT:

![](aspnet-and-iis6/_static/image24.jpg)

Wybierz grupę IIS\_WPG, a następnie kliknij przycisk "Dodaj" i OK, aby zamknąć selektor.

Następnie należy również ustawić domyślną bazę danych i uprawnienia dostępu do bazy danych. Aby ustawić domyślną bazę danych, wybierz pozycję z listy rozwijanej, na przykład, poniżej opcji Northwind:

![](aspnet-and-iis6/_static/image25.jpg)

Następnie kliknij kartę dostęp do bazy danych:

![](aspnet-and-iis6/_static/image26.jpg)

Kliknij pole wyboru Zezwalaj dla każdej bazy danych, do której chcesz zezwolić na dostęp. Należy również wybrać role bazy danych, sprawdzając, czy właściciel\_ma mieć pewność, że logowanie będzie miało wszystkie niezbędne uprawnienia do zarządzania wybraną bazą danych i korzystania z niej.

Kliknij przycisk OK, aby zamknąć okno dialogowe właściwości. Aplikacja ASP.NET jest teraz skonfigurowana do obsługi zintegrowanego uwierzytelniania SQL Server.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>Nie uruchamiaj ASP.NET 1,0 w trybie macierzystym usług IIS 6,0

ASP.NET 1,0 w usługach IIS 6,0 jest obsługiwana tylko w trybie zgodności usług IIS 5.

Aby skonfigurować ASP.NET 1,0 do uruchamiania w trybie zgodności usług IIS 5,0, Otwórz menedżer usług internetowych i kliknij prawym przyciskiem myszy witrynę sieci Web i wybierz polecenie Właściwości:

![](aspnet-and-iis6/_static/image27.jpg)

Przejdź do karty usługa i sprawdź, czy? Czy uruchomić usługę WWW w trybie izolacji usług IIS 5,0?:

![](aspnet-and-iis6/_static/image28.jpg)

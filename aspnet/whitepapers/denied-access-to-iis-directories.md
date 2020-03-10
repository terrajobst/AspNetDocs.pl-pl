---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET odmowa dostępu do katalogów usług IIS | Microsoft Docs
author: rick-anderson
description: W tym dokumencie opisano, co należy zrobić, jeśli żądanie do aplikacji ASP.NET zwróci błąd, "odmowa dostępu do katalogu DirectoryName. Nie powiodło się...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: a3a53aa88abbe1bcaaea7d691406800c8f9b988b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638503"
---
# <a name="aspnet-denied-access-to-iis-directories"></a>Platforma ASP.NET nie ma dostępu do katalogów usług IIS

> W tym dokumencie opisano, co należy zrobić, jeśli żądanie do aplikacji ASP.NET zwróci błąd, "odmowa dostępu do katalogu *DirectoryName* . Nie można rozpocząć monitorowania zmian w katalogu. "
> 
> Dotyczy ASP.NET 1,0 i ASP.NET 1,1.

ASP.NET v1 RTM jest teraz uruchamiany przy użyciu mniej uprzywilejowanego konta systemu Windows, które jest rejestrowane jako konto "ASPNET" na komputerze lokalnym.

W niektórych zablokowanych systemach to konto może nie mieć domyślnie uprawnień do odczytu do katalogów zawartości witryny sieci Web, katalogu głównego aplikacji lub katalogu głównego witryny internetowej. W takim przypadku podczas żądania stron z danej aplikacji sieci Web zostanie wyświetlony następujący błąd:

![](denied-access-to-iis-directories/_static/image1.jpg)

Aby rozwiązać ten problem, należy zmienić uprawnienia zabezpieczeń do odpowiednich katalogów.

W odniesieniu do ASP.NET wymaga dostępu do odczytu, wykonywania i wyświetlania listy dla konta ASPNET dla katalogu głównego witryny sieci Web (na przykład: c:\Inetpub\Wwwroot lub dowolnego alternatywnego katalogu witryn, który mógł zostać skonfigurowany w usługach IIS), katalogu zawartości i katalogu głównego aplikacji. w celu monitorowania zmian w pliku konfiguracji. Katalog główny aplikacji odpowiada ścieżce folderu skojarzonej z katalogiem wirtualnym aplikacji w narzędziu administracyjnym usług IIS (inetmgr).

Rozważmy na przykład następującą hierarchię aplikacji w folderze wwwroot.

`C:\inetpub\wwwroot\myapp\default.aspx`

Na potrzeby tego przykładu konto ASPNET wymaga uprawnień do odczytu zdefiniowanych powyżej dla zawartości w katalogu MojaApl i wwwroot. Pojedyncza dziedziczona lista kontroli dostępu w folderze głównym może być również opcjonalnie użyta dla obu katalogów, jeśli są one zagnieżdżone.

Aby dodać uprawnienia do katalogu, wykonaj następujące czynności:

- Korzystając z Eksploratora Windows, przejdź do katalogu
- Kliknij prawym przyciskiem myszy folder katalogu i wybierz pozycję "właściwości".
- Przejdź do karty "zabezpieczenia" w oknie dialogowym właściwości
- Kliknij przycisk "Dodaj" i wprowadź nazwę komputera, a po nim nazwę konta ASPNET. Na przykład na komputerze o nazwie "webdev" wpisz webdev\ASPNET i naciśnij przycisk "OK".
- Upewnij się, że konto ASPNET ma zaznaczone pola wyboru "Czytaj &amp; Execute", "Wyświetl zawartość folderu" i "read".
- Naciśnij przycisk OK, aby zamknąć okno dialogowe i zapisać zmiany.

![](denied-access-to-iis-directories/_static/image2.jpg)

W razie potrzeby te zmiany można zautomatyzować za pomocą skryptów lub narzędzia "cacls. exe", które jest dostarczane z systemem Windows. Aby uzyskać więcej informacji na temat konta ASPNET, zapoznaj się z [dokumentem często zadawanych pytań](https://go.microsoft.com/fwlink/?LinkId=5828).

Jeśli dana aplikacja sieci Web opiera się na uprawnieniach zapisu lub modyfikacji określonego folderu lub pliku, można to udzielić, wykonując tę samą procedurę i sprawdzając pola wyboru "Write" i/lub "Modify".

Na maszynach zezwalających wszystkim lub użytkownikom na dostęp do odczytu do tych katalogów (co jest konfiguracją domyślną) nie zostaną napotkane żadne problemy i powyższe kroki nie będą wymagane.

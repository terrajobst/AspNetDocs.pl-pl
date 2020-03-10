---
uid: whitepapers/side-by-side-with-10
title: ASP.NET Wykonywanie równoczesne .NET Framework 1,0 i 1,1 | Microsoft Docs
author: rick-anderson
description: W tym dokumencie opisano sposób instalowania na komputerze programu .NET 1,0 i programu .NET 1,1, co umożliwia uruchamianie aplikacji sieci Web ASP.NET w dowolnej wersji ramki...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: c123545099013af71569bce4707f2b3eb732c344
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632973"
---
# <a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>Wykonywanie równoczesne aplikacji .NET Framework 1.0 i 1.1 na platformie ASP.NET

> W tym dokumencie opisano sposób instalowania programów .NET 1,0 i .NET 1,1 na komputerze, co pozwala na uruchamianie aplikacji sieci Web ASP.NET w dowolnej wersji platformy.
> 
> Dotyczy ASP.NET 1,0 i ASP.NET 1,1.

W programie ASP.NET aplikacje są uruchamiane obok siebie, gdy są zainstalowane na tym samym komputerze, ale używają różnych wersji .NET Framework. W poniższym temacie opisano sposób konfigurowania aplikacji ASP.NET do wykonywania równoczesnego i zawiera szczegółowe instrukcje:

- [Zachowaj mapowanie aplikacji sieci Web na .NET Framework w wersji 1,0 podczas instalacji](#1)
- [Mapowanie aplikacji sieci Web do określonej wersji .NET Framework](#2)
- [Znajdź wersję .NET Framework, z której korzysta witryna sieci Web](#3)

Tradycyjnie, kiedy składnik lub aplikacja są aktualizowane na komputerze, Starsza wersja jest usuwana i zastępowana nowszą wersją. Jeśli nowa wersja nie jest zgodna z poprzednią wersją, zwykle przerywa inne aplikacje korzystające z składnika lub aplikacji. .NET Framework zapewnia obsługę wykonywania równoczesnego, co umożliwia zainstalowanie wielu wersji zestawu lub aplikacji na tym samym komputerze w tym samym czasie. Ze względu na to, że można zainstalować wiele wersji jednocześnie, zarządzane aplikacje mogą wybrać wersję, która ma być używana bez wpływu na aplikacje korzystające z innej wersji.

Domyślnie podczas instalacji .NET Framework w wersji 1,1 wszystkie istniejące aplikacje ASP.NET są automatycznie konfigurowane do korzystania z najnowszej wersji .NET Framework. Jeśli nie chcesz, aby aplikacje ASP.NET były domyślnie .NET Framework 1,1, kliknij [tutaj](#1) , aby dowiedzieć się, jak zapobiegać tym podczas instalacji.

Jeśli zaktualizujesz serwer sieci Web do .NET Framework 1,1 i chcesz, aby co najmniej jedna aplikacja sieci Web działała .NET Framework 1,0, musisz zaktualizować mapę skryptu Internet Information Services (IIS). Mapowanie skryptu jest mechanizmem mapowania rozszerzenia pliku aspx dla określonej aplikacji sieci Web na wersję .NET Framework. Kliknij [tutaj](#2) , aby dowiedzieć się, jak zmapować aplikację sieci Web na określoną wersję .NET Framework.

Aby sprawdzić, która wersja .NET Framework ma uruchomioną określoną aplikację sieci Web, można użyć programu Internet Information Manager lub narzędzia rejestracji ASP.NET IIS (ASPNET\_regiis. exe). Kliknij [tutaj](#3) , aby dowiedzieć się, jak znaleźć wersję .NET Framework używaną przez witrynę sieci Web.

Podczas migrowania do .NET Framework 1,1 należy wziąć pod uwagę, że każda wersja .NET Framework używa własnego pliku Machine. config. W związku z tym, jeśli administrator sieci Web wprowadził zmiany w pliku Machine. config, te zmiany muszą zostać zmigrowane do pliku Machine. config .NET Framework 1,1.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Utrzymywanie mapowania aplikacji sieci Web na .NET Framework 1,0 podczas instalacji

Domyślnie wszystkie istniejące aplikacje ASP.NET są automatycznie konfigurowane ponownie podczas instalacji w celu korzystania z nowszej wersji .NET Framework. Przy użyciu nowszej wersji .NET Framework aplikacje mogą w pełni korzystać z ulepszeń i nowych funkcji dostępnych w nowej wersji. W tym samym czasie administrator sieci Web, który może potrzebować szczegółowej kontroli nad uaktualnianymi aplikacjami, może zapobiec automatycznemu ponownemu mapowaniu wszystkich istniejących aplikacji ASP.NET podczas instalacji .NET Framework.

Aby zapobiec automatycznemu ponownemu mapowaniu całej aplikacji ASP.NET na nowszą wersję .NET Framework, administrator sieci Web może użyć opcji wiersza polecenia/noaspupgrade z programem instalacyjnym Dotnetfx. exe.

**Aby zapobiec łącznym ponownym mapowaniu aplikacji ASP.NET na nowszą wersję**

1. Przejdź do pozycji **Start**.
2. Kliknij przycisk **Uruchom**.
3. Wpisz **cmd**.
4. Kliknij przycisk **OK**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. W wierszu polecenia wpisz następujący wiersz, aby rozpocząć instalację .NET Framework: **Dotnetfx. exe/c: "Install/noaspupgrade?** .  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Kliknij przycisk **tak** w konfiguracji Microsoft .NET Framework 1,1. Spowoduje to rozpoczęcie procesu instalacji .NET Framework 1,1.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Mapowanie aplikacji sieci Web do określonej wersji .NET Framework

Każda wersja .NET Framework obejmuje wersję narzędzia rejestracji ASP.NET IIS (ASPNET\_regiis. exe). To narzędzie umożliwia administratorom określenie, że aplikacja sieci Web ma być uruchamiana w określonej wersji .NET Framework. Jest to nazywane mapowaniem aplikacji sieci Web na wersję .NET Framework. Administratorzy muszą wybrać ASPNET\_regiis. exe, który odnosi się do wersji .NET Framework, która zostanie skojarzona z aplikacją sieci Web. Na przykład administrator, który chce określić, że witryna sieci Web używa .NET Framework 1,1 musi używać ASPNET\_regiis. exe, który znajduje się w .NET Framework 1,1.

ASPNET\_regiis. exe w wersji 1,0 znajduje się w:

- C:\WINDOWS\Microsoft.NET\Framework\\**v 1.0.3705**\ASPNET\_regiis

ASPNET\_regiis. exe dla wersji 1, 1 znajduje się w:

- C:\WINDOWS\Microsoft.NET\Framework\\**v 1.1.4322**\ASPNET\_regiis

ASPNET\_regiis. exe oferuje dwie opcje mapowania skryptu aplikacji sieci Web:

- **-s** ustawia mapę skryptu w ścieżce i w jej katalogach podrzędnych.
- **-SN** ustawia mapę skryptu tylko w ścieżce.

Ścieżka definiuje ścieżkę metadanych IIS aplikacji sieci Web, która jest zdefiniowana w formie W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}. Na przykład w przypadku aplikacji sieci Web o nazwie Portal znajdującej się w domyślnej witrynie sieci Web ścieżka metabazy to W3SVC/1/ROOT/Portal.

![](side-by-side-with-10/_static/image4.gif)

Należy pamiętać, że można również użyć narzędzia o nazwie Edytor metabazy do pobrania ścieżki metabazy. To narzędzie można pobrać z witryny pomoc techniczna firmy Microsoft w [https://support.microsoft.com/default.aspx?scid=kb; en-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Uruchom ASPNET\_regiis. exe-s W3SVC/1/ROOT/Portal, aby zaktualizować mapę skryptu usług IIS portalu i jej podaplikację.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Uruchom ASPNET\_regiis. exe-SN W3SVC/1/ROOT/Portal, aby zaktualizować mapę skryptu usługi IIS portalu bez wpływu na aplikacje w podkatalogach portalu? s.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Znajdź wersję .NET Framework używaną przez aplikację sieci Web

Administrator może użyć Service Manager Internet, aby sprawdzić, która wersja .NET Framework ma uruchomioną witrynę sieci Web. Różne wersje systemu operacyjnego uruchamiają Internet Service Manager inaczej. Aby uruchomić program Service Manager, wykonaj czynności opisane poniżej.

**Aby uruchomić Internet Service Manager**

1. Przejdź do pozycji **Start**.
2. Kliknij przycisk **Uruchom**.
3. Wpisz **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. W Service Manager Internet wybierz aplikację sieci Web, której wersję .NET Framework chcesz poznać.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Kliknij prawym przyciskiem myszy aplikację sieci Web, a następnie kliknij pozycję **właściwości.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. W oknie właściwości wybierz pozycję **Konfiguracja.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. W tabeli mapowanie aplikacji wybierz pozycję **. aspx**, a następnie kliknij pozycję **Edytuj**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. W polu tekstowym **plik wykonywalny** Przyjrzyj się katalogowi Version przez przewijanie. Jeśli katalog wersji to v. 1.1.4322, aplikacja jest mapowana na .NET Framework 1,1. Z drugiej strony, jeśli katalog wersji to v 1.0.3705, aplikacja jest mapowana na .NET Framework 1,0.  
  
    ![](side-by-side-with-10/_static/image12.gif)

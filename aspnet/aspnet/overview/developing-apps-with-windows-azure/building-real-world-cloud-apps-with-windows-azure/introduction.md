---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure | Microsoft Docs
author: MikeWasson
description: Ta książka elektroniczna przeprowadzi Cię przez oparte na wzorcach podejście do tworzenia rozwiązań w chmurze w rzeczywistych warunkach. Wzorce stosują się do procesu tworzenia, a także do...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: b88573b3702b755b155e8da35f5f8a67931bafc6
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457118"
---
# <a name="building-real-world-cloud-apps-with-azure"></a>Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Ta książka elektroniczna przeprowadzi Cię przez oparte na wzorcach podejście do tworzenia rozwiązań w chmurze w rzeczywistych warunkach. Wzorce stosują się do procesu tworzenia, a także do praktyk architektury i kodowania.
> 
> Zawartość jest oparta na prezentacji opracowanej przez Scott Guthrie i dostarczonej przez nią na konferencji norweskich deweloperów (NDC) w czerwcu 2013 ([część 1](http://vimeo.com/68215538), [część 2](http://vimeo.com/68215602)), a w firmie Microsoft Tech Ed Australia we wrześniu, 2013 ([część 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [część 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Wiele innych](more-patterns-and-guidance.md#acknowledgments) zaktualizował i powiększał zawartość podczas przechodzenia z filmu wideo do postaci pisanej.

## <a name="intended-audience"></a>Docelowi odbiorcy

Deweloperzy, którzy chcesz wiedzieć się na opracowywanie zawartości w chmurze, biorąc pod uwagę przejście do chmury, lub nowe do programowania w chmurze, znajdziesz zwięzły przegląd najważniejszych koncepcji i praktyk, które muszą znać. Koncepcje są zilustrowane konkretnymi przykładami, a każdy rozdział łączy do innych zasobów, aby uzyskać bardziej szczegółowe informacje. Przykłady i linki do dodatkowych zasobów są przeznaczone dla platform i usług firmy Microsoft, ale przedstawione zasady mają zastosowanie także do innych struktur projektowania sieci Web i środowisk chmury.

Deweloperzy, którzy już opracowują chmurę, mogą znaleźć tutaj pomysły, które pozwolą zwiększyć ich powodzenie. Każdy rozdział w serii może być odczytywany niezależnie, dzięki czemu możesz wybierać i wybierać interesujące Cię tematy.

Każda osoba, która czuje Twoje *aplikacje w chmurze w świecie rzeczywistym z prezentacją platformy Azure* i potrzebuje większej ilości szczegółów i zaktualizowanych informacji, znajdzie się tutaj.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Wzorce programowania w chmurze

Ta książka elektroniczna wyjaśnia trzynaste zalecane wzorce do programowania w chmurze. "Wzorzec" jest tutaj używany w szerokim sensie, aby określić zalecaną metodę wykonywania czynności: jak najlepiej zapoznać się z projektowaniem, projektowaniem i kodowaniem aplikacji w chmurze. Są to kluczowe wzorce, które pomogą Ci "przystąpić do sukcesu", jeśli są one przestrzegane.

- [Automatyzuj wszystko](automate-everything.md).

    - Używaj skryptów, aby zmaksymalizować wydajność i zminimalizować błędy w powtarzalnych procesach.
    - Demonstracja: skrypty zarządzania platformy Azure.
- [Kontrola źródła](source-control.md). 

    - Skonfiguruj strukturę rozgałęziania w kontroli źródła, aby ułatwić przepływ pracy DevOps.
    - Demonstracja: Dodawanie skryptów do kontroli źródła.
    - Demonstracja: Zachowaj dane poufne z kontroli źródła.
    - Demonstracja: Użyj narzędzia Git w programie Visual Studio.
- [Ciągła integracja i dostarczanie](continuous-integration-and-continuous-delivery.md). 

    - Automatyzowanie kompilowania i wdrażania przy każdej kontroli źródła.
- [Najlepsze rozwiązania dotyczące programowania aplikacji sieci Web](web-development-best-practices.md). 

    - Zachowaj bezstanowy poziom warstwy sieci Web.
    - Demonstracja: skalowanie i skalowanie automatyczne w Web Apps w Azure App Service.
    - Należy unikać stanu sesji.
    - Użyj usługi CDN z rezerwą, gdy sieć CDN jest niedostępna.
    - Użyj asynchronicznego modelu programowania.
    - Demonstracja: async w ASP.NET MVC i Entity Framework.
- [Logowanie jednokrotne](single-sign-on.md). 

    - Wprowadzenie do Azure Active Directory.
    - Demonstracja: Tworzenie aplikacji ASP.NET, która używa Azure Active Directory.
- [Opcje magazynu danych](data-storage-options.md). 

    - Typy magazynów danych.
    - Jak wybrać odpowiedni magazyn danych.
    - Demonstracja: Azure SQL Database.
- [Strategie partycjonowania danych](data-partitioning-strategies.md). 

    - Podziel dane na partycje w pionie lub w poziomie, aby ułatwić skalowanie relacyjnej bazy danych.
- [Magazyn obiektów Blob bez struktury](unstructured-blob-storage.md). 

    - Przechowywanie plików w chmurze za pomocą usługi BLOB Service.
    - Demonstracja: używanie usługi BLOB Storage w aplikacji poprawki IT.
- [Zaprojektowanie w celu przetworzenia niepowodzeń](design-to-survive-failures.md). 

    - Typy niepowodzeń.
    - Zakres niepowodzeń.
    - Zrozumienie umowy SLA.
- [Monitorowanie i dane telemetryczne](monitoring-and-telemetry.md). 

    - Dlaczego należy kupować aplikację telemetryczną i pisać własny kod w celu Instrumentacji swojej aplikacji.
    - Demonstracja: nowy Relic dla platformy Azure
    - Demonstracja: rejestrowanie kodu w aplikacji poprawki IT.
    - Demonstracja: iniekcja zależności w aplikacji rozwiązania IT.
    - Demonstracja: Wbudowana obsługa rejestrowania na platformie Azure.
- [Przejściowa obsługa błędów](transient-fault-handling.md). 

    - Użyj inteligentnej logiki ponawiania/wycofywania, aby zmniejszyć wpływ błędów przejściowych.
    - Demonstracja: ponowienie/wycofanie w Entity Framework 6.
- [Buforowanie rozproszone](distributed-caching.md). 

    - Zwiększenie skalowalności i obniżenie kosztów transakcji bazy danych za pomocą rozproszonej pamięci podręcznej.
- [Wzorzec pracy skoncentrowanej na kolejkach](queue-centric-work-pattern.md). 

    - Zapewnianie wysokiej dostępności i poprawianie skalowalności dzięki swobodnemu sprzęgu warstw sieci Web i procesu roboczego.
    - Demonstracja: kolejki usługi Azure Storage w aplikacji poprawki IT.
- [Więcej wzorców i wskazówek dotyczących aplikacji w chmurze](more-patterns-and-guidance.md).
- [Dodatek: Przykładowa aplikacja Fix It](the-fix-it-sample-application.md)

    - Znane problemy
    - Najlepsze rozwiązania
    - Jak pobierać, kompilować, uruchamiać i wdrażać.

Wzorce te mają zastosowanie do wszystkich środowisk w chmurze, ale będziemy je zilustrować przy użyciu przykładów opartych na technologiach i usługach firmy Microsoft, takich jak Visual Studio, Team Foundation Service, ASP.NET i Azure.

W pozostałej części tego rozdziału przedstawiono przykładową aplikację do rozwiązywania problemów i Web Apps w Azure App Service środowisku chmury, w której działa aplikacja do rozwiązywania problemów.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>Przykładowa aplikacja do rozwiązywania problemów

Większość zrzutów ekranu i przykładów kodu pokazanych w tej książce elektronicznej jest oparta na rozwiązaniu IT, który pierwotnie opracowano przez [Scott Guthrie](https://weblogs.asp.net/scottgu/) , aby zademonstrować zalecane wzorce i praktyki projektowania aplikacji w chmurze.

![Strona główna aplikacji IT Fix it](introduction/_static/image1.png)

Przykładowa aplikacja to prosty system tworzenia biletów dla elementów roboczych. Jeśli potrzebujesz czegoś określonego, utworzysz bilet i przypiszesz go do kogoś, a inni mogą się zalogować i zobaczyć przypisane do nich bilety i oznaczyć bilety jako ukończone, gdy zakończy się prace.

Jest to standardowy projekt sieci Web programu Visual Studio. Jest on oparty na ASP.NET MVC i używa bazy danych SQL Server. Może działać lokalnie w IIS Express i można ją wdrożyć w witrynie sieci Web systemu Azure, aby uruchomić ją w chmurze. Możesz zalogować się przy użyciu uwierzytelniania formularzy i lokalnej bazy danych lub przy użyciu dostawcy społecznościowego, takiego jak Google. (W dalszej części przedstawiono również sposób logowania przy użyciu konta organizacyjnego Active Directory).

![Strona logowania](introduction/_static/image2.png)

Po zalogowaniu możesz utworzyć bilet, przypisać go do kogoś i przekazać zdjęcie, które chcesz usunąć.

![Tworzenie rozwiązania do rozwiązywania problemów](introduction/_static/image3.png)

![Napraw to zadanie](introduction/_static/image4.png)

Możesz śledzić postęp utworzonych elementów roboczych, zobaczyć bilety przypisane do Ciebie, wyświetlić szczegóły biletu i oznaczyć elementy jako ukończone.

Jest to bardzo prosta aplikacja z perspektywy funkcji, ale zobaczysz, jak ją skompilować, aby można ją było skalować do milionów użytkowników i będzie ona odporna na uszkodzenia, takie jak awarie bazy danych i zakończenie połączenia. Zobaczysz również, jak utworzyć zautomatyzowany i Agile przepływ pracy programistycznej, który umożliwia uruchamianie prostej aplikacji i ulepszanie jej przez iterację cyklu programowania.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Web Apps w Azure App Service

Środowisko chmury używane dla aplikacji do rozwiązywania problemów to usługa platformy Azure, która wywołuje witryny sieci Web. Ta usługa umożliwia hostowanie własnych aplikacji sieci Web na platformie Azure bez konieczności tworzenia maszyn wirtualnych i ich aktualizowania, instalowania i konfigurowania usług IIS itp. Twoja witryna jest hostowana na maszynach wirtualnych i automatycznie udostępnia kopie zapasowe i odzyskiwanie oraz inne usługi. Usługa witryny sieci Web współpracuje z ASP.NET, Node. js, PHP i Python. Umożliwia to szybkie wdrażanie przy użyciu programu Visual Studio, Web Deploy, FTP, Git lub TFS. Zwykle jest to zaledwie kilka sekund między rozpoczęciem wdrożenia a czasem dostępności aktualizacji przez Internet. Rozpoczęcie pracy jest bezpłatne i można skalować w górę w miarę wzrostu ruchu.

W tle Web Apps w Azure App Service udostępnia wiele składników i funkcji architektury, które trzeba skompilować, jeśli chcesz hostować witrynę sieci Web przy użyciu usług IIS na własnych maszynach wirtualnych. Jeden składnik jest punktem końcowym wdrożenia, który automatycznie konfiguruje usługi IIS i instaluje aplikację na tylu maszynach wirtualnych, w których chcesz uruchamiać lokację.

![Usługa wdrażania](introduction/_static/image5.png)

Gdy użytkownik trafi w stronę sieci Web, nie trafią bezpośrednio z maszyn wirtualnych usług IIS, przechodzą przez moduły równoważenia obciążenia [routingu żądań aplikacji (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) . Można ich używać z własnymi serwerami, ale Skorzystaj z tego, że są one automatycznie konfigurowane. Korzystają one z inteligentnego algorytmu heurystycznego, który uwzględnia takie czynniki jak koligacja sesji, Głębokość kolejki w usługach IIS i użycie procesora CPU na poszczególnych maszynach, aby skierować ruch do maszyn wirtualnych, które obsługują witrynę sieci Web.

![Moduł równoważenia obciążenia ARR](introduction/_static/image6.png)

Jeśli maszyna ulegnie awarii, platforma Azure automatycznie wyciąga ją od obrotu, przesuwa nowe wystąpienie maszyny wirtualnej i rozpoczyna kierowanie ruchu do nowego wystąpienia — wszystko to bez czasu w czasie dla aplikacji.

![Automatyczne odzyskiwanie z awarii komputera](introduction/_static/image7.png)

Wszystkie te działania są wykonywane automatycznie. Wystarczy utworzyć witrynę sieci Web i wdrożyć w niej aplikację przy użyciu programu Windows PowerShell, programu Visual Studio lub portalu zarządzania Azure.

Aby zapoznać się z szybkim i łatwym samouczekem krok po kroku, w którym pokazano, jak utworzyć aplikację sieci Web w programie Visual Studio i wdrożyć ją w witrynie sieci Web platformy Azure, zobacz [Rozpoczynanie pracy z platformą Azure i ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Podsumowanie

W tym wprowadzeniu przedstawiono listę tematów, które obejmuje książka, zrzuty ekranu przykładowej aplikacji oraz krótkie omówienie Web Apps w Azure App Service środowisku chmury. Jedną z zalet tworzenia aplikacji w programie i w chmurze jest łatwe Automatyzowanie powtarzalnych zadań programistycznych, takich jak tworzenie środowiska testowego i wdrażanie kodu. Jak to zrobić, jest to temat w [następnym rozdziale](automate-everything.md).

## <a name="resources"></a>Zasoby

Aby uzyskać więcej informacji na temat tematów omówionych w tym rozdziale, zobacz następujące zasoby.

Łączoną

- [Web Apps w Azure App Service](https://azure.microsoft.com/services/app-service/web/). Strona portalu dla dokumentacji platformy Azure dotyczącej Web Apps.
- [Web Apps, Cloud Services i maszyny wirtualne: Kiedy używać tego programu?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS, jak pokazano w tym rozdziale, to tylko jeden z trzech sposobów uruchamiania aplikacji sieci Web na platformie Azure. W tym artykule wyjaśniono różnice między trzema sposobami i przedstawiono wskazówki dotyczące wyboru, który z nich jest odpowiedni dla danego scenariusza. Podobnie jak w przypadku witryn sieci Web, Cloud Services jest funkcją PaaS platformy Azure. Maszyny wirtualne są funkcją IaaS. Aby uzyskać wyjaśnienie PaaS i IaaS, zobacz rozdział [opcji danych](data-storage-options.md#paasiaas) .

Filmy wideo:

- [Scott Guthrie jest uruchamiany w kroku 0 — co to jest system operacyjny w chmurze platformy Azure?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Architektura witryn sieci Web — z Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Wewnętrzne witryny sieci Web platformy Azure z imię Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [Dalej](automate-everything.md)

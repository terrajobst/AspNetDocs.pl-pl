---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Ciągła integracja i ciągłe dostarczanie (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure) | Microsoft Docs
author: MikeWasson
description: Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: cf3c65ef95528173eed3fb08984035b2512861c4
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457040"
---
# <a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Ciągła integracja i ciągłe dostarczanie (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure)

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure** jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc w pomyślnym tworzeniu aplikacji sieci Web dla chmury. Aby uzyskać informacje na temat książki elektronicznej, zobacz [pierwszy rozdział](introduction.md).

Pierwsze dwa zalecane wzorce procesów programistycznych były [automatyzacją wszystkiego](automate-everything.md) i [kontroli źródła](source-control.md), a trzeci wzorzec procesu łączy je. Ciągła integracja (CI) oznacza, że za każdym razem, gdy deweloper sprawdzi kod w repozytorium źródłowym, kompilacja jest automatycznie wyzwalana. Ciągłe dostarczanie (CD) obejmuje ten jeden krok: po pomyślnym zakończeniu kompilacji i zautomatyzowanej testów jednostkowych aplikacja zostanie automatycznie wdrożona w środowisku, w którym można bardziej szczegółowo przeprowadzić testy.

Chmura pozwala zminimalizować koszty utrzymania środowiska testowego, ponieważ płacisz tylko za zasoby środowiska, o ile są one używane. Proces tworzenia dysku CD może skonfigurować środowisko testowe, gdy będzie to potrzebne, a po zakończeniu testowania można podjąć odpowiednie środowisko.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Przepływ pracy ciągłej integracji i ciągłego dostarczania

Zwykle zalecamy przeprowadzenie ciągłego dostarczania w środowiskach deweloperskich i przejściowych. Większość zespołów, nawet w firmie Microsoft, wymaga ręcznego przeglądu i procesu zatwierdzania dla wdrożenia produkcyjnego. W przypadku wdrożenia produkcyjnego warto upewnić się, że występuje, gdy kluczowe osoby w zespole programistycznym są dostępne do obsługi lub w okresie niskiego obciążenia. Nie ma nic, aby zapobiec całkowitemu automatyzowaniu środowisk deweloperskich i testowych, dzięki czemu wszyscy deweloperzy mają zaewidencjonować zmiany, a środowisko jest skonfigurowane do testowania akceptacji.

Na poniższym diagramie z [wzorców i praktyk firmy Microsoft książka elektroniczna dotycząca ciągłego dostarczania](https://aka.ms/ReleasePipeline) ilustruje typowy przepływ pracy. Kliknij obraz, aby zobaczyć jego pełny rozmiar w pierwotnym kontekście.

[przepływ pracy ciągłego dostarczania ![](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Jak Chmura umożliwia opłacalną CI i CD

Automatyzacja tych procesów na platformie Azure jest prosta. Ponieważ korzystasz z wszystkiego w chmurze, nie musisz kupować i zarządzać serwerami dla kompilacji lub środowisk testowych. Nie musisz czekać, aż serwer będzie dostępny do testowania. W przypadku każdej kompilacji, którą można wykonać, możesz uruchomić środowisko testowe na platformie Azure przy użyciu skryptu automatyzacji, uruchamiać testy akceptacji lub bardziej szczegółowe testy, a następnie po prostu wyłączać je. Jeśli tylko ten serwer jest uruchamiany przez 2 godziny lub 8 godzin lub dzień, kwota pieniędzy, za które należy zapłacić, jest minimalna, ponieważ płacisz tylko za czas, w którym maszyna jest rzeczywiście uruchomiona. Na przykład środowisko wymagane do naprawienia aplikacji IT w pierwszej wysokości kosztuje od 1 centa na godzinę, jeśli przejdziesz do warstwy Bezpłatna. W ciągu miesiąca, jeśli tylko środowisko zostało uruchomione w danym momencie, środowisko testowe prawdopodobnie jest tańsze niż latte, które kupujesz w Starbucks.

## <a name="azure-devops-services"></a>Usługa Azure DevOps Services 

Azure DevOps Services udostępnia wiele funkcji, które ułatwiają tworzenie aplikacji od planowania do wdrożenia.

- Obsługuje zarówno funkcję kontroli źródła git (Distributed), jak i TFVC (scentralizowana).
- Oferuje elastyczną usługę kompilacji, co oznacza, że dynamicznie tworzy serwery kompilacji, gdy są potrzebne, i po ich zakończeniu wykonuje je. Możesz automatycznie uruchomić kompilację, gdy ktoś sprawdzi zmiany kodu źródłowego, i nie musisz przydzielić i zanieść opłat za własne serwery kompilacji, które są w większości czasu bezczynne. Usługa kompilacji jest bezpłatna, dopóki nie zostanie przekroczona określona liczba kompilacji. Jeśli oczekujesz dużej ilości kompilacji, możesz uregulować nieco dodatkowe dla zarezerwowanych serwerów kompilacji.
- Obsługuje ona ciągłe dostarczanie na platformę Azure.
- Obsługuje zautomatyzowane testowanie obciążenia. Testowanie obciążenia ma kluczowe znaczenie dla aplikacji w chmurze, ale jest często zaniedbane do momentu zbyt opóźnienia. Testowanie obciążenia symuluje duże wykorzystanie aplikacji przez tysiące użytkowników, umożliwiając znalezienie wąskich gardeł i zwiększenie przepływności — przed udostępnieniem aplikacji w środowisku produkcyjnym.
- Obsługuje ona współpracę w pokoju zespołu, która ułatwia komunikację i współpracę w czasie rzeczywistym dla małych zespołów Agile.
- Obsługuje zarządzanie projektami Agile.

Aby uzyskać więcej informacji na temat funkcji ciągłej integracji i dostarczania Azure DevOps Services, zobacz [dokumentację usługi Azure DevOps](/azure/devops/index).

Jeśli szukasz rozwiązania do zarządzania projektami, współpracy zespołowej i kontroli źródła, Wyewidencjonuj Azure DevOps Services. Zarejestruj się w [Azure DevOps Services](https://dev.azure.com/).

## <a name="summary"></a>Podsumowanie

Pierwsze trzy wzorce programowania w chmurze zostały zaimplementowane, jak zaimplementować powtarzalny, niezawodny i przewidywalny proces opracowywania z czasem małego cyklu. W [następnym rozdziale](web-development-best-practices.md) Zacznijmy od wzorca architektury i kodowania.

## <a name="resources"></a>Zasoby

Aby uzyskać więcej informacji, zobacz [wdrażanie aplikacji sieci Web w Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Zobacz również następujące zasoby:

- [Tworzenie potoku wydania z Team Foundation Server 2012](https://aka.ms/ReleasePipeline). Książka elektroniczna, szkolenia praktyczne i przykładowy kod według wzorców i praktyk firmy Microsoft zapewnia szczegółowe wprowadzenie do ciągłego dostarczania. Obejmuje korzystanie z Visual Studio Lab Management i Release Management programu Visual Studio.
- [Alm Ranges DevOps i wskazówki](https://aka.ms/vsarsolutions/). Zakresy ALM wprowadzają rozwiązanie towarzyszące przykładowi DevOps Workbench i praktyczne wskazówki w obszarze współpraca ze wzorcami &amp;, który *kompilują potok wydania z serwerem tfs 2012*, jako doskonały sposób na rozpoczęcie uczenia koncepcji DevOps &amp; Release Management dla TFS 2012 oraz do uruchamiania tych danych. Wskazówki przedstawiają sposób kompilowania i wdrażania w wielu środowiskach.
- [Testowanie ciągłego dostarczania przy użyciu programu Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). Książka elektroniczna według wzorców i praktyk firmy Microsoft wyjaśnia, jak zintegrować zautomatyzowane testowanie z ciągłym dostarczaniem.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Kod źródłowy narzędzia przeznaczonego do przechwytywania kompilacji z programu TFS (na podstawie etykiety), kompilowania go, tworzenia pakietu, zezwalania komuś w roli DevOps na konfigurowanie określonych aspektów IT i wypychanie ich do platformy Azure. Narzędzie śledzi proces wdrażania w celu włączenia operacji "wycofywania" do wcześniej wdrożonej wersji. Narzędzie nie ma zależności zewnętrznych i może działać autonomicznie przy użyciu interfejsów API programu TFS i zestawu Azure SDK.
- [Ciągłe dostarczanie: niezawodne wersje oprogramowania za poorednictwem automatyzacji kompilowania, testowania i wdrażania](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Książka według Jez Humble.
- [Wydanie, aby projektować i wdrażać oprogramowanie gotowe do produkcji](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Książka przez Jan T. Nygarda.

> [!div class="step-by-step"]
> [Poprzednie](source-control.md)
> [dalej](web-development-best-practices.md)

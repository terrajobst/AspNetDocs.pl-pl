---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/more-patterns-and-guidance
title: Więcej wzorców i wskazówek (Tworzenie rzeczywistych aplikacji w chmurze na platformie Azure) | Microsoft Docs
author: MikeWasson
description: Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7e97cfc3-d830-4002-8ff7-5790d1ff49e6
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/more-patterns-and-guidance
msc.type: authoredcontent
ms.openlocfilehash: 57e32bf7568ecb0eb22f0f2b585310dcab2d5d43
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457079"
---
# <a name="more-patterns-and-guidance-building-real-world-cloud-apps-with-azure"></a>Więcej wzorców i wskazówek (Tworzenie rzeczywistych aplikacji w chmurze na platformie Azure)

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure** jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc w pomyślnym tworzeniu aplikacji sieci Web dla chmury. Aby uzyskać informacje na temat książki elektronicznej, zobacz [pierwszy rozdział](introduction.md).

Zobaczysz już 13 wzorców, które zapewniają wskazówki dotyczące pomyślnego działania w chmurze obliczeniowej. Są to tylko kilka wzorców, które mają zastosowanie do aplikacji w chmurze. Poniżej przedstawiono kilka dodatkowych tematów dotyczących przetwarzania w chmurze i zasobów ułatwiających ich korzystanie z nich:

- Migrowanie istniejących aplikacji lokalnych do chmury. 

    - [Przeniesienie aplikacji do chmury](https://msdn.microsoft.com/library/ff728592.aspx). Książka elektroniczna według wzorców i praktyk firmy Microsoft. Dostępna również jako [twarda kopia drukowanego podręcznika](https://www.amazon.com/dp/1621140202).
    - [Migrowanie ASP.NET i IIS.NET firmy Microsoft](https://go.microsoft.com/fwlink/?LinkId=400656). Analiza przypadku przez Robert McMurraya.
    - [Przeniesienie czwartego &amp; Mayor do witryny sieci Web systemu Azure](http://www.jeff.wilcox.name/2013/04/4thandmayor-azure-websites/). Wpis w blogu firmy Jan Wilcox chroniczne środowisko w celu przeniesienia aplikacji internetowej z Amazon Web Services do Web Apps w Azure App Service.
    - [Przenosisz aplikacje na platformę Azure: jakie zmiany?](https://azure.microsoft.com/documentation/videos/web-sites-internals-and-the-file-system/) Krótkie wideo według Stefan Schackow, objaśnia dostęp do systemu plików w Web Apps w Azure App Service.
    - [Chmura hybrydowa platformy Azure](https://www.amazon.com/dp/B00EOP4UQW). Książka Hardcopy lub książka elektroniczna według Danny Garber, Jamal Malik i Adam Fazio.
- Problemy dotyczące zabezpieczeń, uwierzytelniania i autoryzacji, które są unikatowe dla aplikacji w chmurze

    - [Wskazówki dotyczące zabezpieczeń platformy Azure](https://azure.microsoft.com/blog/2014/02/10/best-practices-windows-azure-websites-waws/)
    - [Wzorce i praktyki firmy Microsoft — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz wzorzec strażnika, wzorzec tożsamości federacyjnej.
    - [Zabezpieczenia sieci platformy Azure](https://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx). Oficjalny dokument przez Ashin Palekarem.

Zobacz również dodatkowe wzorce obliczeniowe chmury i wskazówki dotyczące [wzorców i praktyk firmy Microsoft — wskazówki dotyczące platformy Azure](https://msdn.microsoft.com/library/dn568099.aspx).

<a id="resources"></a>
## <a name="resources"></a>Zasoby

Każdy rozdział w tej książce elektronicznej zawiera linki do zasobów, aby uzyskać więcej informacji na temat tego konkretnego tematu. Poniższa lista zawiera linki do omówienia najlepszych rozwiązań i zalecanych wzorców związanych z pomyślnym programowaniem w chmurze przy użyciu platformy Azure.

Dokumentacja

- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę w usłudze Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Oficjalny dokument ze znakami SIMM i Michael Thomassy.
- [Failsafe: wskazówki dotyczące odpornych architektur chmurowych](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Oficjalny dokument według wytłoczyn Mercuri, Ulrich Homann i Andrew TOWNHILL. Wersja strony sieci Web serii wideo FailSafe.
- [Wskazówki dotyczące platformy Azure](https://azure.microsoft.com/develop/net/guidance/) Strona portalu do oficjalnej dokumentacji dotyczącej tworzenia aplikacji dla platformy Azure.

Filmy wideo

- [Tworzenie rzeczywistych aplikacji w chmurze na platformie Azure — część 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324) i [część 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325). Film wideo przedstawiający prezentację przez Scott Guthrie, na której bazuje ta książka elektroniczna. Przedstawione na konferencji Tech Ed Australia we wrześniu 2013. Starsza wersja tej samej prezentacji została dostarczona na konferencji norweski Developers (NDC) w czerwcu 2013: [NDC część 1](http://vimeo.com/68215538), [NDC część 2](http://vimeo.com/68215602).
- [Failsafe: kompilowanie skalowalnych, Odpornych Cloud Services](https://channel9.msdn.com/Series/FailSafe). Seria wideo dziewięć części przez Ulrich Homann, Marc Mercuri i marking SIMM. Przedstawia widok 400 na poziomie tworzenia architektury aplikacji w chmurze. Ta seria koncentruje się na teorii i przyczynach związanych z zalecanymi wzorcami; Aby uzyskać więcej informacji na temat szczegółów, zobacz Tworzenie dużych serii przez znaczniki SIMM.
- [Tworzenie dużych: lekcje uzyskane od klientów platformy Azure — część 1](https://channel9.msdn.com/Events/Build/2012/3-029) i [część 2](https://channel9.msdn.com/Events/Build/2012/3-030). Seria wideo z dwoma częściami przez Simon Davies i Markuje moduły SIMM, podobnie jak seria FailSafe, ale zorientowane się bardziej na praktyczne wdrożenie.

Przykład kodu

- [Poprawka do tej książki elektronicznej](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4?cdn_id=2013-12-03-002).
- [Podstawy usługi w chmurze na platformie Azure C# w programie dla programu Visual Studio 2012](https://aka.ms/csf). Projekt do pobrania w witrynie galerii kodu firmy Microsoft zawiera kod i dokumentację opracowaną przez zespół ds. pomocy technicznej firmy Microsoft. Demonstruje wiele najlepszych rozwiązań, które zostały zaprezentowane w FailSafe i tworzeniu dużych serii wideo i FailSafe oficjalny dokument. Strona Galeria kodu łączy się również z obszerną dokumentacją przez autorów projektu — Zobacz szczególnie łącze [Kolekcja stron typu wiki usługi w chmurze](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx) w niebieskim polu w górnej części opisu projektu. Ten projekt i dokumentacja nadal aktywnie opracowywanych, dzięki czemu jest to lepszy wybór w celu uzyskania informacji na temat wielu tematów, takich jak podobne, ale starsze dokumenty.

Książki z kopiami

- [Chmura obliczeniowa Bible](https://www.amazon.com/dp/0470903562). Autor Barrie Sosinsky.
- [Wydanie, aby projektować i wdrażać oprogramowanie gotowe do produkcji](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Przez Jan T. Nygarda.
- [Wzorce architektury chmury: używanie Microsoft Azure](http://shop.oreilly.com/product/0636920023777.do). Według rachunku Wilder.
- [Platforma Windows Azure](https://www.amazon.com/dp/1430235632). Autor Tejaswi Redkara.
- [Wzorce programowania systemu Windows Azure dla uruchamiania](https://www.amazon.com/dp/1849685606). Autor Riccardo Becker.
- [Microsoft Windows Azure Development Cookbook](https://www.amazon.com/dp/1849682224). Autor Neila Mackenzie.

Na koniec po rozpoczęciu tworzenia rzeczywistych aplikacji i uruchamiania ich na platformie Azure prawdopodobnie będziesz potrzebować pomocy od ekspertów. Możesz zadawać pytania w witrynach społeczności, takich jak [fora platformy Azure lub StackOverflow](https://azure.microsoft.com/support/forums/), albo skontaktować się bezpośrednio z firmą Microsoft w celu uzyskania pomocy technicznej platformy Azure. Firma Microsoft oferuje kilka poziomów pomocy technicznej platformy Azure: Aby zapoznać się z podsumowaniem i porównaniem opcji, zobacz [Pomoc techniczna platformy Azure](https://azure.microsoft.com/support/plans/).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Potwierdzeń

Ta zawartość została zapisywana przez Tomasz Dykstra, Rick Anderson i Jan Wasson. Większość oryginalnej zawartości pochodzi z [Scott Guthrie](https://weblogs.asp.net/scottgu/), a firma z kolei nanosi materiały z znaczników SIMM i działu Doradczego firmy Microsoft (Cat).

Wielu innych współpracowników w firmie Microsoft zrecenzowany i dodał komentarze dotyczące wersji roboczych i kodu:

- Tim Ammann — zbadano rozdział usługi Automation.
- Christopher Bennage — sprawdzona i przetestowana poprawka kodu IT.
- Ryanów z przejrzałych.
- Vittorio Bertocci — przegląd rozdziału logowania jednokrotnego.
- Krzysztof Clayton — pomaga rozwiązywać problemy techniczne ze skryptami programu PowerShell.
- Conor Cunningham — Przegląd opcji magazynu danych.
- Carlos Farre — sprawdzona i przetestowana poprawka kodu dla problemów z zabezpieczeniami.
- Larry Piotrs — przegląd działu telemetrii i monitorowania.
- Jonathana Gao — przeanalizowane sekcje Hadoop i MapReduce w rozdziale opcje magazynu danych.
- Sidney Higa — przeanalizowane wszystkie rozdziały.
- Gordon Hogenson — przegląd rozdziału kontroli źródła.
- Tamra Myers — Przegląd opcji magazynu danych, obiektów blob i kolejek.
- Pranav Rastogi — przegląd rozdziału logowania jednokrotnego.
- Czerwiec Blender Rogers — dodano obsługę błędów i pomoc dotyczącą skryptów automatyzacji programu PowerShell.
- Mani Subramanian — sprawdzono wszystkie rozdziały i przeprowadził proces przeglądu kodu i testowania dla tego kodu.
- Shaun Tinline-Kowalski-zbadano rozdział partycjonowania danych.
- Selcine Tukarslan — analizowane rozdziały obejmujące SQL Database i SQL Server.
- Edward Wu — podano przykładowy kod dla rozdziału logowania jednokrotnego.
- Guang przerywają Yang — Zapisano skrypty automatyzacji programu PowerShell.

Członkowie [Rady Doradczej wskazówki dla deweloperów firmy Microsoft](https://aka.ms/DGAC) (DGAC) również przeglądali i komentarz dotyczący wersji roboczych:

- Jean-Luc Boucho
- Catalin Gheorghiu
- Wouter de kort
- Carlos dos Santos
- Neila Mackenzie
- Dennis Persson
- Sunil Saba
- [Aleksey Sinyagin](http://www.linkedin.com/in/sinyagin)
- Wagner rachunku
- Michael drewno

Inni członkowie DGAC zrecenzowani i komentarze do wstępnego konspektu:

- Damir Arh
- Edward Bakker
- Srdjan Bozovic
- Kanał Ming Man
- Gianni Rosa Gallina
- Paulo Morgado
- Jason Oliveira
- Alberto Poblacion
- Ryan Riley
- Perez Nowak Tsisah
- Roger Whitehead
- Pawel Wilkosz

> [!div class="step-by-step"]
> [Poprzednie](queue-centric-work-pattern.md)
> [dalej](the-fix-it-sample-application.md)

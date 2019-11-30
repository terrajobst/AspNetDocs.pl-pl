---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: Strategie partycjonowania danych (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure) | Microsoft Docs
author: MikeWasson
description: Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 2f79b1f459aff3e81dab7ea7eb4ebf3f71084463
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585816"
---
# <a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Strategie partycjonowania danych (Tworzenie aplikacji w chmurze w rzeczywistych warunkach na platformie Azure)

przez [Jan Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tomasz Dykstra](https://github.com/tdykstra)

[Pobierz poprawkę](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) lub [Pobierz książkę elektroniczną](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Tworzenie aplikacji w chmurze w świecie rzeczywistym za pomocą książki elektronicznej platformy Azure** jest oparte na prezentacji opracowanej przez Scott Guthrie. Wyjaśniono 13 wzorców i praktyk, które mogą pomóc w pomyślnym tworzeniu aplikacji sieci Web dla chmury. Informacje o serii znajdują [się w pierwszym rozdziale](introduction.md).

Wcześniej zawarto jak łatwo skalować warstwę sieci Web aplikacji w chmurze, dodając i usuwając serwery sieci Web. Ale jeśli wszyscy naprzódją ten sam magazyn danych, wąskie gardła aplikacji przemieszcza się z frontonu do zaplecza, a warstwa danych jest najtrudniejsza do skalowania. W tym rozdziale zawarto informacje na temat skalowania warstwy danych przez Partycjonowanie danych do wielu relacyjnych baz danych lub łączenie magazynu relacyjnej bazy danych z innymi opcjami przechowywania danych.

Konfigurowanie schematu partycjonowania najlepiej naprzód z tego samego powodu wymienionego wcześniej: bardzo trudno jest zmienić strategię magazynowania danych, gdy aplikacja jest w środowisku produkcyjnym. Jeśli myślisz o różnych podejściach, możesz uniknąć wystąpienia "w serwisie Twitter", gdy aplikacja ulegnie awarii lub przez długi czas, w trakcie ponownego organizowania danych i kodu dostępu do danych aplikacji.

## <a name="the-three-vs-of-data-storage"></a>Trzy a przechowywanie danych

Aby określić, czy potrzebna jest strategia partycjonowania i jakie powinna być, weź pod uwagę trzy pytania dotyczące danych:

- Wolumin — ile danych będzie ostatecznie przechowywanych? Kilka gigabajtów? Kilka setek gigabajtów? Terabajtów? Petabajtów?
- Szybkość — jaka jest szybkość, z jaką będą się zwiększać dane? Czy jest to wewnętrzna aplikacja, która nie generuje dużej ilości danych? Aplikacja zewnętrzna, do której klienci będą przekazywać obrazy i wideo?
- Odmiana — jakiego typu dane będą przechowywane? Relacyjne, obrazy, pary klucz-wartość, wykresy społecznościowe?

Jeśli uważasz, że chcesz mieć dużo ilości, szybkości lub różnorodności, musisz starannie rozważyć, jakiego rodzaju schemat partycjonowania najlepiej umożliwi aplikacji skalowanie w sposób wydajny i efektywny, w miarę wzrostu i zapewnienia, że nie będzie można uruchamiać żadnych wąskich gardeł.

Istnieją zasadniczo trzy podejścia do partycjonowania:

- Partycjonowanie pionowe
- Partycjonowanie poziome
- Partycjonowanie hybrydowe

## <a name="vertical-partitioning"></a>Partycjonowanie pionowe

Pionowa część jest taka sama jak dzielenie tabeli według kolumn: jeden zestaw kolumn należy do jednego magazynu danych, a inny zestaw kolumn przechodzi do innego magazynu danych.

Załóżmy na przykład, że moja aplikacja przechowuje dane dotyczące osób, w tym obrazów:

![Tabela danych](data-partitioning-strategies/_static/image1.png)

Gdy dane są reprezentowane jako tabela i przyjrzyj się różnym odmianom danych, można zobaczyć, że trzy kolumny po lewej stronie mają dane ciągów, które mogą być efektywnie przechowywane przez relacyjną bazę danych, natomiast dwie kolumny po prawej stronie są tablicami bajtowymi, które są niektó z plików obrazów. Istnieje możliwość przechowywania danych plików obrazów w relacyjnej bazie danych, a wiele osób może zrobić, ponieważ nie chcą zapisywać danych w systemie plików. Mogą nie mieć systemu plików z możliwością przechowywania wymaganych woluminów danych lub mogą nie chcieć zarządzać osobnym systemem tworzenia kopii zapasowej i przywracania. Takie podejście działa dobrze w przypadku lokalnych baz danych i małych ilości danych w bazach danych w chmurze. W środowisku lokalnym może być łatwiej, aby tylko DBA o wszystko.

Jednak w bazie danych w chmurze magazyn jest stosunkowo kosztowny, a duża ilość obrazów może zwiększyć rozmiar bazy danych poza granicami, w których może wydajnie działać. Te problemy można rozwiązać przez Partycjonowanie danych w pionie, co oznacza, że można wybrać najbardziej odpowiedni magazyn danych dla każdej kolumny w tabeli danych. Co może być najlepszym rozwiązaniem dla tego przykładu jest umieszczenie danych ciągu w relacyjnej bazie danych i w obrazach w usłudze BLOB Storage.

![Tabela danych w pionie](data-partitioning-strategies/_static/image2.png)

Przechowywanie obrazów w magazynie obiektów blob, a nie w bazie danych, jest bardziej praktyczne w chmurze niż w środowisku lokalnym, ponieważ nie trzeba martwić się o Konfigurowanie serwerów plików ani zarządzanie kopią zapasową danych przechowywanych poza relacyjną bazą danych: wszystkie obsługiwane przez usługę BLOB Storage automatycznie.

Jest to podejście do partycjonowania wdrożone w aplikacji do rozwiązywania problemów IT i poinformujemy kod o tym w [rozdziale usługi BLOB Storage](unstructured-blob-storage.md). Bez tego schematu partycjonowania i przy założeniu średniego rozmiaru obrazu wynoszącego 3 megabajty, poprawka aplikacji IT będzie mogła przechowywać informacje o 40 000 zadaniach przed upływem maksymalnego rozmiaru bazy danych wynoszącego 150 gigabajty. Po usunięciu obrazów baza danych może przechowywać 10 razy tyle zadań; Możesz kontynuować pracę znacznie dłużej, zanim będzie trzeba myśleć o implementacji schematu partycjonowania poziomego. W miarę skalowania aplikacji Twoje wydatki rosną wolniej, ponieważ większość potrzeb związanych z magazynem odbywa się bardzo niedrogim magazynem obiektów BLOB.

## <a name="horizontal-partitioning-sharding"></a>Partycjonowanie poziome (fragmentowania)

Podział w poziomie jest podobny do dzielenia tabeli według wierszy: jeden zestaw wierszy przejdzie w jeden magazyn danych, a inny zestaw wierszy przechodzi do innego magazynu danych.

Uwzględniając ten sam zestaw danych, kolejną opcją jest przechowywanie różnych zakresów nazw klientów w różnych bazach danych.

![Tabela danych w poziomie partycjonowana](data-partitioning-strategies/_static/image3.png)

Należy zachować ostrożność w odróżnieniu od schematu fragmentowania, aby upewnić się, że dane są równomiernie dystrybuowane w celu uniknięcia aktywnych miejsc. Ten prosty przykład przy użyciu pierwszej litery nazwiska nie spełnia tego wymagania, ponieważ wiele osób ma nazwiska, które zaczynają się od niektórych typowych liter. Ograniczenia rozmiaru tabeli trafią wcześniej niż można spodziewać się, że niektóre bazy danych byłyby bardzo duże, a większość pozostanie niewielka.

Po stronie partycjonowania poziomego może być trudne wykonywanie zapytań we wszystkich danych. W tym przykładzie zapytanie będzie musiało zostać narysowane z maksymalnie 26 różnych baz danych, aby uzyskać wszystkie dane przechowywane przez aplikację.

## <a name="hybrid-partitioning"></a>Partycjonowanie hybrydowe

Można połączyć partycjonowanie pionowe i poziome. Przykładowo w przykładowych danych można przechowywać obrazy w usłudze BLOB Storage i dzielić je na partycje w poziomie.

![Podzielona na partycje hybrydowe tabeli danych](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Partycjonowanie aplikacji produkcyjnej

Koncepcyjnie można łatwo sprawdzić, jak działa schemat partycjonowania, ale każdy schemat partycjonowania zwiększa złożoność kodu i wprowadza wiele nowych komplikacji, z którymi należy się zająć. Jeśli przenosisz obrazy do magazynu obiektów blob, co robi, gdy usługa magazynu nie działa? Jak obsługiwać zabezpieczenia obiektów BLOB? Co się stanie, jeśli baza danych i magazyn obiektów BLOB nie zostaną zsynchronizowane? Jeśli fragmentowania, w jaki sposób będzie obsługiwał zapytania dla wszystkich baz danych?

Komplikacje można zarządzać tak długo, jak planujesz je przed przejściem do środowiska produkcyjnego. Wielu osób, które nie zrobiły, chcą później. W przypadku średniej nasz zespół ds. doradztwa klienta pobiera panicked rozmowy telefonicznej o jeden miesiąc od klientów, których aplikacje są wyłączane w bardzo dużym zakresie i nie przeprowadzą tego planowania. I mówią o tym, jak: "Help! Umieszczam wszystko w jednym magazynie danych, a w ciągu 45 dni brakuje miejsca na tym komputerze. A jeśli masz wiele logiki biznesowej wbudowanych w sposób uzyskiwania dostępu do magazynu danych i masz klientów, którzy korzystają z aplikacji, nie masz wystarczającej ilości czasu na przeprowadzenie migracji. Jesteśmy w trakcie przechodzenia przez Herculean wysiłki, aby pomóc klientom podzielić swoje dane na bieżąco bez czasu. Jest to bardzo atrakcyjne i bardzo scarye, a nie coś, czego potrzebujesz, jeśli można to uniknąć! Zastanawiasz się na tym przed chwilą i integrujemy ją z Twoją aplikacją, dzięki czemu Twoja praca będzie znacznie łatwiejsza w przypadku późniejszego rozrastania aplikacji.

## <a name="summary"></a>Podsumowanie

Efektywny schemat partycjonowania może umożliwić aplikacji w chmurze skalowanie do petabajtów danych w chmurze bez wąskich gardeł. Nie trzeba więc wyliczać z góry za duże maszyny lub rozległą infrastrukturę, która może być używana w przypadku uruchamiania aplikacji w lokalnym centrum danych. W chmurze możesz zwiększyć pojemność w miarę potrzeb i płacić tylko za tyle, ile korzystasz z niego podczas korzystania z niego.

W [następnym rozdziale](unstructured-blob-storage.md) zobaczymy, jak aplikacja Poprawka It implementuje partycjonowanie pionowe przez przechowywanie obrazów w magazynie obiektów BLOB.

## <a name="resources"></a>Resources

Aby uzyskać więcej informacji na temat strategii partycjonowania, zobacz następujące zasoby.

Łączoną

- [Najlepsze rozwiązania dotyczące projektowania usług na dużą skalę na platformie Microsoft Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Oficjalny dokument ze znakami SIMM i Michael Thomassy.
- [Wzorce i praktyki firmy Microsoft — wzorce projektowania w chmurze](https://msdn.microsoft.com/library/dn568099.aspx). Zobacz wskazówki dotyczące partycjonowania danych, wzorzec fragmentowania.

Filmy wideo:

- [Failsafe: kompilowanie skalowalnych, Odpornych Cloud Services](https://channel9.msdn.com/Series/FailSafe). Seria dziewięciu części przez Ulrich Homann, Marc Mercuri i marking SIMM. Prezentuje koncepcje wysokiego poziomu i zasady architektury w bardzo dostępnym i interesującym scenariuszu, w tym scenariusze opracowane przez firmę Microsoft Customer Advisory Team (CAT) z rzeczywistymi klientami. Zobacz dyskusję partycjonowania w epizod 7.
- [Tworzenie dużych: lekcji uzyskane od klientów systemu Windows Azure — część I](https://channel9.msdn.com/Events/Build/2012/3-029). Oznacz moduły SIMM omawiają schematy partycjonowania, strategie fragmentowania, sposób implementacji fragmentowania i SQL Database federacyjne, począwszy od 19:49. Podobnie jak w przypadku serii failsafe, ale więcej szczegółów.

Przykładowy kod:

- [Podstawy usługi w chmurze w systemie Windows Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Przykładowa aplikacja, która zawiera bazę danych podzielonej na fragmenty. Opis zaimplementowanego schematu fragmentowania można znaleźć w temacie [dal – fragmentowania of RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) in the Windows Azure blog.

> [!div class="step-by-step"]
> [Poprzednie](data-storage-options.md)
> [dalej](unstructured-blob-storage.md)

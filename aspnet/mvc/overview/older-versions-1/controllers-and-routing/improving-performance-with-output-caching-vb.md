---
uid: mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
title: Poprawa wydajności dzięki buforowaniu danych wyjściowych (VB) | Microsoft Docs
author: microsoft
description: W tym samouczku dowiesz się, jak znacznie zwiększyć wydajność aplikacji sieci Web ASP.NET MVC, wykorzystując buforowanie danych wyjściowych. Ty...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 0e7b4d85-2c46-4eaf-b6a8-6cd566a67334
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/improving-performance-with-output-caching-vb
msc.type: authoredcontent
ms.openlocfilehash: b713b56e149f196794b3223ba88e3b41bf3e34c4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601249"
---
# <a name="improving-performance-with-output-caching-vb"></a>Poprawa wydajności dzięki buforowaniu danych wyjściowych (VB)

przez [firmę Microsoft](https://github.com/microsoft)

> W tym samouczku dowiesz się, jak znacznie zwiększyć wydajność aplikacji sieci Web ASP.NET MVC, wykorzystując buforowanie danych wyjściowych. Dowiesz się, jak buforować wynik zwrócony z akcji kontrolera, tak aby ta sama zawartość nie musiała zostać utworzona, i za każdym razem, gdy nowy użytkownik wywoła akcję.

Celem tego samouczka jest wyjaśnienie, jak można znacząco poprawić wydajność aplikacji ASP.NET MVC przez wykorzystanie wyjściowej pamięci podręcznej. Wyjściowa pamięć podręczna umożliwia buforowanie zawartości zwróconej przez akcję kontrolera. Dzięki temu ta sama zawartość nie musi być generowana za każdym razem, gdy wywoływana jest taka sama akcja kontrolera.

Załóżmy na przykład, że aplikacja ASP.NET MVC wyświetla listę rekordów bazy danych w widoku o nazwie index. Zwykle każda i za każdym razem, gdy użytkownik wywołuje akcję kontrolera zwracającą widok indeksu, zestaw rekordów bazy danych musi zostać pobrany z bazy danych, wykonując zapytanie bazy danych.

Jeśli z drugiej strony korzystasz z wyjściowej pamięci podręcznej, możesz uniknąć wykonywania zapytania w bazie danych za każdym razem, gdy każdy użytkownik wywoła tę samą akcję kontrolera. Widok można pobrać z pamięci podręcznej, a nie ponownie wygenerować z poziomu akcji kontrolera. Buforowanie pozwala uniknąć wykonywania nadmiarowej pracy na serwerze.

#### <a name="enabling-output-caching"></a>Włączanie buforowania danych wyjściowych

Buforowanie danych wyjściowych można włączyć, dodając atrybut &lt;OutputCache&gt; do akcji pojedynczego kontrolera lub całej klasy kontrolera. Na przykład kontroler na liście 1 uwidacznia akcję o nazwie index (). Dane wyjściowe akcji index () są buforowane przez 10 sekund.

**Lista 1 – Controllers\HomeController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample1.vb)]

W wersjach beta ASP.NET MVC buforowanie danych wyjściowych nie działa dla adresu URL, takiego jak [http://www.MySite.com/](http://www.mysite.com/). Zamiast tego należy wprowadzić adres URL, taki jak [http://www.MySite.com/Home/Index](http://www.mysite.com/Home/Index).

W przypadku listy 1 dane wyjściowe akcji index () są buforowane przez 10 sekund. Jeśli wolisz, możesz określić znacznie dłuższy czas trwania pamięci podręcznej. Na przykład jeśli chcesz buforować dane wyjściowe akcji kontrolera przez jeden dzień, możesz określić czas trwania pamięci podręcznej wynoszący 86400 sekund (60 s \* 60 minut \* 24 godzin).

Nie ma gwarancji, że zawartość będzie buforowana przez określony czas. Gdy zasoby pamięci stają się niskie, pamięć podręczna zaczyna wykluczać zawartość automatycznie.

Kontroler Home na liście 1 zwraca widok indeksu na liście 2. Nie ma żadnych specjalnych informacji o tym widoku. Widok indeks po prostu wyświetla bieżącą godzinę (patrz rysunek 1).

**Lista 2 — Views\Home\Index.aspx**

[!code-aspx[Main](improving-performance-with-output-caching-vb/samples/sample2.aspx)]

**Rysunek 1 — Widok indeksu buforowanego**

![clip_image002](improving-performance-with-output-caching-vb/_static/image1.jpg)

Jeśli wywołajesz akcję index () wiele razy, wprowadzając adres URL/Home/Index na pasku adresu przeglądarki i naciskając wielokrotnie przycisk Odśwież/Załaduj ponownie w przeglądarce, czas wyświetlany przez widok indeksu nie zostanie zmieniony na 10 sekund. Ten sam czas jest wyświetlany, ponieważ widok jest buforowany.

Ważne jest, aby zrozumieć, że ten sam widok jest buforowany dla wszystkich osób odwiedzających Twoją aplikację. Każda osoba, która wywołuje akcję index (), będzie mieć tę samą buforowaną wersję widoku indeksu. Oznacza to, że ilość pracy, którą serwer sieci Web musi wykonać, aby obsłużyć widok indeksu, jest znacznie zmniejszona.

Widok na liście 2 ma być bardzo prosty. Widok po prostu wyświetla bieżącą godzinę. Można jednak równie łatwo w pamięci podręcznej widok, który wyświetla zestaw rekordów bazy danych. W takim przypadku zestaw rekordów bazy danych nie musi zostać pobrany z bazy danych i za każdym razem, gdy akcja kontrolera, która zwraca ten widok jest wywoływana. Buforowanie może zmniejszyć ilość pracy, jaką musi wykonać zarówno serwer sieci Web, jak i serwer baz danych.

Nie używaj strony &lt;% @ OutputCache%&gt; w widoku MVC. Ta dyrektywa jest wykrwawiania z świata formularzy sieci Web i nie powinna być używana w aplikacji ASP.NET MVC. 

#### <a name="where-content-is-cached"></a>Miejsce, w którym zawartość jest buforowana

Domyślnie, jeśli używasz atrybutu &lt;OutputCache&gt;, zawartość jest buforowana w trzech lokalizacjach: serwer sieci Web, wszystkie serwery proxy i przeglądarka sieci Web. Można kontrolować, gdzie zawartość jest buforowana, modyfikując właściwość Location obiektu &lt;OutputCache&gt; Attribute.

Właściwość Location można ustawić na dowolną jedną z następujących wartości:

> · Ile
> 
> · Klient
> 
> · Licznik
> 
> · Server
> 
> · Dawaj
> 
> · ServerAndClient

Domyślnie właściwość Location ma wartość any. Istnieją jednak sytuacje, w których może być potrzebna pamięć podręczna tylko w przeglądarce lub tylko na serwerze. Na przykład w przypadku buforowania informacji, które są personalizowane dla każdego użytkownika, nie należy buforować informacji na serwerze. Jeśli są wyświetlane różne informacje dla różnych użytkowników, należy buforować informacje tylko na kliencie.

Na przykład kontroler na liście 3 uwidacznia akcję o nazwie GetName (), która zwraca bieżącą nazwę użytkownika. Jeśli wtyczka loguje się do witryny sieci Web i wywołuje akcję GetName (), Akcja zwraca ciąg "Hi Jack". W takim przypadku usługa Jill loguje się do witryny sieci Web i wywołuje akcję GetName (), a następnie uzyska ciąg "Hi Jack". Ten ciąg jest buforowany na serwerze sieci Web dla wszystkich użytkowników, gdy wtyczka początkowo wywołuje akcję kontrolera.

**Lista 3 — Controllers\BadUserController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample3.vb)]

Najprawdopodobniej kontroler na liście 3 nie działa tak, jak chcesz. Nie chcesz wyświetlać komunikatu "Hi Jack" do Jill.

Nigdy nie należy buforować spersonalizowanej zawartości w pamięci podręcznej serwera. Jednak może być konieczne buforowanie spersonalizowanej zawartości w pamięci podręcznej przeglądarki w celu zwiększenia wydajności. Jeśli zawartość zostanie zbuforowana w przeglądarce, a użytkownik wywoła tę samą akcję kontrolera wiele razy, zawartość może zostać pobrana z pamięci podręcznej przeglądarki, a nie na serwerze.

Zmodyfikowany kontroler z listy 4 buforuje dane wyjściowe akcji GetName (). Jednak zawartość jest buforowana tylko w przeglądarce, a nie na serwerze. Dzięki temu w przypadku wywołania metody GetName () przez wielu użytkowników każda osoba otrzymuje własną nazwę użytkownika, a nie nazwę użytkownika innej osoby.

**Lista 4 – Controllers\UserController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample4.vb)]

Zwróć uwagę, że atrybut &lt;OutputCache&gt; na liście 4 zawiera właściwość lokalizacji ustawioną na wartość OutputCacheLocation. Client. Atrybut &lt;OutputCache&gt; również zawiera właściwość NoStore. Właściwość NoStore służy do informowania serwerów proxy i przeglądarek, że nie powinny przechowywać trwałej kopii pamięci podręcznej.

#### <a name="varying-the-output-cache"></a>Różnicowanie wyjściowej pamięci podręcznej

W niektórych sytuacjach można chcieć mieć różne wersje pamięci podręcznej o tej samej zawartości. Załóżmy na przykład, że tworzysz stronę wzorcową/szczegółową. Na stronie wzorca zostanie wyświetlona lista tytułów filmu. Po kliknięciu tytułu otrzymujesz szczegóły dotyczące wybranego filmu.

Jeśli przejdziesz do strony szczegółów, szczegóły dla tego samego filmu będą wyświetlane niezależnie od tego, który film został kliknięty. Pierwszy film wybrany przez pierwszego użytkownika będzie wyświetlany wszystkim użytkownikom w przyszłości.

Ten problem można rozwiązać, korzystając z właściwości VaryByParam&gt; atrybutu &lt;OutputCache. Ta właściwość pozwala tworzyć różne wersje pamięci podręcznej o tej samej zawartości, gdy parametr formularza lub parametr ciągu zapytania różni się.

Na przykład kontroler na liście 5 uwidacznia dwie akcje o nazwie Master () i Details (). Akcja Master () zwraca listę tytułów filmu i akcja szczegóły () zwraca szczegóły wybranego filmu.

**Lista 5 – Controllers\MoviesController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample5.vb)]

Akcja Master () zawiera właściwość VaryByParam o wartości "none". Gdy jest wywoływana akcja główna (), zwracana jest ta sama wersja w pamięci podręcznej widoku głównego. Wszystkie parametry formularza lub parametry ciągu zapytania są ignorowane (patrz rysunek 2).

**Rysunek 2 — widok/Movies/Master**

![clip_image004](improving-performance-with-output-caching-vb/_static/image2.jpg)

**Rysunek 3 — widok/Movies/Details**

![clip_image006](improving-performance-with-output-caching-vb/_static/image3.jpg)

Akcja szczegóły () zawiera właściwość VaryByParam o wartości "ID". Gdy różne wartości parametru ID są przesyłane do akcji kontrolera, generowane są różne wersje w pamięci podręcznej widoku szczegółów.

Ważne jest, aby zrozumieć, że użycie właściwości VaryByParam powoduje zwiększenie buforowania i nie mniejsze. Inna wersja w pamięci podręcznej widoku szczegółów jest tworzona dla każdej innej wersji parametru ID.

Właściwość VaryByParam można ustawić na następujące wartości:

> \* = Utwórz inną buforowaną wersję za każdym razem, gdy wartość parametru lub ciągu zapytania jest różna.
> 
> Brak = nigdy nie twórz różnych wersji zapisanych w pamięci podręcznej
> 
> Średnikowa lista parametrów = Twórz różne wersje w pamięci podręcznej, gdy dowolne parametry formularza lub ciągu zapytania na liście różnią się

#### <a name="creating-a-cache-profile"></a>Tworzenie profilu pamięci podręcznej

Alternatywnie, aby skonfigurować właściwości wyjściowej pamięci podręcznej przez modyfikację właściwości &lt;atrybutu&gt; OutputCache, można utworzyć profil pamięci podręcznej w pliku konfiguracji sieci Web (Web. config). Tworzenie profilu pamięci podręcznej w pliku konfiguracji sieci Web oferuje kilka ważnych korzyści.

Najpierw przez skonfigurowanie buforowania danych wyjściowych w pliku konfiguracji sieci Web, można kontrolować sposób, w jaki akcje kontrolera buforują zawartość w jednej centralnej lokalizacji. Można utworzyć jeden profil pamięci podręcznej i zastosować profil do kilku kontrolerów lub akcji kontrolera.

Następnie można zmodyfikować plik konfiguracji sieci Web bez ponownego kompilowania aplikacji. Jeśli konieczne jest wyłączenie buforowania dla aplikacji, która została już wdrożona w środowisku produkcyjnym, można po prostu zmodyfikować profile pamięci podręcznej zdefiniowane w pliku konfiguracji sieci Web. Wszelkie zmiany w pliku konfiguracji sieci Web zostaną wykryte automatycznie i zastosowane.

Na przykład sekcja &lt;buforowanie&gt; konfiguracja sieci Web na liście 6 definiuje profil pamięci podręcznej o nazwie Cache1Hour. Sekcja&gt; pamięci podręcznej &lt;musi znajdować się w sekcji &lt;system. Web&gt; pliku konfiguracji sieci Web.

**Lista 6 — sekcja pamięci podręcznej dla pliku Web. config**

[!code-xml[Main](improving-performance-with-output-caching-vb/samples/sample6.xml)]

Kontroler z listy 7 ilustruje, jak można zastosować profil Cache1Hour do akcji kontrolera z atrybutem&gt; &lt;OutputCache.

**Lista 7 – Controllers\ProfileController.vb**

[!code-vb[Main](improving-performance-with-output-caching-vb/samples/sample7.vb)]

Jeśli wywołasz akcję index () uwidocznioną przez kontroler na liście 7, to ten sam czas będzie zwracany przez 1 godzinę.

#### <a name="summary"></a>Podsumowanie

Buforowanie danych wyjściowych zapewnia bardzo łatwą metodę znacząco ulepszania wydajności aplikacji ASP.NET MVC. W tym samouczku przedstawiono sposób użycia atrybutu&gt; &lt;OutputCache do buforowania danych wyjściowych akcji kontrolera. Zawarto również informacje na temat modyfikowania właściwości &lt;OutputCache&gt; Attribute, takich jak właściwości Duration i VaryByParam, aby zmodyfikować sposób, w jaki zawartość jest buforowana. Na koniec wiesz już, jak definiować profile pamięci podręcznej w pliku konfiguracji sieci Web.

> [!div class="step-by-step"]
> [Poprzednie](understanding-action-filters-vb.md)
> [dalej](adding-dynamic-content-to-a-cached-page-vb.md)

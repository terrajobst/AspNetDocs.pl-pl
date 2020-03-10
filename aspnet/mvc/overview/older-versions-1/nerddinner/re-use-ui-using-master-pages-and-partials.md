---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Ponowne używanie interfejsu użytkownika za pomocą stron wzorcowych i częściowych | Microsoft Docs
author: microsoft
description: Krok 7. analizujemy sposoby zastosowania "zasady SUCHEj" w naszych szablonach widoków w celu wyeliminowania duplikowania kodu przy użyciu częściowych szablonów widoków i stron wzorcowych.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 0b17cb6ac14b7f187bf1f175097a37907689d46e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580333"
---
# <a name="re-use-ui-using-master-pages-and-partials"></a>Ponowne używanie interfejsu użytkownika za pomocą stron wzorcowych i częściowych

przez [firmę Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Jest to krok 7 bezpłatnego [samouczka dotyczącego aplikacji "NerdDinner"](introducing-the-nerddinner-tutorial.md) , który zawiera instrukcje tworzenia niewielkiej, ale kompletnej aplikacji sieci Web przy użyciu ASP.NET MVC 1.
> 
> Krok 7. analizujemy sposoby zastosowania "zasady SUCHEj" w naszych szablonach widoków w celu wyeliminowania duplikowania kodu przy użyciu częściowych szablonów widoków i stron wzorcowych.
> 
> Jeśli używasz ASP.NET MVC 3, zalecamy użycie [wprowadzenie ze samouczkami ze sklepu MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) lub [MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-7-partials-and-master-pages"></a>NerdDinner krok 7: części i strony wzorcowe

Jednym z projektów filozofiami ASP.NET MVC to "nie powtarzaj sobie" (powszechnie określany jako "SUCHy"). SUCHy projekt pomaga wyeliminować duplikowanie kodu i logiki, co ostatecznie sprawia, że aplikacje są szybciej kompilowane i łatwiejsze w obsłudze.

Ta zasada została już zastosowana w kilku naszych scenariuszach NerdDinner. Kilka przykładów: Nasza logika walidacji jest implementowana w ramach naszej warstwy modelu, co umożliwia wymuszanie jej w ramach obu scenariuszy edycji i tworzenia w naszym kontrolerze; Używamy szablonu widoku "NotFound" w ramach metod edycji, szczegółów i usuwania. korzystamy ze wzorca nazewnictwa Konwencji z naszymi szablonami widoków, co eliminuje konieczność jawnego określenia nazwy podczas wywoływania metody pomocnika view (). i ponownie korzystamy z klasy DinnerFormViewModel dla scenariuszy akcji Edytuj i Utwórz.

Teraz przyjrzyjmy się sposobom zastosowania "zasady SUCHEj" w naszych szablonach widoków, aby wyeliminować również duplikaty kodu.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Odwiedzanie naszych szablonów edycji i tworzenia widoków

Obecnie korzystamy z dwóch różnych szablonów widoku — "Edit. aspx" i "Create. aspx" — aby wyświetlić nasz interfejs użytkownika formularza obiadu. Szybkie wizualne porównywanie ich pokazuje, jak przypominają one podobne. Poniżej przedstawiono wygląd formularza:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

Poniżej przedstawiono wygląd formularza "Edytuj":

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Czy istnieje zbyt wiele różnic? Oprócz tytułu i tekstu nagłówka, układ formularza i kontrolki wprowadzania są identyczne.

Jeśli otworzysz szablony widoków "Edit. aspx" i "Create. aspx", zobaczymy, że zawierają one identyczny układ formularza i kod kontroli wejścia. To duplikowanie oznacza, że chcemy, aby zmiany były wprowadzane dwa razy, gdy wprowadzimy lub zmienimy nową właściwość obiadu, która nie jest dobra.

### <a name="using-partial-view-templates"></a>Korzystanie z szablonów widoku częściowego

ASP.NET MVC obsługuje możliwość definiowania szablonów "widok częściowy", których można użyć do hermetyzacji logiki renderowania widoku dla podczęści strony. "Częściowe" zapewniają przydatny sposób definiowania logiki renderowania widoku, a następnie ponownie używać jej w wielu miejscach w aplikacji.

Aby pomóc "w wyschnięciu" z naszymi szablonami Edit. aspx i Create. aspx, można utworzyć szablon widoku częściowego o nazwie "DinnerForm. ascx", który hermetyzuje układ formularza i elementy wejściowe wspólne dla obu tych elementów. W tym celu należy kliknąć prawym przyciskiem myszy katalog/Views/Dinners i wybrać polecenie menu "Dodawanie&gt;":

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Spowoduje to wyświetlenie okna dialogowego "Dodawanie widoku". Zmienimy nazwę nowego widoku, który chcemy utworzyć "DinnerForm", zaznacz pole wyboru "Utwórz widok częściowy" w oknie dialogowym i wskaż, że przekażemy go do klasy DinnerFormViewModel:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Po kliknięciu przycisku "Dodaj" program Visual Studio utworzy nowy szablon widoku "DinnerForm. ascx" dla nas w katalogu "\Views\Dinners".

Następnie możemy skopiować/wkleić zduplikowany układ formularza/kod kontroli danych wejściowych z naszego szablonu Edytuj. aspx/Create. aspx w nowym szablonie widoku "DinnerForm. ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Następnie możemy zaktualizować nasze szablony edycji i tworzenia widoków w celu wywołania częściowego szablonu DinnerForm i wyeliminowania duplikowania formularzy. Możemy to zrobić, wywołując plik HTML. RenderPartial ("DinnerForm") w naszych szablonach widoków:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Można jawnie zakwalifikować ścieżkę częściowego szablonu podczas wywoływania kodu HTML. RenderPartial (na przykład: ~ widoki/obiady/DinnerForm. ascx "). W naszym powyżej kodzie firma Microsoft korzysta ze wzorca nazewnictwa opartego na Konwencji w ramach ASP.NET MVC i po prostu określa "DinnerForm" jako nazwę częściowej do renderowania. Gdy to zrobimy, ASP.NET MVC będzie wyglądał najpierw w katalogu widoków opartych na konwencji (w przypadku DinnersController to/Views/Dinners). Jeśli nie znajdzie częściowego szablonu, będzie on następnie szukać w katalogu/Views/Shared.

Gdy plik HTML. RenderPartial () jest wywoływany przy użyciu tylko nazwy widoku częściowego, ASP.NET MVC przejdzie do widoku częściowego tego samego modelu i obiektów słownika ViewData, które są używane przez szablon widoku wywołującego. Alternatywnie istnieją przeciążone wersje języka HTML. RenderPartial (), które umożliwiają przekazywanie alternatywnego obiektu modelu i/lub słownika ViewData dla widoku częściowego do użycia. Jest to przydatne w scenariuszach, w których chcesz tylko przekazać podzestaw pełnego modelu/ViewModel.

| **Temat po stronie: Dlaczego &lt;%%&gt; zamiast &lt;% =%&gt;?** |
| --- |
| Jednym z delikatnych cech, z którymi może być zauważalny kod, jest użycie bloku &lt;%%&gt; zamiast &lt;% =%&gt; podczas wywoływania pliku HTML. RenderPartial (). &lt;% =%&gt; bloki w ASP.NET wskazują, że programista chce renderować określoną wartość (na przykład: &lt;% = "Hello"%&gt; będzie renderować "Hello"). &lt;%%&gt; zamiast tego wskazują, że deweloper chce wykonać kod i że wszystkie renderowane dane wyjściowe muszą zostać wykonane jawnie (na przykład: &lt;% Response. Write ("Witaj")%&gt;. Powód użycia bloku &lt;%%&gt; z naszym kodem HTML. RenderPartial powyżej jest to spowodowane tym, że metoda html. RenderPartial () nie zwraca ciągu, a zamiast tego wyprowadza zawartość bezpośrednio do strumienia wyjściowego szablonu widoku wywołującego. Ma to na celu zwiększenie wydajności i pozwala uniknąć konieczności tworzenia obiektu ciągu tymczasowego (potencjalnie bardzo dużego). Zmniejsza to użycie pamięci i zwiększa ogólną przepływność aplikacji. Jeden typowy błąd podczas korzystania z języka HTML. RenderPartial () polega na dodaniu średnika na końcu wywołania, gdy znajduje się w bloku &lt;%%&gt;. Na przykład ten kod spowoduje błąd kompilatora: &lt;% html. RenderPartial ("DinnerForm")%&gt; zamiast tego należy napisać: &lt;% html. RenderPartial ("DinnerForm"); %&gt; jest to spowodowane tym, że &lt;%%&gt; są niezależnymi instrukcjami kodu i w C# przypadku używania instrukcji Code należy kończyć średnikami. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Korzystanie z szablonów widoku częściowego w celu wyjaśnienia kodu

Utworzyliśmy szablon widoku częściowego "DinnerForm", aby uniknąć duplikowania logiki renderowania widoku w wielu miejscach. Jest to najbardziej typowy powód tworzenia szablonów widoku częściowego.

Czasami nadal warto tworzyć częściowe widoki nawet wtedy, gdy są one wywoływane tylko w jednym miejscu. Bardzo skomplikowane szablony widoków często stają się znacznie łatwiejsze do odczytania, gdy logika renderowania widoku jest wyodrębniana i partycjonowana do jednego lub większej liczby dobrze wymienionych szablonów częściowych.

Rozważmy na przykład poniższy fragment kodu z pliku site. Master w naszym projekcie (wkrótce będzie on wyświetlany). Kod jest relatywnie prosty do odczytu — częściowo, ponieważ logika wyświetlania linku logowania/wylogowania w prawym górnym rogu ekranu jest hermetyzowana w części "LogOnUserControl":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Za każdym razem, gdy znajdziesz odmylić, spróbuj zrozumieć kod HTML/znacznik w szablonie widoku, rozważ, czy nie będzie to wyraźniejsze, jeśli niektóre z nich zostały wyodrębnione i wdrożone w dobrze nazwanych widokach częściowych.

### <a name="master-pages"></a>Strony wzorcowe

Oprócz obsługi widoków częściowych, ASP.NET MVC obsługuje również możliwość tworzenia szablonów "Strona wzorcowa", których można użyć do zdefiniowania wspólnego układu i pliku HTML najwyższego poziomu. Kontrolki symbol zastępczy zawartości można następnie dodać do strony wzorcowej, aby identyfikować regiony, które mogą zostać zastąpione lub wypełnione przez widoki. Zapewnia to bardzo efektywny (i SUCHy) sposób zastosowania wspólnego układu w aplikacji.

Domyślnie nowe projekty ASP.NET MVC mają automatycznie dodawane do nich szablon strony głównej. Ta strona wzorcowa ma nazwę "site. master" i znajduje się w folderze \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

Domyślny plik site. Master wygląda jak poniżej. Definiuje on zewnętrzny kod HTML witryny wraz z menu nawigacji w górnej części strony. Zawiera dwie kontrolki symbolu zastępczego zawartości — jeden dla tytułu, a drugi, dla którego należy zastąpić podstawową zawartość strony:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Wszystkie szablony widoków utworzone dla naszej aplikacji NerdDinner ("list", "Details", "Edit", "Create", "NotFound" itp.) zostały oparte na tym szablonie witryny. Master. Jest to wskazywane za pośrednictwem atrybutu "MasterPageFile", który został domyślnie dodany do górnej &lt;% @ Page%&gt; dyrektywy podczas tworzenia widoków przy użyciu okna dialogowego "Dodaj widok":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Oznacza to, że możemy zmienić zawartość witryny. Master i spowodować, że zmiany są automatycznie stosowane i używane podczas renderowania dowolnego z naszych szablonów widoków.

Zaktualizujmy sekcję nagłówkową witryny. Master, aby nagłówek naszej aplikacji miał wartość "NerdDinner", a nie "moja aplikacja MVC". Zaktualizujmy również nasze menu nawigacji, tak aby pierwsza karta "znalazła się na obiad" (obsługiwana przez metodę akcji index () HomeController () i dodamy nową kartę o nazwie "host a obiad" (obsłużoną przez metodę akcji Create () DinnersController):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

W przypadku zapisania pliku site. Master i odświeżenia naszej przeglądarki zobaczymy, że nasze zmiany w nagłówku będą widoczne dla wszystkich widoków w naszej aplikacji. Na przykład:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

I z adresem URL */Dinners/Edit/[ID]* :

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Następny krok

Części i strony wzorcowe zapewniają bardzo elastyczne opcje umożliwiające przejrzyste organizowanie widoków. Zobaczysz, że pomogą Ci uniknąć duplikowania zawartości widoku/kodu i ułatwić odczytywanie i konserwowanie szablonów widoku.

Przejdźmy teraz do utworzonej wcześniej scenariusza tworzenia listy i włączania skalowalnej obsługi stronicowania.

> [!div class="step-by-step"]
> [Poprzednie](use-viewdata-and-implement-viewmodel-classes.md)
> [dalej](implement-efficient-data-paging.md)

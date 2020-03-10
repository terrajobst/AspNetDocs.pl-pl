---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Strony wzorcowe | Microsoft Docs
author: microsoft
description: Jednym ze najważniejszych składników do pomyślnej witryny sieci Web jest spójny wygląd i działanie. W ASP.NET 1. x deweloperzy korzystali z formantów użytkownika do replikowania wspólnych elem stron...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: 36f2caf7c2c9bcafd22c8f6681c1d6b19fe5078a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78567334"
---
# <a name="master-pages"></a>Strony wzorcowe

przez [firmę Microsoft](https://github.com/microsoft)

> Jednym ze najważniejszych składników do pomyślnej witryny sieci Web jest spójny wygląd i działanie. W ASP.NET 1. x deweloperzy korzystali z formantów użytkownika do replikowania wspólnych elementów strony w aplikacji sieci Web. Chociaż jest to oczywiście rozwiązaniem, korzystanie z kontrolek użytkownika ma pewne wady. Na przykład zmiana położenia kontrolki użytkownika wymaga zmiany na wiele stron w witrynie. Formanty użytkownika nie są również renderowane w widok Projekt po wstawieniu na stronie.

Jednym ze najważniejszych składników do pomyślnej witryny sieci Web jest spójny wygląd i działanie. W ASP.NET 1. x deweloperzy korzystali z formantów użytkownika do replikowania wspólnych elementów strony w aplikacji sieci Web. Chociaż jest to oczywiście rozwiązaniem, korzystanie z kontrolek użytkownika ma pewne wady. Na przykład zmiana położenia kontrolki użytkownika wymaga zmiany na wiele stron w witrynie. Formanty użytkownika nie są również renderowane w widok Projekt po wstawieniu na stronie.

ASP.NET 2,0 wprowadza strony wzorcowe jako sposób utrzymywania spójnego wyglądu i działania oraz jak zobaczysz, że strony wzorcowe reprezentują znaczącą poprawę w porównaniu z metodą kontroli użytkownika.

## <a name="why-master-pages"></a>Dlaczego strony główne?

Może być zastanawiasz się, dlaczego strony główne były potrzebne w ASP.NET 2,0. Wszyscy deweloperzy witryn sieci Web używają już formantów użytkownika w ASP.NET 1. x do udostępniania obszarów zawartości między stronami. Istnieje kilka powodów, dla których kontrolki użytkownika to mniej niż optymalne rozwiązanie do tworzenia wspólnego układu.

Kontrolki użytkownika nie definiują w rzeczywistości układu strony. Zamiast tego definiują układ i funkcjonalność części strony. Różnica między tymi dwoma jest ważna, ponieważ sprawia, że zarządzanie rozwiązaniem kontroli użytkownika znacznie trudniejsze. Na przykład jeśli chcesz zmienić położenie kontrolki użytkownika na stronie, musisz edytować stronę rzeczywistą, na której zostanie wyświetlona kontrolka użytkownika. Która się, jeśli masz tylko kilka stron, ale w dużych lokacjach szybko to okropnej zarządzanie lokacją.

Inna Wadą korzystania z kontrolek użytkownika do definiowania wspólnego układu jest katalog główny w architekturze ASP.NET. Jeśli jakikolwiek publiczny element członkowski kontrolki użytkownika zostanie zmieniony, wymaga ponownego skompilowania wszystkich stron, które używają kontrolki użytkownika. Z kolei ASP.NET będzie ponownie JIT strony podczas pierwszego dostępu do nich. Dzięki temu program tworzy nieskalowalną architekturę i problem z zarządzaniem lokacją w większych lokacjach.

Oba te problemy (i wiele innych) są dobrze rozkierowane przez strony wzorcowe w ASP.NET 2,0.

## <a name="how-master-pages-work"></a>Jak działają strony wzorcowe

Strona wzorcowa jest analogiczna do szablonu dla innych stron. Elementy strony, które powinny być współużytkowane przez inne strony (tj. menu, obramowania itp.), są dodawane do strony wzorcowej. Po dodaniu nowych stron do witryny można je skojarzyć ze stroną wzorcową. Strona, która jest skojarzona ze stroną wzorcową, nosi nazwę **strony zawartości**. Domyślnie strona zawartości przyjmuje wygląd na stronie wzorcowej. Jednak podczas tworzenia strony wzorcowej można zdefiniować fragmenty strony, które strona zawartości może zamienić na własną zawartość. Te części są definiowane przy użyciu nowej kontrolki wprowadzonej w ASP.NET 2,0; formant **ContentPlaceHolder** .

Strona główna może zawierać dowolną liczbę kontrolek ContentPlaceHolder (lub wcale nie istnieje). Na stronie zawartość, zawartość z formantów ContentPlaceHolder pojawia się wewnątrz kontrolek **zawartości** , kolejną nową kontrolką w ASP.NET 2,0. Domyślnie formanty zawartości stron zawartości są puste, aby można było zapewnić własną zawartość. Jeśli chcesz użyć zawartości z poziomu strony wzorcowej wewnątrz kontrolek zawartości, możesz to zrobić, ponieważ będzie ona widoczna w dalszej części tego modułu. Kontrolka zawartości jest mapowana na formant ContentPlaceHolder za pośrednictwem atrybutu ContentPlaceHolderID formantu Content. Poniższy kod mapuje formant zawartości na formant ContentPlaceHolder o nazwie mainBody na stronie wzorcowej.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Często użytkownicy będą chcieli opisywać strony wzorcowe jako klasę bazową dla innych stron. Która faktycznie nie prawda. Relacja między stronami wzorcowymi a stronami zawartości nie jest jednym z dziedziczenia.

**Rysunek 1** przedstawia stronę wzorcową i skojarzoną z nią stronę zawartości, która pojawia się w programie Visual Studio 2005. Formant ContentPlaceHolder można zobaczyć na stronie wzorcowej i odpowiadającej jej kontroli zawartości na stronie zawartość. Zauważ, że zawartość stron wzorcowych spoza elementu ContentPlaceHolder jest widoczna, ale wyszarzona na stronie zawartość. Tylko zawartość wewnątrz elementu ContentPlaceHolder może być supplanted na stronie zawartości. Cała inna zawartość, która pochodzi ze strony głównej, jest niezmienna.

![Strona wzorcowa i skojarzona z nią zawartość](master-pages/_static/image1.jpg)

**Rysunek 1**: Strona wzorcowa i skojarzona z nią strona zawartości

## <a name="creating-a-master-page"></a>Tworzenie strony wzorcowej

Aby utworzyć nową stronę wzorcową:

1. Otwórz program Visual Studio 2005 i Utwórz nową witrynę sieci Web.
2. Kliknij plik, nowy, plik.
3. Wybierz pozycję plik główny z okna dialogowego Dodaj nowy element, jak pokazano na **rysunku 2**.
4. Kliknij przycisk Dodaj.

![Tworzenie nowej strony wzorcowej](master-pages/_static/image2.jpg)

**Rysunek 2**. Tworzenie nowej strony wzorcowej

Zwróć uwagę, że rozszerzenie pliku dla strony głównej to *. Master*. Jest to jeden z metod, które różnią się od zwykłej strony. Druga podstawowa różnica polega na tym, że w przypadku dyrektywy @Page Strona główna zawiera @Master dyrektywy. Przełącz się do widoku źródła dla strony wzorcowej, która została właśnie utworzona, i Przejrzyj kod.

Nowa Strona główna ma domyślnie jeden formant ContentPlaceHolder. W większości przypadków warto najpierw utworzyć wspólne elementy strony, a następnie wstawić kontrolki ContentPlaceHolder, w których jest wymagana zawartość niestandardowa. W takich przypadkach deweloperzy będą chcieć usunąć domyślną kontrolkę ContentPlaceHolder i wstawić nowe w miarę rozwijania strony. Kontrolki ContentPlaceHolder nie są zmieniane, pomimo faktu, że są wyświetlane uchwyty rozmiaru. Rozmiary formantów ContentPlaceHolder są automatycznie oparte na zawartości zawartej w jednym wyjątku; Jeśli umieścisz formant ContentPlaceHolder wewnątrz elementu bloku, takiego jak komórka tabeli, rozmiar będzie zmieniany w zależności od rozmiaru elementu.

## <a name="lab-1-working-with-master-pages"></a>Laboratorium 1 — Praca ze stronami wzorcowymi

W tym laboratorium utworzysz nową stronę wzorcową i zdefiniujesz trzy kontrolki ContentPlaceHolder. Następnie utworzysz nową stronę zawartości i zastąpisz zawartość z co najmniej jednej kontrolki ContentPlaceHolder.

1. Utwórz stronę wzorcową i Wstaw kontrolki ContentPlaceHolder. 

    1. Utwórz nową stronę wzorcową zgodnie z powyższym opisem.
    2. Usuń domyślną kontrolkę ContentPlaceHolder.
    3. Wybierz formant ContentPlaceHolder, klikając przycieniowaną górną krawędź kontrolki, a następnie usuń ją, naciskając klawisz DEL na klawiaturze.
    4. Wstaw nową tabelę przy użyciu *nagłówka i szablonu po stronie* , jak pokazano na rysunku 3. Zmień szerokość i wysokość na 90%, aby cała tabela była widoczna w projektancie.

![](master-pages/_static/image3.jpg)

**Rysunek 3**

1. Umieść kursor w każdej komórce tabeli i ustaw właściwość *VAlign* na *Top*.
2. Z przybornika Wstaw formant ContentPlaceHolder w górnej komórce tabeli (komórka nagłówka).
3. Po wstawieniu tego formantu ContentPlaceHolder zobaczysz, że wysokość wiersza zajmie prawie całą stronę, jak pokazano na rysunku 4. Nie dotyczy to w tym momencie.

![Puste miejsce znajduje się w tej samej komórce co ContentPlaceHolder](master-pages/_static/image1.gif)

**Ilustracja 4**. puste miejsce znajduje się w tej samej komórce co ContentPlaceHolder

1. Umieść formant ContentPlaceHolder w dwóch pozostałych komórkach. Po wstawieniu innych kontrolek ContentPlaceHolder rozmiar komórek tabeli powinien być taki jak oczekiwano. Strona powinna teraz wyglądać podobnie jak strona pokazana na **rysunku 5**.

![Wzorzec ze wszystkimi kontrolkami ContentPlaceHolder. Należy zauważyć, że wysokość komórki dla komórki nagłówka jest teraz to, co powinno być](master-pages/_static/image2.gif)

**Rysunek 5**: wzorzec ze wszystkimi kontrolkami elementów ContentPlaceHolder. Należy zauważyć, że wysokość komórki dla komórki nagłówka jest teraz to, co powinno być

1. Wprowadź wybrany tekst do każdego z trzech formantów ContentPlaceHolder.
2. Zapisz stronę wzorcową jako Exercise1. Master.
3. Utwórz nowy formularz sieci Web i skojarz go z Exercise1. Master Master.
4. Wybierz plik, nowy, plik w programie Visual Studio 2005.
5. W oknie dialogowym Dodaj nowy element wybierz pozycję **formularz sieci Web** .
6. Upewnij się, że pole wyboru Wybierz stronę wzorcową jest zaznaczone, jak pokazano na rysunku 6.

![Dodawanie nowej strony zawartości](master-pages/_static/image3.gif)

**Ilustracja 6**. Dodawanie nowej strony zawartości

1. Kliknij przycisk Dodaj.
2. Wybierz Exercise1. Master w oknie dialogowym Wybierz stronę wzorcową, jak pokazano na rysunku 7.
3. Kliknij przycisk OK, aby dodać nową stronę zawartości.

Nowa zawartość zostanie wyświetlona w programie Visual Studio z jedną kontrolką zawartości dla każdej kontrolki ContentPlaceHolder na stronie wzorcowej. Domyślnie formanty zawartości są puste, aby można było dodać własną zawartość. Jeśli chcesz, aby użytkownicy korzystali z zawartości kontrolki ContentPlaceHolder na stronie wzorcowej, po prostu kliknij symbol taga inteligentnego (małą czarną strzałkę w prawym górnym rogu kontrolki) i wybierz opcję *domyślne do zawartości wzorca* z taga inteligentnego, jak pokazano na **rysunku 8**. Po wykonaniu tej czynności element menu zostanie zmieniony w celu *utworzenia zawartości niestandardowej*. Kliknięcie go w tym momencie spowoduje usunięcie zawartości z strony wzorcowej, co pozwala na zdefiniowanie zawartości niestandardowej dla tej konkretnej kontrolki zawartości.

![Ustawianie domyślnej kontrolki zawartości na stronie wzorcowej](master-pages/_static/image4.gif)

**Rysunek 7**. Ustawianie domyślnego formantu zawartości dla zawartości stron wzorcowych

## <a name="connecting-master-page-and-content-pages"></a>Łączenie strony wzorcowej i stron zawartości

Skojarzenie między stroną wzorcową a stroną zawartości można skonfigurować na jeden z czterech różnych sposobów:

- Atrybut <strong>MasterPageFile</strong> dyrektywy @Page
- Ustawianie właściwości **Page. MasterPageFile** w kodzie.
- **&lt;strony&gt;** elementu w pliku konfiguracji aplikacji (Web. config w folderze głównym aplikacji)
- **&lt;strony&gt;** elementu w podfolderach plik konfiguracyjny (Web. config w folderze)

## <a name="masterpagefile-attribute"></a>MasterPageFile — atrybut

Atrybut MasterPageFile ułatwia stosowanie strony wzorcowej do określonej strony ASP.NET. Jest to również Metoda używana do zastosowania strony wzorcowej po zaznaczeniu pola wyboru **Wybierz stronę wzorcową** , jak w ćwiczeniu 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Ustawianie strony. MasterPageFile w kodzie

Ustawiając właściwość MasterPageFile w kodzie, można zastosować określoną stronę wzorcową do zawartości w czasie wykonywania. Jest to przydatne w przypadkach, w których może być konieczne zastosowanie konkretnej strony wzorcowej na podstawie roli użytkownika lub innych kryteriów. Właściwość MasterPageFile musi być ustawiona w metodzie preinicjowania. Jeśli jest ustawiona po metodzie inicjowania, zostanie zgłoszony InvalidOperationException. Strona, na której jest ustawiana ta właściwość, musi mieć również kontrolkę zawartości jako formant najwyższego poziomu dla strony. W przeciwnym razie zostanie wygenerowany element HttpException, gdy ustawiona jest właściwość MasterPageFile.

## <a name="using-the-ltpagesgt-element"></a>Używanie &lt;stron&gt; elementu

Stronę wzorcową dla stron można skonfigurować przez ustawienie atrybutu masterPageFile na stronach &lt;&gt; elemencie pliku Web. config. Korzystając z tej metody, należy pamiętać, że pliki Web. config znajdujące się poniżej struktury aplikacji mogą zastąpić to ustawienie. Dowolny atrybut MasterPageFile ustawiony w dyrektywie @Page również przesłoni to ustawienie. Za pomocą &lt;stron&gt; element ułatwia utworzenie *głównej* strony wzorcowej, która może zostać przesłonięta w razie potrzeby w określonych folderach lub plikach.

## <a name="properties-in-master-pages"></a>Właściwości na stronach wzorcowych

Strona wzorcowa może uwidaczniać właściwości, po prostu czyniąc te właściwości publicznie na stronie głównej. Na przykład poniższy kod definiuje właściwość o nazwie SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Aby uzyskać dostęp do właściwości SomeProperty ze strony zawartości, należy użyć właściwości głównej, takiej jak:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Zagnieżdżanie stron wzorcowych

Strony wzorcowe są idealnym rozwiązaniem do zapewnienia wspólnego wyglądu i działania w ramach dużej aplikacji sieci Web. Nierzadko zdarza się jednak, że niektóre części dużej lokacji mają wspólny interfejs, podczas gdy inne części współużytkują inny interfejs. Aby rozwiązać ten problem, wiele stron głównych jest idealnym rozwiązaniem. Jednak nadal nie dotyczy faktu, że duża aplikacja może mieć pewne składniki (na przykład menu), które są współużytkowane przez wszystkie strony i inne składniki, które są współużytkowane tylko w niektórych sekcjach witryny. Dla tego typu sytuacji zagnieżdżone strony wzorcowe wypełniają potrzebną dobrze. Jak widać, normalna Strona wzorcowa składa się z strony wzorcowej i strony zawartości. W sytuacji zagnieżdżonej strony wzorcowej istnieją dwie strony główne: nadrzędny wzorzec i podrzędny element główny. Podrzędna Strona wzorcowa jest również stroną zawartości, a jej główną stroną wzorcową jest strona nadrzędna.

Oto kod typowej strony głównej:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

W zagnieżdżonym scenariuszu głównym będzie to nadrzędny wzorzec. Inna strona wzorcowa użyje tej strony jako strony głównej, a ten kod będzie wyglądać następująco:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Należy zauważyć, że w tym scenariuszu podrzędny wzorzec jest również stroną zawartości dla nadrzędnego serwera głównego. Cała zawartość nadrzędnego elementu głównego jest wyświetlana wewnątrz kontrolki zawartości, która pobiera jej zawartość z formantu ContentPlaceHolder elementu nadrzędnego.

> [!NOTE]
> Obsługa projektanta nie jest dostępna dla zagnieżdżonych stron wzorcowych. Podczas programowania przy użyciu zagnieżdżonych wzorców należy użyć widoku źródła.

Ten film wideo przedstawia przewodnik po użyciu zagnieżdżonych stron wzorcowych.

![](master-pages/_static/image1.png)

[Otwórz wideo pełnoekranowe](master-pages/_static/nested1.wmv)

![Wybieranie strony wzorcowej](master-pages/_static/image4.jpg)

**Ilustracja 8**. Wybieranie strony wzorcowej

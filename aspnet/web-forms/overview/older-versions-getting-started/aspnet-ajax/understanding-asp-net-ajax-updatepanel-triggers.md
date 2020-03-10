---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Informacje o wyzwalaczach UpdatePanel ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Podczas pracy w edytorze znaczników w programie Visual Studio możesz zauważyć (z IntelliSense), że istnieją dwa elementy podrzędne formantu UpdatePanel. Jeden z WH...
ms.author: riande
ms.date: 03/12/2008
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: b1cc869f373d4f8283b4d92af74707c3f11fef61
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78547454"
---
# <a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Objaśnienie wyzwalaczy UpdatePanel ASP.NET AJAX

przez [Scott Cate](https://github.com/scottcate)

[Pobierz plik PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Podczas pracy w edytorze znaczników w programie Visual Studio możesz zauważyć (z IntelliSense), że istnieją dwa elementy podrzędne formantu UpdatePanel. Jednym z nich jest element Triggers, który określa kontrolki na stronie (lub kontrolki użytkownika, jeśli są używane), które będą wyzwalać częściowe renderowanie formantu UpdatePanel, w którym znajduje się element.

## <a name="introduction"></a>Wprowadzenie

Technologia ASP.NET firmy Microsoft zapewnia model programowania zorientowany obiektowo i oparty na zdarzeniach oraz kieruje go do zalet skompilowanego kodu. Jednak model przetwarzania po stronie serwera ma kilka wad związanych z technologią, z których wiele może być rozwiązywana przez nowe funkcje zawarte w rozszerzeniach Microsoft ASP.NET AJAX 3,5. Te rozszerzenia umożliwiają korzystanie z wielu nowych zaawansowanych funkcji klienta, w tym częściowego renderowania stron, bez konieczności odświeżania pełnych stron, możliwość uzyskiwania dostępu do usług sieci Web za pośrednictwem skryptu klienta (w tym interfejsu API profilowania ASP.NET) i szerokiego interfejsu API po stronie klienta zaprojektowana do dublowania wielu schematów formantów widzianych w zestawie kontroli po stronie serwera ASP.NET.

Ten oficjalny dokument analizuje funkcje wyzwalaczy XML składnika ASP.NET `UpdatePanel` AJAX. Wyzwalacze XML zapewniają szczegółową kontrolę nad składnikami, które mogą powodować częściowe renderowanie dla określonych kontrolek UpdatePanel.

Niniejszy dokument jest oparty na wersji beta 2 .NET Framework 3,5 i Visual Studio 2008. Rozszerzenia AJAX ASP.NET, wcześniej zestaw dodatków, które są przeznaczone dla ASP.NET 2,0, są teraz zintegrowane z biblioteką klas podstawowych .NET Framework. W tym dokumencie przyjęto również założenie, że użytkownik pracuje z programem Visual Studio 2008, a nie z Visual Web Developer Express i zapewni wskazówki zgodne z interfejsem użytkownika programu Visual Studio (chociaż listy kodu będą całkowicie zgodne, niezależnie od środowisko programistyczne).

## <a name="triggers"></a>*Wyzwalacze*

Wyzwalacze dla danego elementu UpdatePanel domyślnie automatycznie zawierają wszystkie kontrolki podrzędne, które wywołują ogłaszanie zwrotne, w tym (na przykład) kontrolki TextBox, które mają właściwość `AutoPostBack` ustawioną na **wartość true**. Wyzwalacze mogą jednak być również uwzględniane deklaratywnie przy użyciu znaczników. jest to wykonywane w sekcji `<triggers>` deklaracji kontrolki UpdatePanel. Chociaż wyzwalacze są dostępne za pośrednictwem właściwości kolekcji `Triggers`, zaleca się zarejestrowanie wszelkich częściowych wyzwalaczy renderowania w czasie wykonywania (na przykład jeśli kontrolka nie jest dostępna w czasie projektowania) przy użyciu metody `RegisterAsyncPostBackControl(Control)` obiektu ScriptManager dla strony w ramach zdarzenia `Page_Load`. Pamiętaj, że strony są bezstanowe i dlatego należy je ponownie zarejestrować za każdym razem, gdy są tworzone.

Automatyczne dołączanie wyzwalacza podrzędnego może być również wyłączone (w związku z tym, że formanty podrzędne, które tworzą ogłaszanie zwrotne nie wyzwalają automatycznie renderowania częściowego), ustawiając właściwość `ChildrenAsTriggers` na **false**. Pozwala to na największą elastyczność przypisywania określonych kontrolek, które mogą wywoływać renderowanie strony, i jest zalecane, aby deweloper mógł odpowiedzieć na zdarzenie, zamiast obsługiwać wszystkie zdarzenia, które mogą wystąpić.

Należy pamiętać, że gdy kontrolki UpdatePanel są zagnieżdżone, gdy element UpdateMode ma wartość **Conditional**, w przypadku wyzwolenia podrzędnego elementu UpdatePanel zostanie odświeżony tylko podrzędny element UpdatePanel. Jeśli jednak nadrzędny element UpdatePanel zostanie odświeżony, element podrzędny UpdatePanel również zostanie odświeżony.

## <a name="the-lttriggersgt-element"></a>*&lt;wyzwalacze&gt; elementu*

Podczas pracy w edytorze znaczników w programie Visual Studio możesz zauważyć (z IntelliSense), że istnieją dwa elementy podrzędne formantu `UpdatePanel`. Najczęściej widzianym elementem jest element `<ContentTemplate>`, który zasadniczo hermetyzuje zawartość, która będzie przechowywana przez panel aktualizacji (zawartość, dla której włączane jest renderowanie częściowe). Drugi element to `<Triggers>` element, który określa kontrolki na stronie (lub kontrolki użytkownika, jeśli jest używany), które będą wyzwalać częściowe renderowanie formantu UpdatePanel, w którym znajduje się &lt;wyzwalacze&gt; elementu.

Element `<Triggers>` może zawierać dowolną liczbę każdego z dwóch węzłów podrzędnych: `<asp:AsyncPostBackTrigger>` i `<asp:PostBackTrigger>`. Oba te elementy akceptują dwa atrybuty, `ControlID` i `EventName`i mogą określać dowolną kontrolę w bieżącej jednostce hermetyzacji (na przykład jeśli formant UpdatePanel znajduje się w kontrolce użytkownika sieci Web, nie należy próbować odwołać się do kontrolki na stronie, w której znajduje się kontrolka użytkownika).

Element `<asp:AsyncPostBackTrigger>` jest szczególnie przydatny w odniesieniu do każdego zdarzenia z formantu, który istnieje jako element podrzędny *dowolnego* formantu UpdatePanel w jednostce hermetyzacji, a nie tylko elementu UpdatePanel, pod którym ten wyzwalacz jest elementem podrzędnym. W ten sposób można wykonać każdą kontrolkę, aby wyzwolić aktualizację strony częściowej.

Podobnie element `<asp:PostBackTrigger>` może służyć do wyzwalania częściowego renderowania strony, ale jeden, który wymaga pełnego przeprowadzenia rundy na serwerze. Tego elementu wyzwalacza można również użyć, aby wymusić renderowanie całej strony, gdy kontrolka w przeciwnym razie normalnie wyzwoli częściowe renderowanie strony (na przykład gdy kontrolka `Button` istnieje w `<ContentTemplate>` elemencie formantu UpdatePanel). Ponownie element PostBackTrigger może określić dowolny formant, który jest elementem podrzędnym dowolnego formantu UpdatePanel w bieżącej jednostce hermetyzacji.

## <a name="lttriggersgt-element-reference"></a>*Wyzwalacze &lt;&gt; odwołania do elementu*

*Elementy potomne znaczników:*

| **Tag** | **Opis** |
| --- | --- |
| &lt;ASP: AsyncPostBackTrigger&gt; | Określa kontrolkę i zdarzenie, które spowodują aktualizację strony częściowej dla elementu UpdatePanel, który zawiera odwołanie tego wyzwalacza. |
| &lt;ASP: PostBackTrigger&gt; | Określa kontrolkę i zdarzenie, które spowodują aktualizację pełnej strony (odświeżanie pełne strony). Ten tag może służyć do wymuszania pełnego odświeżenia, gdy kontrolka w przeciwnym razie wyzwolenie renderowania częściowego. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Przewodnik: wyzwalacze krzyżowe-UpdatePanel*

1. Utwórz nową stronę ASP.NET z obiektem ScriptManager ustawionym na włączenie renderowania częściowego. Dodaj dwa elementy UpdatePanel do tej strony — w pierwszej kolejności Dołącz kontrolkę etykieta (Label1) i dwie kontrolki przycisku (Button1 i Button2). Button1 powinien powiedzieć, że kliknij, aby zaktualizować zarówno, jak i Button2 powinien powiedzieć, że kliknij, aby zaktualizować ten element, lub coś obok tych wierszy. W drugim elemencie UpdatePanel Uwzględnij tylko kontrolkę etykieta (etykiety 2), ale ustaw jej Właściwość ForeColor na inną niż domyślna, aby odróżnić ją.
2. Ustaw właściwość UpdateMode obu tagów UpdatePanel na **Conditional**.

**Listing 1: Markup dla default. aspx:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. W programie obsługi zdarzeń kliknięcia dla Button1 ustaw wartość Label1. Text i etykiety 2. Text na coś zależnego od czasu (np. DateTime. Now. ToLongTimeString ()). Dla programu obsługi zdarzeń kliknięcia dla Button2 ustaw wartość w polu Label1. Text.

**Lista 2: CodeBehind (przycięta) w default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Naciśnij klawisz F5, aby skompilować i uruchomić projekt. Należy pamiętać, że po kliknięciu przycisku Aktualizuj oba panele obie etykiety zmieniają tekst; Jednak po kliknięciu przycisku Aktualizuj ten panel tylko aktualizacje Label1.

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))

## <a name="under-the-hood"></a>*Pod okapem*

Korzystając ze współtworzonego przykładu, możemy zajrzeć się na to, co robi ASP.NET AJAX W tym celu będziemy korzystać z wygenerowanego kodu HTML ze źródłem strony, a także rozszerzenia Mozilla Firefox o nazwie FireBug-with, ale będziemy mogli łatwo przeanalizować ogłaszanie zwrotne AJAX. Użyjemy również narzędzia reflektora .NET Lutz Roeder. Oba te narzędzia są swobodnie dostępne w trybie online i można je znaleźć za pomocą wyszukiwania internetowego.

Badanie kodu źródłowego strony pokazuje niemal poza normalnymi końcami; kontrolki UpdatePanel są renderowane jako kontenery `<div>` i zobaczysz, że zasób skryptu obejmuje dostarczone przez `<asp:ScriptManager>`. Istnieją także nowe wywołania AJAX specyficzne dla programu PageRequestManager, które są wewnętrzne dla biblioteki skryptów klienta AJAX. Na koniec widzimy dwa kontenery elementu UpdatePanel — jeden z renderowanymi `<input>`mi przyciskami z dwoma `<asp:Label>` kontrolkami renderowanymi jako kontenery `<span>`. (W przypadku inspekcji drzewa DOM w FireBug należy zauważyć, że etykiety są wygaszone, aby wskazać, że nie wytwarzają widocznej zawartości).

Kliknij przycisk Aktualizuj ten panel i zwróć uwagę na to, że górny element UpdatePanel zostanie zaktualizowany przy użyciu bieżącego czasu serwera. W FireBug, wybierz kartę konsoli, aby można było przeanalizować żądanie. Najpierw przejrzyj parametry żądania POST:

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))

Należy zauważyć, że element UpdatePanel został wskazany do kodu AJAX po stronie serwera, który dokładnie określa, że drzewo kontroli zostało wyzwolone za pośrednictwem parametru ScriptManager1: `Button1` kontrolki `UpdatePanel1`. Teraz kliknij przycisk Aktualizuj oba panele. Następnie podczas badania odpowiedzi zobaczymy serię zmiennych, które są rozdzielone potokami, w ciągu; w każdym przypadku zobaczymy najpopularniejszy element UpdatePanel, `UpdatePanel1`, zawiera całościowy kod HTML wysłany do przeglądarki. Biblioteka skryptów klienta AJAX zastępuje oryginalną zawartość HTML elementu UpdatePanel nową zawartością za pomocą właściwości `.innerHTML` i dlatego serwer wysyła zmienioną zawartość z serwera w formacie HTML.

Teraz kliknij przycisk Aktualizuj oba panele i sprawdź wyniki z serwera. Wyniki są bardzo podobne — oba elementy UpdatePanel otrzymują nowy kod HTML z serwera. Podobnie jak w przypadku poprzedniego wywołania zwrotnego jest wysyłany dodatkowy stan strony.

Jak widać, ponieważ nie jest używany żaden specjalny kod do przeprowadzania ogłaszania zwrotnego AJAX, biblioteka skryptów klienta AJAX jest w stanie przechwycić formularz ogłaszania zwrotnego bez dodatkowego kodu. Formanty serwera automatycznie wykorzystują język JavaScript, aby nie przesyłali automatycznie formularza-ASP.NET automatycznie wprowadza kod do walidacji i stanu formularza, głównie przez automatyczne dołączanie zasobów skryptu, klasy PostBackOptions i Klasa ClientScriptManager.

Na przykład rozważmy kontrolkę CheckBox; Badanie demontażu klasy w reflektorze platformy .NET. W tym celu upewnij się, że zestaw system. Web jest otwarty, i przejdź do klasy `System.Web.UI.WebControls.CheckBox`, otwierając metodę `RenderInputTag`. Poszukaj warunku sprawdzającego Właściwość `AutoPostBack`:

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))

Gdy automatyczne ogłaszanie zwrotne jest włączone w formancie `CheckBox` (za pośrednictwem właściwości autoogłaszania zwrotnego jest prawdziwe), wynikowy tag `<input>` jest renderowany ze skryptem obsługi zdarzeń ASP.NET w jego atrybucie `onclick`. Przechwycenie zgłoszenia formularza umożliwia nieinwazyjne doASP.NET AJAX do strony, pomagając uniknąć ewentualnych zmian, które mogą wystąpić, wykorzystując prawdopodobnie nieprecyzyjne zastępowanie ciągów. Ponadto umożliwia to *dowolnemu* niestandardowemu formantowi ASP.NET wykorzystanie mocy ASP.NET AJAX bez dodatkowego kodu do obsługi użycia w kontenerze UpdatePanel.

Funkcja `<triggers>` odpowiada wartościom inicjowanym w wywołaniu PageRequestManager na \_updateControls (należy zauważyć, że biblioteka skryptów klienta ASP.NET AJAX używa konwencji, które metody, zdarzenia i nazwy pól zaczynające się od podkreślenia są oznaczone jako wewnętrzne i nie są przeznaczone do użycia poza samą biblioteką). Z działem IT można obserwować, które kontrolki mają na celu spowodowanie ogłaszania zwrotnego AJAX.

Na przykład Dodajmy do strony dwie dodatkowe kontrolki, pozostawiając w całości kontrolki poza elementami UpdatePanel i pozostawiając je w elemencie UpdatePanel. Dodamy kontrolkę CheckBox w obrębie górnego elementu UpdatePanel i porzucasz DropDownList z liczbą kolorów zdefiniowaną na liście. Oto nowe oznakowanie:

**Lista 3: nowe oznakowanie**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

A oto nowy kod w tle:

**Lista 4: CodeBehind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

Pomysł związany z tą stroną polega na tym, że lista rozwijana wybiera jeden z trzech kolorów do wyświetlania drugiej etykiety, że pole wyboru określa, czy jest pogrubienie, oraz czy etykiety wyświetlają datę oraz godzinę. Pole wyboru nie powinno spowodować aktualizacji AJAX, ale lista rozwijana powinna być, nawet jeśli nie znajduje się w elemencie UpdatePanel.

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))

Jak widać na powyższym zrzucie ekranu, najnowszy przycisk, który zostanie kliknięty, był prawym przyciskiem myszy Aktualizuj ten panel, który zaktualizował czas pierwszego uruchomienia niezależny od czasu ostatniego. Data została również przełączona między kliknięciami, ponieważ data jest widoczna na dolnej etykiecie. Na koniec interesu jest kolor dolnej etykiety: został on zaktualizowany niedawno niż tekst etykiety, który pokazuje, że stan kontroli jest istotny, a użytkownicy oczekują, że zostaną zachowani przez ogłaszanie zwrotne AJAX. *Jednak*czas nie został zaktualizowany. Czas został automatycznie ponownie wypełniony przez trwałość pola stan \_\_wartość strony interpretowanej przez środowisko uruchomieniowe ASP.NET, gdy kontrolka była ponownie renderowana na serwerze. Kod serwera ASP.NET AJAX nie rozpoznaje metod, w których kontrolki zmieniają stan; po prostu ponownie wypełnia stan widoku, a następnie uruchamia odpowiednie zdarzenia.

Należy jednak pamiętać, że po zainicjowaniu czasu w trakcie zdarzenia ładowania strony\_czas został zwiększony poprawnie. W związku z tym deweloperzy powinni mieć ostrożność, że odpowiedni kod jest uruchamiany podczas odpowiednich programów obsługi zdarzeń, i unikaj stosowania\_ładowania strony, gdy program obsługi zdarzeń sterowania będzie odpowiedni.

## <a name="summary"></a>Podsumowanie

Formant UpdatePanel rozszerzeń ASP.NET AJAX jest uniwersalny i może korzystać z szeregu metod identyfikacji zdarzeń kontroli, które powinny spowodować ich aktualizację. Obsługuje on automatyczne aktualizowanie przez jego kontrolki podrzędne, ale może także reagować na zdarzenia kontroli w innym miejscu na stronie.

Aby zmniejszyć liczbę potencjalnych operacji ładowania serwera, zaleca się, aby Właściwość `ChildrenAsTriggers` elementu UpdatePanel była ustawiona na wartość `false`, a zdarzenia te są uwzględniane domyślnie. Zapobiega to również niepotrzebnym zdarzeniom spowodowanym potencjalnie niechcianymi skutkami, w tym walidacją i zmianami pól wejściowych. Te typy usterek mogą być trudne do odizolowania, ponieważ strona jest aktualizowana w sposób niewidoczny dla użytkownika i powód może nie być natychmiast oczywisty.

Badając wewnętrzne działania modelu przechwycenia w formie ASP.NET AJAX, mogliśmy określić, że wykorzystuje platformę już zapewnioną przez ASP.NET. W tym celu zachowuje ona maksymalną zgodność z kontrolkami zaprojektowanymi przy użyciu tej samej struktury i zapewnia intruzom niewielkie zastosowanie do dodatkowego kodu JavaScript napisanego na stronie.

## <a name="bio"></a>Materiał

Rob Paveza jest starszym deweloperem aplikacji .NET w firmie Terralever ([www.Terralever.com](http://www.terralever.com)), wiodącym interaktywnym przedsiębiorstwem marketingu w Tempe, AZ. Można go osiągnąć w [robpaveza@gmail.com](mailto:robpaveza@gmail.com), a jego blog znajduje się w [http://geekswithblogs.net/robp/](http://geekswithblogs.net/robp/).

Scott Cate pracował z technologiami sieci Web firmy Microsoft od 1997 i jest prezydentem myKB.com ([www.myKB.com](http://www.myKB.com)), w którym wyspecjalizowany jest pisanie aplikacji opartych na ASP.NET, które są zgodne z podstawowymi rozwiązaniami oprogramowania. W witrynie Scotta można skontaktować się z pocztą e-mail na [scott.cate@myKB.com](mailto:scott.cate@myKB.com) lub w blogu w witrynie [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Poprzednie](understanding-partial-page-updates-with-asp-net-ajax.md)
> [dalej](understanding-asp-net-ajax-authentication-and-profile-application-services.md)

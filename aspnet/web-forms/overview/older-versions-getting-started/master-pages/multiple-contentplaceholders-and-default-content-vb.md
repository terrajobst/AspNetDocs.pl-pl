---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
title: Wiele elementów ContentPlaceHolders i zawartości domyślnej (VB) | Microsoft Docs
author: rick-anderson
description: Sprawdza, jak dodać posiadaczy wielu miejsc zawartości do strony wzorcowej, a także jak określić domyślną zawartość w polu posiadacze miejsca zawartości.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 866a7177-6884-451e-88f4-c934b1dd1af5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
msc.type: authoredcontent
ms.openlocfilehash: b71cacb143094dcc5cf483c69c2fcc0f10def51c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74628486"
---
# <a name="multiple-contentplaceholders-and-default-content-vb"></a>Wiele kontrolek ContentPlaceHolder i zawartość domyślna (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_VB.pdf)

> Sprawdza, jak dodać posiadaczy wielu miejsc zawartości do strony wzorcowej, a także jak określić domyślną zawartość w polu posiadacze miejsca zawartości.

## <a name="introduction"></a>Wprowadzenie

W poprzednim samouczku sprawdziłmy, jak strony wzorcowe umożliwiają deweloperom ASP.NET tworzenie spójnego układu dla całej lokacji. Strony wzorcowe definiują oba znaczniki, które są wspólne dla wszystkich swoich stron zawartości i regionów, które można dostosować w zależności od strony. W poprzednim samouczku utworzyliśmy prostą stronę wzorcową (`Site.master`) i dwie strony zawartości (`Default.aspx` i `About.aspx`). Nasza Strona główna składa się z dwóch elementów ContentPlaceHolder o nazwie `head` i `MainContent`, które znajdują się odpowiednio w `<head>` element i formularz sieci Web. Gdy każda z nich ma dwie kontrolki zawartości, to tylko określone znaczniki dla danego elementu, które odpowiadają `MainContent`.

Zgodnie z dowodem dwóch formantów ContentPlaceHolder w `Site.master`, Strona wzorcowa może zawierać wiele elementów ContentPlaceHolder. Co więcej, Strona wzorcowa może określić domyślne znaczniki dla formantów ContentPlaceHolder. Na stronie zawartości można opcjonalnie określić własne znaczniki lub użyć znaczników domyślnych. W tym samouczku Przyjrzyjmy się użyciu wielu kontrolek zawartości na stronie wzorcowej i zobacz, jak definiować znaczniki domyślne w kontrolkach ContentPlaceHolder.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Krok 1. Dodawanie dodatkowych kontrolek ContentPlaceHolder do strony wzorcowej

Wiele projektów witryn sieci Web zawiera kilka obszarów na ekranie, które są dostosowane do poszczególnych stron. `Site.master`, Strona wzorcowa utworzona w poprzednim samouczku zawiera pojedynczy element ContentPlaceHolder w formularzu sieci Web o nazwie `MainContent`. W odróżnieniu od tego element ContentPlaceHolder znajduje się w elemencie `mainContent` `<div>`.

Rysunek 1 przedstawia `Default.aspx`, gdy jest wyświetlany za pomocą przeglądarki. Region w kolorze czerwonym jest znacznikiem specyficznym dla strony odpowiadającym `MainContent`.

[![region w kółku pokazuje obszar aktualnie dostosowywany na stronie](multiple-contentplaceholders-and-default-content-vb/_static/image2.png)](multiple-contentplaceholders-and-default-content-vb/_static/image1.png)

**Ilustracja 01**: region w kółku pokazuje obszar aktualnie dostosowywany na stronie ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image3.png))

Załóżmy, że oprócz regionu pokazanego na rysunku 1, należy również dodać elementy specyficzne dla strony do lewej kolumny poniżej sekcji lekcje i wiadomości. Aby to osiągnąć, Dodaj kolejną kontrolkę ContentPlaceHolder do strony wzorcowej. Aby wykonać te czynności, Otwórz stronę wzorcową `Site.master` w programie Visual Web Developer, a następnie przeciągnij formant ContentPlaceHolder z przybornika do projektanta po sekcji wiadomości. Ustaw `ID` elementu ContentPlaceHolder na `LeftColumnContent`.

[![dodać kontrolki ContentPlaceHolder do lewej kolumny strony wzorcowej](multiple-contentplaceholders-and-default-content-vb/_static/image5.png)](multiple-contentplaceholders-and-default-content-vb/_static/image4.png)

**Ilustracja 02**. Dodawanie kontrolki ContentPlaceHolder do lewej kolumny strony wzorcowej ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image6.png))

Po dodaniu elementu `LeftColumnContent` ContentPlaceHolder do strony głównej można zdefiniować zawartość dla tego regionu na podstawie strony, dołączając kontrolkę zawartość na stronie, której `ContentPlaceHolderID` jest ustawiona na `LeftColumnContent`. Analizujemy ten proces w kroku 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Krok 2. Definiowanie zawartości dla nowego elementu ContentPlaceHolder na stronach zawartości

W przypadku dodawania nowej strony zawartości do witryny sieci Web deweloper internetowy automatycznie tworzy kontrolkę zawartości na stronie dla każdego elementu ContentPlaceHolder na wybranej stronie wzorcowej. Po dodaniu `LeftColumnContent` elementu ContentPlaceHolder do naszej strony głównej w kroku 1 nowe strony ASP.NET będą miały teraz trzy formanty zawartości.

Aby to zilustrować, Dodaj nową stronę zawartości do katalogu głównego o nazwie `MultipleContentPlaceHolders.aspx`, która jest powiązana z `Site.master` stroną wzorcową. Visual Web Developer tworzy Tę stronę przy użyciu następujących znaczników deklaratywnych:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample1.aspx)]

Wprowadź zawartość do kontrolki zawartości odwołującej się do `MainContent` ContentPlaceHolders (`Content2`). Następnie Dodaj następujący znacznik do kontrolki zawartości `Content3` (która odwołuje się do `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample2.html)]

Po dodaniu tego znacznika odwiedź stronę za pomocą przeglądarki. Jak pokazano na rysunku 3, znacznik umieszczony w `Content3` formancie zawartości zostanie wyświetlony w lewej kolumnie poniżej sekcji wiadomości (w kółku na czerwoną). Znacznik umieszczony w `Content2` jest wyświetlany w prawej części strony (koło w kolorze niebieskim).

[![lewa kolumna zawiera teraz zawartość specyficzną dla strony poniżej sekcji wiadomości](multiple-contentplaceholders-and-default-content-vb/_static/image8.png)](multiple-contentplaceholders-and-default-content-vb/_static/image7.png)

**Ilustracja 03**: lewa kolumna zawiera teraz zawartość specyficzną dla strony poniżej sekcji wiadomości ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image9.png))

### <a name="defining-content-in-existing-content-pages"></a>Definiowanie zawartości na istniejących stronach zawartości

Utworzenie nowej strony zawartości automatycznie zawiera formant ContentPlaceHolder dodany w kroku 1. Jednak nasze dwie istniejące strony zawartości — `About.aspx` i `Default.aspx` — nie mają kontroli zawartości dla `LeftColumnContent` ContentPlaceHolder. Aby określić zawartość dla tego elementu ContentPlaceHolder na tych dwóch istniejących stronach, musimy dodać wypróbujemy kontrolki zawartości.

W przeciwieństwie do większości ASP.NETych formantów sieci Web, Przybornik Visual Web Developer nie obejmuje elementu kontroli zawartości. Możemy ręcznie wpisywać znaczniki deklaratywne kontrolki zawartości do widoku źródła, ale łatwiejszym i szybszym podejściem jest użycie widok Projekt. Otwórz stronę `About.aspx` i przejdź do widok Projekt. Jak pokazano na rysunku 4, `LeftColumnContent` ContentPlaceHolder pojawia się w widok Projekt; Po umieszczeniu nad nim wskaźnika myszy wyświetlany jest napis "LeftColumnContent (Master)". Włączenie elementu "Master" w tytule oznacza, że na stronie tego elementu ContentPlaceHolder nie zdefiniowano żadnej kontrolki zawartości. Jeśli istnieje formant zawartości dla elementu ContentPlaceHolder, jak w przypadku `MainContent`, tytuł zostanie odczytany: "*ContentPlaceHolderID* (Custom)".

Aby dodać kontrolkę zawartości dla `LeftColumnContent` ContentPlaceHolder do `About.aspx`, rozwiń tag inteligentny "ContentPlaceHolder" i kliknij link Utwórz zawartość niestandardową.

[![widok projektu dla elementu about. aspx zawiera LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image11.png)](multiple-contentplaceholders-and-default-content-vb/_static/image10.png)

**Ilustracja 04**: widok projektu dla `About.aspx` pokazuje `LeftColumnContent` ContentPlaceHolder (kliknij,[Aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image12.png))

Kliknięcie linku Utwórz zawartość niestandardową generuje niezbędną kontrolkę zawartości na stronie i ustawia jej Właściwość `ContentPlaceHolderID` na `ID`elementu ContentPlaceHolder. Na przykład kliknięcie linku Utwórz zawartość niestandardową dla `LeftColumnContent` regionu w `About.aspx` dodaje do strony następujący znacznik deklaratywny:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>Pomijanie kontrolek zawartości

ASP.NET nie wymaga, aby wszystkie strony zawartości obejmowały kontrolki zawartości dla każdego i każdego elementu ContentPlaceHolder zdefiniowanego na stronie wzorcowej. Jeśli formant zawartości zostanie pominięty, aparat ASP.NET używa znacznika zdefiniowanego w elemencie ContentPlaceHolder na stronie wzorcowej. Ten znacznik jest określany jako *Domyślna zawartość* elementu ContentPlaceHolder i jest użyteczny w scenariuszach, w których zawartość dla pewnego regionu jest wspólna między większością stron, ale należy ją dostosować w przypadku niewielkiej liczby stron. Krok 3. Eksplorowanie określania zawartości domyślnej na stronie wzorcowej.

Obecnie `Default.aspx` zawiera dwie kontrolki zawartości dla `head` i `MainContent` elementów ContentPlaceHolders; nie ma kontrolki zawartości dla `LeftColumnContent`. W związku z tym, gdy `Default.aspx` jest renderowany, zostanie użyta domyślna zawartość elementu ContentPlaceHolder `LeftColumnContent`. Ponieważ jeszcze nie zdefiniowano żadnej domyślnej zawartości dla tego elementu ContentPlaceHolder, efektem netto jest to, że żadne znaczniki nie są emitowane dla tego regionu. Aby sprawdzić to zachowanie, odwiedź stronę `Default.aspx` za pomocą przeglądarki. Jak pokazano na rysunku 5, żadne znaczniki nie są emitowane w lewej kolumnie poniżej sekcji wiadomości.

[![nie jest renderowana żadna zawartość dla LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image14.png)](multiple-contentplaceholders-and-default-content-vb/_static/image13.png)

**Ilustracja 05**: nie jest renderowana żadna zawartość dla `LeftColumnContent` ContentPlaceHolder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image15.png))

## <a name="step-3-specifying-default-content-in-the-master-page"></a>Krok 3. Określanie zawartości domyślnej na stronie wzorcowej

Niektóre projekty witryny sieci Web obejmują region, którego zawartość jest taka sama dla wszystkich stron w lokacji, z wyjątkiem jednego lub dwóch wyjątków. Weź pod uwagę witrynę sieci Web, która obsługuje konta użytkowników. Ta lokacja wymaga strony logowania, na której osoby odwiedzające mogą wprowadzić swoje poświadczenia, aby zalogować się do witryny. Aby przyspieszyć proces logowania, Projektant witryny sieci Web może zawierać pola tekstowe username i Password w lewym górnym rogu każdej strony, aby umożliwić użytkownikom logowanie się bez konieczności jawnego odwiedzenia strony logowania. Chociaż te pola tekstowe nazwy użytkownika i hasła są przydatne na większości stron, są nadmiarowe na stronie logowania, która zawiera już pola tekstowe dla poświadczeń użytkownika.

Aby zaimplementować ten projekt, można utworzyć formant ContentPlaceHolder w lewym górnym rogu strony wzorcowej. Każda Strona, która musi wyświetlić pola tekstowe nazwy użytkownika i hasła w lewym górnym rogu, utworzy kontrolkę zawartości dla tego elementu ContentPlaceHolder i doda wymagany interfejs. Na stronie logowania można pominąć Dodawanie kontrolki zawartości dla tego elementu ContentPlaceHolder lub utworzyć kontrolkę zawartości bez zdefiniowanego znacznika. Minusem tego podejścia polega na tym, że należy pamiętać, aby dodać pola tekstowe username i Password do każdej strony, którą dodamy do witryny (z wyjątkiem strony logowania). Jest to prośba o problemy. Najprawdopodobniej zapomnimy dodać te pola tekstowe do strony lub dwóch lub, co jest bardziej niekorzystniej, możemy nie zaimplementować poprawnie interfejsu (może to spowodować dodanie tylko jednego pola tekstowego zamiast dwóch).

Lepszym rozwiązaniem jest zdefiniowanie pól tekstowych username i Password jako zawartości domyślnej elementu ContentPlaceHolder. Dzięki temu wystarczy przesłonić tę zawartość domyślną na kilku stronach, które nie wyświetlają pól tekstowych Nazwa użytkownika i hasło (na przykład strona logowania. Aby zilustrować Określanie zawartości domyślnej dla kontrolki ContentPlaceHolder, zaimplementujmy scenariusz opisany poniżej.

> [!NOTE]
> W pozostałej części tego samouczka zostanie zaktualizowana witryna sieci Web w celu uwzględnienia interfejsu logowania w lewej kolumnie dla wszystkich stron, ale na stronie logowania. Ten samouczek nie zawiera jednak sposobu konfigurowania witryny sieci Web do obsługi kont użytkowników. Aby uzyskać więcej informacji na temat tego tematu, zobacz moje samouczki [uwierzytelnianie, autoryzacja, konta użytkowników i role](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) .

### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Dodawanie elementu ContentPlaceHolder i określanie jego zawartości domyślnej

Otwórz stronę wzorcową `Site.master` i Dodaj następujące znaczniki do lewej kolumny między sekcją `DateDisplay` etykiety i lekcji:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample4.aspx)]

Po dodaniu tego znacznika widok Projekt strony głównej powinna wyglądać podobnie do rysunku 6.

[![Strona wzorcowa zawiera kontrolkę logowania](multiple-contentplaceholders-and-default-content-vb/_static/image17.png)](multiple-contentplaceholders-and-default-content-vb/_static/image16.png)

**Ilustracja 06**. Strona wzorcowa zawiera kontrolkę logowania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image18.png))

Ten element ContentPlaceHolder `QuickLoginUI`ma kontrolkę sieci Web logowania jako domyślną zawartość. Kontrolka logowania wyświetla interfejs użytkownika, który będzie monitował użytkownika o nazwę użytkownika i hasło oraz przycisk Zaloguj. Po kliknięciu przycisku Zaloguj, kontrola logowania wewnętrznie weryfikuje poświadczenia użytkownika względem interfejsu API członkostwa. Aby użyć tej kontrolki logowania w programie, należy skonfigurować witrynę do korzystania z członkostwa. Ten temat wykracza poza zakres tego samouczka; Aby uzyskać więcej informacji na temat tworzenia aplikacji sieci Web, która obsługuje konta użytkowników, zobacz moje samouczki [uwierzytelnianie, autoryzacja, konta użytkowników i role](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) .

Możesz dostosowywać zachowanie lub wygląd kontrolki logowania. Mam ustawioną dwie właściwości: `TitleText` i `FailureAction`. Wartość właściwości `TitleText`, która domyślnie jest "log in", jest wyświetlana u góry interfejsu użytkownika formantu. Mam ustawioną tę właściwość, aby wyświetlić tekst "Zaloguj się" jako element `<h3>`. Właściwość `FailureAction` wskazuje, co należy zrobić, jeśli poświadczenia użytkownika są nieprawidłowe. Domyślnie jest to wartość `Refresh`, która pozostawia użytkownika na tej samej stronie i wyświetla komunikat o niepowodzeniu w obrębie formantu logowania. Został zmieniony na `RedirectToLoginPage`, co spowoduje wysłanie użytkownika do strony logowania w przypadku nieprawidłowych poświadczeń. Wolisz wysłać użytkownika do strony logowania, gdy użytkownik próbuje zalogować się z innej strony, ale kończy się niepowodzeniem, ponieważ strona logowania może zawierać dodatkowe instrukcje i opcje, które nie mogą być łatwo dopasowane do lewej kolumny. Na przykład strona logowania może zawierać opcje umożliwiające pobranie zapomnianego hasła lub utworzenie nowego konta.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Tworzenie strony logowania i zastępowanie zawartości domyślnej

Po zakończeniu pracy ze stroną wzorcową następnym krokiem jest utworzenie strony logowania. Dodaj stronę ASP.NET do katalogu głównego witryny o nazwie `Login.aspx`, a następnie powiąż ją ze stroną wzorcową `Site.master`. Spowoduje to utworzenie strony z czterema kontrolkami zawartości, jeden dla każdego elementu ContentPlaceHolders zdefiniowanego w `Site.master`.

Dodaj kontrolkę login do kontrolki zawartości `MainContent`. Ponadto możesz bezpłatnie dodać dowolną zawartość do regionu `LeftColumnContent`. Należy jednak pamiętać o pozostawieniu formantu zawartości dla `QuickLoginUI` ContentPlaceHolder puste. Dzięki temu formant logowania nie zostanie wyświetlony w lewej kolumnie strony logowania.

Po zdefiniowaniu zawartości dla regionów `MainContent` i `LeftColumnContent`, znaczniki deklaratywne strony logowania powinny wyglądać podobnie do następujących:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample5.aspx)]

Rysunek 7 przedstawia Tę stronę, gdy oglądasz za pomocą przeglądarki. Ponieważ ta strona określa kontrolkę zawartości dla `QuickLoginUI` ContentPlaceHolder, zastępuje domyślną zawartość określoną na stronie wzorcowej. Efektem netto jest to, że kontrolka logowania wyświetlana na widok Projekt stronie wzorcowej (patrz rysunek 6) nie jest renderowana na tej stronie.

[![na stronie logowania zostanie wyciśnięty QuickLoginUI domyślna zawartość elementu ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image20.png)](multiple-contentplaceholders-and-default-content-vb/_static/image19.png)

**Ilustracja 07**: Strona logowania umożliwia ponowne naciśnięcie `QuickLoginUI` domyślnej zawartości elementu ContentPlaceHolder ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image21.png))

### <a name="using-the-default-content-in-new-pages"></a>Używanie zawartości domyślnej na nowych stronach

Chcemy wyświetlić formant logowania w lewej kolumnie dla wszystkich stron oprócz strony logowania. Aby to osiągnąć, wszystkie strony zawartości z wyjątkiem strony logowania powinny pominąć kontrolkę zawartości dla `QuickLoginUI` ContentPlaceHolder. Pomijając kontrolkę zawartości, zamiast niej zostanie użyta domyślna zawartość elementu ContentPlaceHolder.

Nasze istniejące strony zawartości — `Default.aspx`, `About.aspx`i `MultipleContentPlaceHolders.aspx` — nie Uwzględniaj kontrolki zawartości dla `QuickLoginUI`, ponieważ zostały one utworzone przed dodaniem tej kontrolki ContentPlaceHolder do strony wzorcowej. W związku z tym istniejące strony nie wymagają aktualizacji. Jednak nowe strony dodane do witryny sieci Web domyślnie zawierają kontrolkę zawartości dla `QuickLoginUI` ContentPlaceHolder. W związku z tym należy pamiętać, aby usunąć te kontrolki zawartości przy każdym dodawaniu nowej strony zawartości (chyba że chcemy zastąpić domyślną zawartość elementu ContentPlaceHolder, tak jak w przypadku strony logowania).

Aby usunąć kontrolkę zawartości, można ręcznie usunąć jej Tagi deklaracyjne z widoku źródła lub z widok Projekt wybrać łącze do zawartości wzorca domyślnego z jego tagu inteligentnego. Jedno z metod usuwa kontrolkę zawartości ze strony i daje ten sam efekt netto.

Rysunek 8 przedstawia `Default.aspx`, gdy jest wyświetlany za pomocą przeglądarki. Funkcja odwoływania, która `Default.aspx` ma tylko dwie kontrolki zawartości określone w jego deklaratywnym znaczniku — jeden dla `head` i jeden dla `MainContent`. W efekcie zostanie wyświetlona domyślna zawartość dla `LeftColumnContent` i `QuickLoginUI` Elementy ContentPlaceHolders.

[![zostanie wyświetlona domyślna zawartość dla elementów ContentPlaceHolder LeftColumnContent i QuickLoginUI](multiple-contentplaceholders-and-default-content-vb/_static/image23.png)](multiple-contentplaceholders-and-default-content-vb/_static/image22.png)

**Ilustracja 08**: zostanie wyświetlona domyślna zawartość dla `LeftColumnContent` i `QuickLoginUI` Elementy ContentPlaceHolders ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](multiple-contentplaceholders-and-default-content-vb/_static/image24.png))

## <a name="summary"></a>Podsumowanie

Model strony głównej ASP.NET umożliwia użycie dowolnej liczby elementów ContentPlaceHolder na stronie wzorcowej. Co więcej, Elementy ContentPlaceHolders zawierają zawartość domyślną, która jest emitowana w przypadku braku odpowiedniej kontroli zawartości na stronie zawartości. W tym samouczku przedstawiono sposób dołączania dodatkowych kontrolek ContentPlaceHolder na stronie wzorcowej i definiowania kontrolek zawartości dla tych nowych elementów ContentPlaceHolder na nowych i istniejących stronach ASP.NET. Podano również, jak określamy domyślną zawartość w elemencie ContentPlaceHolder, co jest przydatne w scenariuszach, w których tylko niewielka liczba stron musi dostosowywać zawartość znormalizowaną w inny sposób w określonym regionie.

W następnym samouczku sprawdzimy `head` element ContentPlaceHolder w bardziej szczegółowy sposób, ale widzisz sposób deklaratywny i programistyczny definiowania tytułu, tagów Meta i innych nagłówków HTML na zasadzie strony.

Szczęśliwe programowanie!

### <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 3,5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott można uzyskać w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został suchi Banerjee. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](creating-a-site-wide-layout-using-master-pages-vb.md)
> [dalej](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)

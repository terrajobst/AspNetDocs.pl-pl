---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
title: Adresy URL na stronach wzorcowych (VB) | Microsoft Docs
author: rick-anderson
description: Adresy URL na stronie wzorcowej mogą być przerwane ze względu na plik strony głównej znajdujący się w innym katalogu względnym niż strona zawartości. Trwa zmienianie bazy...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: 43d1e83c-0092-4dcf-977c-e709c4dce7c3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 01627988f68bb619969a5fe3cfaae68fe70b5d4f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548112"
---
# <a name="urls-in-master-pages-vb"></a>Adresy URL na stronach wzorcowych (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_VB.pdf)

> Adresy URL na stronie wzorcowej mogą być przerwane ze względu na plik strony głównej znajdujący się w innym katalogu względnym niż strona zawartości. Przegląda adresy URL w ramach składni deklaratywnej i programowo przy użyciu ResolveUrl i ResolveClientUrl. (Sprawdź również

## <a name="introduction"></a>Wprowadzenie

We wszystkich przykładach, które zostały tu przedstawione, strony główne i zawartości znajdują się w tym samym folderze (folderze głównym witryny sieci Web). Nie istnieje jednak powód, dla którego wzorce i strony zawartości muszą znajdować się w tym samym folderze. Można na pewno tworzyć strony zawartości w podfolderach. Podobnie można utworzyć folder `~/MasterPages/`, w którym będą umieszczane strony wzorcowe witryny.

Jednym z potencjalnych problemów dotyczących umieszczania stron głównych i zawartości w różnych folderach jest uszkodzenie adresów URL. Jeśli strona wzorcowa zawiera względne adresy URL w hiperłączach, obrazach lub innych elementach, link będzie nieprawidłowy dla stron zawartości znajdujących się w innym folderze. W tym samouczku sprawdzimy źródło tego problemu oraz obejścia problemu.

## <a name="the-problem-with-relative-urls"></a>Problem z względnymi adresami URL

Adres URL na stronie sieci Web jest określany jako *względny adres URL* , jeśli lokalizacja zasobu, na który wskazuje, jest określana względem lokalizacji strony sieci Web w strukturze folderów witryny internetowej. Każdy adres URL, który nie zaczyna się od wiodącego ukośnika (`/`) lub protokołu (na przykład `http://`) jest względny, ponieważ jest rozpoznawany przez przeglądarkę w oparciu o lokalizację strony sieci Web, która zawiera adres URL.

Na przykład nasza witryna sieci Web ma folder `~/Images/` z pojedynczym plikiem obrazu, `PoweredByASPNET.gif`. Plik strony głównej `Site.master` ma element `<img>` w regionie `footerContent` z następującą adiustacją:

[!code-html[Main](urls-in-master-pages-vb/samples/sample1.html)]

Wartość atrybutu `src` w elemencie `<img>` jest względnym adresem URL, ponieważ nie zaczyna się od `/` lub `http://`. W skrócie wartość atrybutu `src` nakazuje przeglądarce wyszukać w podfolderze `Images` pliku o nazwie `PoweredByASPNET.gif`.

Podczas odwiedzania strony zawartości powyższe znaczniki są wysyłane bezpośrednio do przeglądarki. Poświęć chwilę na odwiedzenie `About.aspx` i wyświetlenie źródła HTML wysłanego do przeglądarki. Zobaczysz, że do przeglądarki zostało wysłane dokładne oznakowanie na stronie wzorcowej.

[!code-html[Main](urls-in-master-pages-vb/samples/sample2.html)]

Jeśli strona zawartości znajduje się w folderze głównym (w `About.aspx`), wszystko działa zgodnie z oczekiwaniami, ponieważ istnieje `Images` podfolder względem folderu głównego. Jednakże, jeśli strona zawartości znajduje się w innym folderze niż strona wzorcowa. Aby to zilustrować, utwórz podfolder o nazwie `Admin`. Następnie Dodaj stronę zawartości o nazwie `Default.aspx` do folderu `Admin`, aby upewnić się, że nowa strona została powiązana z `Site.master` stroną wzorcową.

> [!NOTE]
> W samouczku [*Określanie tytułu, Meta tagów i innych nagłówków HTML na stronie wzorcowej*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) utworzono niestandardową klasę strony podstawowej o nazwie `BasePage`, która automatycznie ustawia tytuł strony zawartości (jeśli nie został jawnie przypisany). Nie zapomnij, aby nowo utworzona strona z kodem w kodzie była pochodną `BasePage`, aby można było korzystać z tej funkcji.

Po utworzeniu strony zawartości Eksplorator rozwiązań powinna wyglądać podobnie do rysunku 1.

![Nowy folder i strona ASP.NET zostały dodane do projektu](urls-in-master-pages-vb/_static/image1.png)

**Ilustracja 01**: nowy folder i strona ASP.NET zostały dodane do projektu

Następnie zaktualizuj plik `Web.sitemap`, aby zawierał nowy wpis `<siteMapNode>` dla tej lekcji. Poniższy kod XML przedstawia kompletny znacznik `Web.sitemap`, który obejmuje teraz dodanie trzeciego `<siteMapNode>` elementu.

[!code-xml[Main](urls-in-master-pages-vb/samples/sample3.xml)]

Nowo utworzona strona `Default.aspx` powinna mieć cztery kontrolki zawartości odpowiadające czterema elementom ContentPlaceHolders w `Site.master`. Dodaj tekst do kontrolki zawartości odwołującej się do `MainContent` ContentPlaceHolder, a następnie odwiedź stronę za pomocą przeglądarki. Jak pokazano na rysunku 2, przeglądarka nie może odnaleźć pliku obrazu `PoweredByASPNET.gif`. Co się dzieje tutaj?

Strona zawartości `~/Admin/Default.aspx` jest wysyłana do tego samego kodu HTML dla regionu `footerContent`, jako strona `About.aspx`:

[!code-html[Main](urls-in-master-pages-vb/samples/sample4.html)]

Ponieważ atrybut `src` elementu `<img>` jest względnym adresem URL, przeglądarka próbuje wyszukać folder `Images` względem lokalizacji folderu strony sieci Web. Inaczej mówiąc, przeglądarka szuka pliku obrazu `Admin/Images/PoweredByASPNET.gif`.

[![nie można odnaleźć pliku obrazu PoweredByASPNET. gif](urls-in-master-pages-vb/_static/image3.png)](urls-in-master-pages-vb/_static/image2.png)

**Ilustracja 02**: nie można odnaleźć pliku obrazu `PoweredByASPNET.gif` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](urls-in-master-pages-vb/_static/image4.png))

### <a name="replacing-relative-urls-with-absolute-urls"></a>Zamienianie względnych adresów URL na bezwzględne adresy URL

Przeciwieństwem względnego adresu URL jest *bezwzględny adres URL*, który rozpoczyna się od ukośnika (`/`) lub protokołu, takiego jak `http://`. Ponieważ bezwzględny adres URL określa lokalizację zasobu ze znanego stałego punktu, ten sam bezwzględny adres URL jest prawidłowy na dowolnej stronie sieci Web, niezależnie od lokalizacji strony sieci Web w strukturze folderów witryny internetowej.

Aby naprawić uszkodzony obraz przedstawiony na rysunku 2, musimy zaktualizować atrybut `src` elementu `<img>`, tak aby używał bezwzględnego adresu URL, a nie względem siebie. Aby określić poprawny bezwzględny adres URL, odwiedź stronę sieci Web w witrynie internetowej i sprawdź pasek adresu. Jak pokazano na rysunku 2 pasek adresu, w pełni kwalifikowana ścieżka do aplikacji sieci Web jest `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/`. W związku z tym możemy zaktualizować atrybut `src` elementu `<img>` do jednego z następujących dwóch bezwzględnych adresów URL:

- `/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`

Poświęć chwilę na zaktualizowanie atrybutu `src` elementu `<img>` do bezwzględnego adresu URL przy użyciu jednego z pokazanych powyżej formularzy, a następnie odwiedź stronę `~/Admin/Default.aspx` za pośrednictwem przeglądarki. Tym razem przeglądarka będzie poprawnie znajdować i wyświetlać plik obrazu `PoweredByASPNET.gif` (zobacz rysunek 3).

[![teraz zostanie wyświetlony obraz PoweredByASPNET. gif](urls-in-master-pages-vb/_static/image6.png)](urls-in-master-pages-vb/_static/image5.png)

**Ilustracja 03**: obraz `PoweredByASPNET.gif` jest teraz wyświetlany ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](urls-in-master-pages-vb/_static/image7.png))

Podczas gdy silne kodowanie w bezwzględnym adresie URL działa prawidłowo, Couples kod HTML do lokalizacji serwera i folderu witryny sieci Web, co może się zmienić. Używanie bezwzględnego adresu URL formularza `http://localhost:3908/...` jest kruchy, ponieważ numer portu poprzedzający localhost jest wybierany automatycznie za każdym razem, gdy zostanie uruchomiony program Visual Studio ASP.NET Development Web Server. Podobnie składnik `http://localhost` jest prawidłowy tylko podczas testowania lokalnego. Po wdrożeniu kodu na serwerze produkcyjnym baza adresów URL zmieni się na coś innego, na przykład `http://www.yourserver.com`. Bezwzględny adres URL w formularzu `/ASPNET_MasterPages_Tutorial_04_VB/...` również cierpi z tego samego brittleness, ponieważ ta ścieżka aplikacji różni się od serwerów deweloperskich i produkcyjnych.

Dobrymi wiadomościami jest to, że ASP.NET oferuje metodę generowania prawidłowego względnego adresu URL w czasie wykonywania.

## <a name="usingandresolveclienturl"></a>Używanie`~`i`ResolveClientUrl`

Zamiast naliczać bezwzględnie absolutny kod URL, ASP.NET umożliwia deweloperom stron korzystanie z tyldy (`~`), aby wskazać katalog główny aplikacji sieci Web. Na przykład we wcześniejszej części tego samouczka użyto notacji `~/Admin/Default.aspx` w tekście, aby odwołać się do strony `Default.aspx` w folderze `Admin`. `~` wskazuje, że folder `Admin` jest podfolderem katalogu głównego aplikacji sieci Web.

[Metoda`ResolveClientUrl`](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) klasy `Control` przyjmuje adres URL i modyfikuje go na WZGLĘDNY adres URL odpowiedni dla strony sieci Web, na której znajduje się formant. Na przykład wywoływanie `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` z `About.aspx` zwraca `Images/PoweredByASPNET.gif`. Wywołanie go z `~/Admin/Default.aspx`, jednak zwraca `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Ponieważ wszystkie formanty serwera ASP.NET pochodzą z klasy `Control`, wszystkie formanty serwera mają dostęp do metody `ResolveClientUrl`. Nawet Klasa `Page` dziedziczy z klasy `Control`, co oznacza, że można użyć tej metody bezpośrednio ze klas ASP.NETych w kodzie.

### <a name="usingin-the-declarative-markup"></a>Używanie`~`w znacznikach deklaratywnych

Kilka kontrolek sieci Web ASP.NET zawiera właściwości powiązane z adresem URL: formant HyperLink ma właściwość `NavigateUrl`; Kontrolka obrazu ma właściwość `ImageUrl`; i tak dalej. Po wyrenderowaniu te kontrolki przekazują swoje wartości właściwości powiązane z adresem URL do `ResolveClientUrl`. W związku z tym, jeśli te właściwości zawierają `~`, aby wskazać katalog główny aplikacji sieci Web, adres URL zostanie zmodyfikowany na prawidłowy względny adres URL.

Należy pamiętać, że tylko kontrolki serwera ASP.NET przekształcają `~` we właściwościach związanych z adresem URL. Jeśli `~` pojawia się w statycznej tabeli HTML, takiej jak `<img src="~/Images/PoweredByASPNET.gif" />`, aparat ASP.NET wysyła `~` do przeglądarki wraz z resztą zawartości HTML. W przeglądarce założono, że `~` jest częścią adresu URL. Na przykład jeśli przeglądarka otrzymuje znacznik, `<img src="~/Images/PoweredByASPNET.gif" />` założono, że istnieje podfolder o nazwie `~` z podfolderem `Images`, który zawiera `PoweredByASPNET.gif`pliku obrazu.

Aby naprawić znacznik obrazu w `Site.master`, Zastąp istniejący element `<img>` elementem kontrolki sieci Web obrazu ASP.NET. Ustaw `ID` kontrolki sieci Web obrazu na `PoweredByImage`, jej Właściwość `ImageUrl` na `~/Images/PoweredByASPNET.gif`i Właściwość `AlternateText` na "obsługiwane przez ASP.NET!"

[!code-aspx[Main](urls-in-master-pages-vb/samples/sample5.aspx)]

Po wprowadzeniu tej zmiany na stronie wzorcowej ponownie odwiedź stronę `~/Admin/Default.aspx`. Tym razem plik obrazu `PoweredByASPNET.gif` pojawia się na stronie (patrz rysunek 3). Gdy kontrolka sieci Web obrazu jest renderowana, używa metody `ResolveClientUrl`, aby rozwiązać jej `ImageUrl` wartość właściwości. W `~/Admin/Default.aspx` `ImageUrl` jest konwertowany na odpowiedni względny adres URL, ponieważ następujący fragment kodu przedstawia źródło HTML:

[!code-html[Main](urls-in-master-pages-vb/samples/sample6.html)]

> [!NOTE]
> Oprócz użycia we właściwościach kontrolek sieci Web opartych na adresach URL, `~` może być również używana podczas wywoływania `Response.Redirect` i `Server.MapPath` metod między innymi. Ponadto Metoda `ResolveClientUrl` może być wywoływana bezpośrednio ze znacznika deklaracyjnego ASP.NET lub strony wzorcowej, w razie konieczności; Zobacz wpis w blogu [Fritz cebuli](https://www.pluralsight.com/blogs/fritz/) [przy użyciu `ResolveClientUrl` znaczników](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).

## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Ustalanie pozostałych względnych adresów URL strony wzorcowej

Oprócz elementu `<img>` w `footerContent`, który właśnie został naprawiony, Strona główna zawiera jeden bardziej względny adres URL, który wymaga naszej uwagi. Region `topContent` zawiera link "samouczki stron wzorcowych", które wskazują `Default.aspx`.

[!code-html[Main](urls-in-master-pages-vb/samples/sample7.html)]

Ponieważ ten adres URL jest względny, wyśle użytkownika do strony `Default.aspx` w folderze strony zawartości, do której się odwiedza. Aby ten link zawsze wskazywał `Default.aspx` w folderze głównym, musi zastąpić element `<a>` za pomocą kontrolki sieci Web hiperłącza, aby można było użyć notacji `~`.

Usuń `<a>` znaczników elementów i Dodaj kontrolkę HyperLink w swoim miejscu. Ustaw `ID` hiperłącze na `lnkHome`, jego właściwość `NavigateUrl` na `~/Default.aspx`i Właściwość `Text` na "samouczków stron wzorcowych".

[!code-aspx[Main](urls-in-master-pages-vb/samples/sample8.aspx)]

Gotowe. W tym momencie wszystkie adresy URL na naszej stronie głównej są prawidłowo oparte na tym, jak są renderowane przez stronę zawartości, niezależnie od tego, które foldery strony wzorcowej i zawartości znajdują się w.

### <a name="automatic-url-resolution-in-theheadsection"></a>Automatyczne rozpoznawanie adresów URL w sekcji`<head>`

W samouczku [*Tworzenie układu dla całej witryny za pomocą stron wzorcowych*](creating-a-site-wide-layout-using-master-pages-vb.md) dodaliśmy `<link>` do pliku `Styles.css` w regionie `<head>`:

[!code-aspx[Main](urls-in-master-pages-vb/samples/sample9.aspx)]

Gdy atrybut `href` elementu `<link>` jest względny, jest automatycznie konwertowany do odpowiedniej ścieżki w czasie wykonywania. Zgodnie z opisem w sekcji [*Określanie tytułu, Meta tagów i innych nagłówków HTML na stronie wzorcowej*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) , region `<head>` jest w rzeczywistości kontrolk po stronie serwera, dzięki czemu można modyfikować zawartość wewnętrznych formantów, gdy jest renderowany.

Aby to sprawdzić, odwiedź stronę `~/Admin/Default.aspx` i Wyświetl źródło HTML wysłane do przeglądarki. Jak ilustruje poniższy fragment kodu, atrybut `href` elementu `<link>` został automatycznie zmodyfikowany do odpowiedniego względnego adresu URL, `../Styles.css`.

[!code-html[Main](urls-in-master-pages-vb/samples/sample10.html)]

## <a name="summary"></a>Podsumowanie

Na stronach głównych często są uwzględniane linki, obrazy i inne zasoby zewnętrzne, które należy określić za pośrednictwem adresu URL. Ze względu na to, że strony wzorcowe i strony zawartości mogą nie istnieć w tym samym folderze, ważne jest, aby wstrzymać korzystanie z adresów URL. Chociaż możliwe jest użycie sztywnych bezwzględnych adresów URL, należy to dokładnie Couples bezwzględny adres URL do aplikacji sieci Web. Jeśli bezwzględne zmiany adresu URL — często podczas przechodzenia lub wdrażania aplikacji sieci Web, trzeba pamiętać, aby wrócić i zaktualizować bezwzględne adresy URL.

Idealnym podejściem jest użycie tyldy (`~`), aby wskazać katalog główny aplikacji. Kontrolki sieci Web ASP.NET zawierające właściwości powiązane z adresem URL mapują `~` do katalogu głównego aplikacji w czasie wykonywania. Wewnętrznie formanty sieci Web używają metody `ResolveClientUrl` klasy `Control` do wygenerowania prawidłowego względnego adresu URL. Ta metoda jest publiczna i dostępna z każdego formantu serwera (łącznie z klasą `Page`), dlatego można używać jej programowo z poziomu klas związanych z kodem, w razie potrzeby.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Strony wzorcowe w ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [Rebazowanie adresu URL na stronie wzorcowej](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Używanie `ResolveClientUrl` w znacznikach](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor wielu książek ASP/ASP. NET Books i założyciel of 4GuysFromRolla.com, pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 3,5 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott można uzyskać w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu w [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania

Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Poprzednie](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
> [dalej](control-id-naming-in-content-pages-vb.md)

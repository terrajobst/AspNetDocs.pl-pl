---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Dodawanie sieci społecznościowych do witryn ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym rozdziale wyjaśniono, jak zintegrować witrynę z usługami sieci społecznościowych. W tym rozdziale dowiesz się, jak umożliwić użytkownikom zakładanie/łączenie witryny sieci Web...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 1637464b0473bba8133acbbf8918d92b4f552701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526937"
---
# <a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Dodawanie sieci społecznościowych do witryn ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak dodawać linki do sieci społecznościowych dla usług Facebook, Twitter, reddit i Digg do stron w witrynie sieci Web ASP.NET (Razor), a także dołączać kanały informacyjne usługi Twitter, karty Xbox graczy i obrazy skonfigurowali Gravatar.
> 
> Zawartość:
> 
> - Jak umożliwić użytkownikom zakładanie/łączenie się z witryną.
> - Jak dodać kanał informacyjny usługi Twitter.
> - Jak dodać przycisk **like** do serwisu Facebook do stron.
> - Jak renderować obrazy Gravatar.com.
> - Jak wyświetlić kartę Xbox graczy w swojej witrynie.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 2
> - Biblioteka pomocnika sieci Web ASP.NET (pakiet NuGet)
>   
> 
> Ten samouczek działa również z ASP.NET Web Pages 3, z wyjątkiem części, które korzystają z biblioteki pomocnika sieci Web ASP.NET.

<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Łączenie witryny sieci Web w witrynach sieci społecznościowej

Jeśli ludzie lubią coś w Twojej witrynie, często chcą udostępnić go znajomym. Możesz to łatwo zrobić, wyświetlając glify (ikony), które użytkownicy mogą kliknąć, aby udostępnić stronę w witrynie Digg, reddit, Facebook, Twitter lub podobne.

Aby wyświetlić te glify, Dodaj pomocnika `LinkSharecode` do strony. Osoby, które odwiedzają Twoją stronę, mogą kliknąć pojedynczy glif. Jeśli mają one konto w tej witrynie sieci społecznościowej, można opublikować link do strony w tej witrynie.

![Obraz 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny internetowej zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli nie została jeszcze dodana. — Utwórz stronę o nazwie *ListLinkShare. cshtml* i Dodaj następujące znaczniki:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    W tym przykładzie w przypadku uruchomienia pomocnika `LinkShare` tytuł strony jest przekazywany jako parametr, który z kolei przekazuje tytuł strony do witryny sieci społecznościowej. Można jednak przekazać dowolny ciąg. Ten przykład określa również, które witryny sieci społecznościowej mają być uwzględnione na liście. Możesz określić witryny sieci społecznościowej odpowiednie dla danej witryny.
2. Uruchom stronę *ListLinkShare. cshtml* w przeglądarce. (Upewnij się, że strona została wybrana w obszarze roboczym **pliki** przed jej uruchomieniem).
3. Kliknij symbol dla jednej z witryn, dla których jesteś zarejestrowany. Łącze prowadzi do strony w wybranej witrynie sieci społecznościowej, w której można udostępnić link. Jeśli na przykład klikniesz link Reddit, nastąpi przekierowanie do strony `submit to reddit` w witrynie reddit.

     ![Obraz 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Dodawanie kanału informacyjnego usługi Twitter

Aby uzyskać informacje dotyczące korzystania z pomocnika usługi Twitter, który jest zgodny z bieżącą wersją interfejsu API usługi Twitter, zobacz [pomocnik usługi Twitter](../ui-layouts-and-themes/twitter-helper.md). Ten przykład pokazuje, jak napisać własny pomocnik, aby można było łatwo użyć kodu z wielu stron.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Wyświetlanie &quot;w serwisie Facebook, np. przycisku&quot;

W niektórych przypadkach najlepszym rozwiązaniem jest uzyskanie kodu bezpośrednio od dostawcy sieci społecznościowej, a nie poleganie na Pomocniku. Jest to szczególnie ważne, jeśli dostawca sieci społecznościowej aktualizuje swoje opcje szybciej niż w przypadku aktualizacji pomocnika.

Aby dodać do witryny funkcje serwisu Facebook (takie jak przycisk Like), można pobrać fragmenty kodu z witryny [Developers.Facebook.com](https://developers.facebook.com/) . W witrynie Facebook można użyć narzędzi do wygenerowania fragmentu kodu, który jest istotny dla danej witryny.

Następujący wyróżniony kod to kod, który został pobrany z narzędzia Like Button w witrynie developers.facebook.com. Musisz podać własny identyfikator aplikacji.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Renderowanie obrazu skonfigurowali Gravatar

Element *skonfigurowali Gravatar* (&quot;globalnie rozpoznany&quot;) to obraz, który może być używany w wielu witrynach sieci Web jako awatar &#8212; , który reprezentuje obraz. Na przykład skonfigurowali Gravatar może identyfikować osobę w wpisie na forum, w komentarzu do blogu i tak dalej. (Możesz zarejestrować własny skonfigurowali Gravatar w witrynie sieci Web skonfigurowali Gravatar w [http://www.gravatar.com/](http://www.gravatar.com/)). Jeśli chcesz wyświetlać obrazy obok nazw osób lub adresów e-mail w witrynie sieci Web, możesz użyć pomocnika skonfigurowali Gravatar.

W tym przykładzie jest używany pojedynczy skonfigurowali Gravatar, który reprezentuje siebie. Innym sposobem korzystania z skonfigurowali Gravatar jest umożliwienie użytkownikom określenia ich adresu skonfigurowali Gravatar po zarejestrowaniu w witrynie. (Można dowiedzieć się, jak umożliwić użytkownikom rejestrowanie się w [celu dodania zabezpieczeń i członkostwa w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).) Następnie za każdym razem, gdy wyświetlasz informacje dla tego użytkownika, możesz po prostu dodać skonfigurowali Gravatar do miejsca, w którym jest wyświetlana nazwa użytkownika.

1. Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny sieci Web zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372)(jeśli jeszcze nie zostało to zrobione).
2. Utwórz nową stronę sieci Web o nazwie *skonfigurowali Gravatar. cshtml*.
3. Dodaj następujący znacznik do pliku: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    Metoda `Gravatar.GetHtml` wyświetla obraz skonfigurowali Gravatar na stronie. Aby zmienić rozmiar obrazu, można dołączyć liczbę jako drugi parametr. Domyślny rozmiar to 80. Liczby mniejsze niż 80 sprawiają, że obraz jest mniejszy. Liczby większe niż 80 zwiększają rozmiar obrazu.
4. W `Gravatar.GetHtml` metod Zastąp `<Your Gravatar account here>` adresem e-mail używanym dla konta skonfigurowali Gravatar. (Jeśli nie masz konta skonfigurowali Gravatar, możesz użyć adresu e-mail osoby, która robi).
5. Uruchom stronę w przeglądarce. Na stronie są wyświetlane dwa obrazy skonfigurowali Gravatar dla podanego adresu e-mail. Drugi obraz jest mniejszy od pierwszego. 

    ![Obraz 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Wyświetlanie karty Xbox graczy

Gdy ludzie ododgrywają gry Microsoft Xbox w trybie online, każdy użytkownik ma unikatowy identyfikator. Statystyki są przechowywane dla każdego odtwarzacza w postaci karty graczy, która przedstawia ich reputację, ocenę graczy i ostatnio odtwarzane gry. Jeśli jesteś konsolą Xbox graczy, możesz wyświetlić kartę graczy na stronach w witrynie za pomocą pomocnika `GamerCard`.

1. Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny sieci Web zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372)(jeśli jeszcze nie zostało to zrobione).
2. Utwórz nową stronę o nazwie *XboxGamer. cshtml* i Dodaj następujący znacznik.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Użyj właściwości `GamerCard.GetHtml`, aby określić alias dla karty graczy, która ma być wyświetlana.
3. Uruchom stronę w przeglądarce. Na stronie zostanie wyświetlona określona karta Xbox graczy.

    ![Obraz 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)

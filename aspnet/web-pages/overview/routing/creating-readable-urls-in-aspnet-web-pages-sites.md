---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Tworzenie możliwych do odczytania adresów URL w witrynach ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym artykule opisano Routing w witrynie internetowej ASP.NET Web Pages (Razor) i sposób, w jaki można korzystać z adresów URL, które są bardziej czytelne i lepsze w przypadku optymalizacji. Co się stanie...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 832db8e144cab730f16c78f67c12feb9b7c92c7c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628395"
---
# <a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Tworzenie możliwych do odczytania adresów URL w witrynach ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule opisano Routing w witrynie internetowej ASP.NET Web Pages (Razor) i sposób, w jaki można korzystać z adresów URL, które są bardziej czytelne i lepsze w przypadku optymalizacji.
> 
> Zawartość:
> 
> - Jak ASP.NET korzysta z routingu, aby umożliwić korzystanie z bardziej czytelnych i przeszukiwanych adresów URL.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 3
>   
> 
> Ten samouczek działa również z ASP.NET Web Pages 2.

## <a name="about-routing"></a>Informacje o routingu

Adresy URL stron w witrynie mogą mieć wpływ na działanie lokacji. Adres URL, który jest &quot;przyjazny&quot; może ułatwić innym osobom korzystanie z tej witryny. Może również pomóc w połączeniu z optymalizacją aparatu wyszukiwania (wyszukiwarka) dla witryny. Witryny sieci Web ASP.NET umożliwiają automatyczne korzystanie z przyjaznych adresów URL.

ASP.NET umożliwia tworzenie znaczących adresów URL, które opisują akcje użytkownika zamiast wskazywać na plik na serwerze. Te adresy URL należy wziąć pod uwagę dla fikcyjnego bloga:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Porównaj te adresy URL z następującymi:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

W pierwszej parze użytkownik musi wiedzieć, że blog jest wyświetlany przy użyciu strony *blog. cshtml* , a następnie musi utworzyć ciąg zapytania, który pobiera prawą kategorię lub zakres dat. Drugi zestaw przykładów jest znacznie łatwiejszy do comprehend i tworzenia.

Adresy URL pierwszego przykładu wskazują również na określony plik (*blog. cshtml*). Jeśli z jakiegoś powodu blog został przeniesiony do innego folderu na serwerze, lub jeśli blog został ponownie zapisany w celu użycia innej strony, linki byłyby nieodpowiednie. Drugi zestaw adresów URL nie wskazuje na określoną stronę, więc nawet w przypadku zmiany implementacji lub lokalizacji blogu adresy URL nadal będą prawidłowe.

Na stronach sieci Web ASP.NET można tworzyć bardziej przyjaznej adresy URL, takie jak te w powyższych przykładach, ponieważ usługa ASP.NET używa *routingu*. Routing tworzy logiczne mapowanie na podstawie adresu URL strony (lub stron), które mogą spełnić żądanie. Ponieważ mapowanie jest logiczne (nie fizyczne, do określonego pliku), routing zapewnia dużą elastyczność w zakresie definiowania adresów URL dla witryny.

## <a name="how-routing-works"></a>Jak działa Routing

Gdy ASP.NET przetwarza żądanie, odczytuje adres URL, aby określić sposób ich trasy. ASP.NET próbuje dopasować poszczególne segmenty adresu URL do plików na dysku, przechodząc od lewej do prawej. W przypadku dopasowania wszystkie pozostałe elementy w adresie URL są przesyłane do strony jako *Informacje o ścieżce*.

Załóżmy, że ktoś wysyła żądanie przy użyciu tego adresu URL:

`http://www.contoso.com/a/b/c`

Wyszukiwanie przebiega następująco:

1. Czy istnieje plik o ścieżce i nazwie */a/b/c.cshtml*? Jeśli tak, uruchom tę stronę i Przekaż do niej informacje. W innym przypadku...
2. Czy istnieje plik o ścieżce i nazwie */a/b.cshtml*? Jeśli tak, uruchom tę stronę i przekaż wartość `c`. W przeciwnym razie...
3. Czy istnieje plik o ścieżce i nazwie */a.cshtml*? Jeśli tak, uruchom tę stronę i przekaż wartość `b/c`.

Jeśli w określonych folderach nie znaleziono dokładnego dopasowania dla plików *. cshtml* , ASP.NET nadal szuka następujących plików:

1. */a/b/c/default.cshtml* (brak informacji o ścieżce).
2. */a/b/c/index.cshtml* (brak informacji o ścieżce).

> [!NOTE]
> Aby można było wyczyścić, żądania dla określonych stron (czyli żądań zawierających rozszerzenie nazwy pliku *. cshtml* ) działają podobnie jak w przypadku oczekiwań. Żądanie, takie jak `http://www.contoso.com/a/b.cshtml`, uruchomi stronę *b. cshtml* w prawidłowy sposób.

Wewnątrz strony możesz uzyskać informacje o ścieżce za pośrednictwem właściwości `UrlData` strony, która jest słownikiem. Załóżmy, że masz plik o nazwie *ViewCustomers. cshtml* , a witryna otrzymuje to żądanie:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Zgodnie z opisem w powyższych regułach żądanie zostanie umieszczone na stronie. Wewnątrz strony można użyć kodu, takiego jak następujące, aby uzyskać i wyświetlić informacje o ścieżce (w tym przypadku wartość &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Ponieważ Routing nie obejmuje pełnych nazw plików, może wystąpić niejednoznaczność, jeśli masz strony mające taką samą nazwę, ale inne rozszerzenia nazwy pliku (np *. webpage. cshtml* i *webpage. html*). Aby uniknąć problemów z routingiem, najlepiej upewnić się, że nie masz stron w swojej witrynie, których nazwy różnią się tylko w ich rozszerzeniu.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

[WebMatrix — adresy URL, UrlData i Routing dla aparatu optymalizacji](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Ten wpis w blogu z Jan solance zawiera kilka dodatkowych informacji na temat sposobu działania routingu na stronach sieci Web ASP.NET.

---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Część 7: Dodawanie funkcji | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 7 dodaje dodatkowe funkcje, takie jak Revie konta...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: ffd2b862c727db9572c272b7b21bcc33c822fffa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641996"
---
# <a name="part-7-adding-features"></a>Część 7. Dodawanie funkcji

Jan [Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.
> 
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 7 dodaje dodatkowe funkcje, takie jak przegląd konta, przeglądy produktów i "popularne elementy" oraz "zakupione również" kontrolki użytkownika.

## <a id="_Toc260221673"></a>Dodawanie funkcji

Chociaż użytkownicy mogą przeglądać nasz katalog, umieszczać elementy w koszyku zakupów i dokończyć proces wyewidencjonowania, można dołączać do nich wiele funkcji pomocniczych w celu ulepszania naszej witryny.

1. Przegląd konta (Lista zamówień umieszczonych i Wyświetl szczegóły).
2. Dodaj zawartość specyficzną dla kontekstu do strony frontonu.
3. Dodaj funkcję, aby umożliwić użytkownikom przeglądanie produktów w wykazie.
4. Utwórz kontrolkę użytkownika, aby wyświetlić popularne elementy i umieścić tę kontrolkę na stronie frontonu.
5. Utwórz "zakupione również" kontrolkę użytkownika i Dodaj ją do strony szczegółów produktu.
6. Dodaj stronę kontaktu.
7. Dodaj stronę informacje.
8. Błąd globalny

## <a id="_Toc260221674"></a>Przegląd konta

W folderze "Account" Utwórz dwie strony. aspx o nazwie OrderList. aspx, a drugie o nazwie OrderDetails. aspx

OrderList. aspx będzie korzystać z kontrolek GridView i EntityDataSource, tak jak wcześniej.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

Obiekt EntityDataSource wybiera rekordy z tabeli Orders filtrowanej według nazwy użytkownika (zobacz WhereParameter), która została ustawiona w zmiennej sesji podczas logowania użytkownika.

Zwróć uwagę na to, że te parametry HyperlinkField widoku GridView:

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

Określają one łącze do widoku szczegółów zamówienia dla każdego produktu, określając pole IDZamówienia jako parametr QueryString na stronie OrderDetails. aspx.

## <a id="_Toc260221675"></a>OrderDetails. aspx

Użyjemy kontrolki EntityDataSource do uzyskiwania dostępu do zamówień i FormView, aby wyświetlić dane zamówienia i inny obiekt EntityDataSource z elementem GridView, aby wyświetlić wszystkie elementy wiersza zamówienia.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

W pliku związanym z kodem (OrderDetails.aspx.cs) mamy dwa małe bity dla gospodarstw domowych.

Najpierw musimy upewnić się, że OrderDetails zawsze otrzymuje IDZamówienia.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

Musimy również obliczyć i wyświetlić sumę zamówień z elementów wiersza.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>Strona główna

Dodajmy zawartość statyczną do domyślnej strony. aspx.

Najpierw utworzymy folder "Content" (zawartość), a w tym folderze obrazów (i dodamy obraz do użycia na stronie głównej).

Na dolny symbol zastępczy domyślnej strony. aspx Dodaj następujący znacznik.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>Przeglądy produktu

Najpierw dodamy przycisk z linkiem do formularza, za pomocą którego możemy wprowadzić przegląd produktu.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

Zwróć uwagę, że przekazujemy klucz ProductID w ciągu zapytania

Dodaj stronę o nazwie ReviewAdd. aspx

Ta strona będzie używać zestawu narzędzi ASP.NET AJAX Control. Jeśli jeszcze tego nie zrobiono, możesz pobrać go z [subskrypcja DevExpress](http://devexpress.com/act) i uzyskać wskazówki dotyczące konfigurowania zestawu narzędzi do użycia z programem Visual Studio tutaj [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).

W trybie projektowania przeciągnij kontrolki i moduły walidacji z przybornika i Utwórz formularz podobny do przedstawionego poniżej.

![](tailspin-spyworks-part-7/_static/image2.jpg)

Oznakowanie będzie wyglądać podobnie do tego.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

Teraz, gdy możemy wprowadzić przeglądy, umożliwia wyświetlenie tych przeglądów na stronie produkt.

Dodaj tę adiustację do strony ProductDetails. aspx.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

Uruchomienie naszej aplikacji teraz i przechodzenie do produktu powoduje wyświetlenie informacji o produkcie, w tym przeglądów klientów.

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>Kontrolka popularne elementy (Tworzenie formantów użytkownika)

Aby zwiększyć sprzedaż w witrynie sieci Web, dodamy kilka funkcji do popularnych lub powiązanych produktów "z sugestiami".

Pierwszy z tych funkcji będzie listą bardziej popularnego produktu w naszym katalogu produktów.

Utworzymy "kontrolkę użytkownika" w celu wyświetlenia najważniejszych elementów sprzedaży na stronie głównej naszej aplikacji. Ponieważ będzie to formant, można go używać na dowolnej stronie przez przeciąganie i upuszczanie kontrolki w projektancie programu Visual Studio na dowolną stronę, którą lubimy.

W Eksploratorze rozwiązań programu Visual Studio kliknij prawym przyciskiem myszy nazwę rozwiązania i Utwórz nowy katalog o nazwie "Controls" (kontrolki). Chociaż nie jest to konieczne, pomożemy nam zorganizować projekt, tworząc wszystkie formanty użytkownika w katalogu "Controls".

Kliknij prawym przyciskiem myszy folder Controls i wybierz pozycję "nowy element":

![](tailspin-spyworks-part-7/_static/image4.jpg)

Określ nazwę naszej kontroli "PopularItems". Należy zauważyć, że rozszerzenie pliku dla kontrolek użytkownika to. ascx not. aspx.

Nasze popularne elementy formant użytkownika zostanie zdefiniowany w następujący sposób.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

W tym miejscu korzystamy z metody, która nie została jeszcze użyta w tej aplikacji. Używamy formantu wzmacniania, a nie za pomocą kontroli źródła danych, aby powiązać formant wzmacnia z wynikami zapytania LINQ to Entitiesowego.

W kodzie związanym z naszym formantem Wykonujemy poniższe czynności.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

Pamiętaj również, że ten ważny wiersz znajduje się w górnej części znacznika kontrolki.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

Ze względu na to, że najpopularniejsze elementy nie zostaną zmienione na minutę, możemy dodać dyrektywę aching, aby zwiększyć wydajność naszej aplikacji. Ta dyrektywa spowoduje, że kod kontrolny zostanie wykonany tylko po wygaśnięciu zbuforowanego danych wyjściowych formantu. W przeciwnym razie zostanie użyta buforowana wersja danych wyjściowych kontrolki.

Teraz wszystko, co należy zrobić, obejmuje nasz nowy formant na stronie Default. aspx.

Użyj przeciągania i upuszczania, aby umieścić wystąpienie kontrolki w kolumnie Otwórz w naszym formularzu domyślnym.

![](tailspin-spyworks-part-7/_static/image5.jpg)

Teraz podczas uruchamiania naszej aplikacji na stronie głównej są wyświetlane najbardziej popularne elementy.

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>Kontrolka "kupione również" (kontrolki użytkownika z parametrami)

Druga kontrolka użytkownika, którą utworzymy, zajmie się sugerowanym sprzedażą do następnego poziomu, dodając specyficzność kontekstu.

Logika służąca do obliczania elementów Top "zakupionych" jest nieprosta.

Nasz "zakupiony" formant wybierze rekordy OrderDetails (wcześniej zakupione) dla aktualnie wybranego identyfikatora ProductID i odnalezienie identyfikatorów unikatowych dla każdego unikatowego zamówienia.

Następnie wybierzemy pozycję Al produkty ze wszystkich tych zamówień i zasumujesz zakupione ilości. Posortujemy produkty według tej ilości sum i wyświetlamy pięć pierwszych elementów.

Ze względu na złożoność tej logiki zaimplementujmy ten algorytm jako procedurę składowaną.

Język T-SQL dla procedury składowanej jest następujący:

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

Należy zauważyć, że ta procedura składowana (SelectPurchasedWithProducts) istniała w bazie danych, gdy została ona uwzględniona w naszej aplikacji, a w przypadku wygenerowania Entity Data Model określić, że oprócz tabel i widoków, które są niezbędne, Entity Data Model powinna zawierać tę procedurę składowaną.

Aby uzyskać dostęp do procedury składowanej z Entity Data Model musimy zaimportować funkcję.

Kliknij dwukrotnie Entity Data Model w Eksploratorze rozwiązań, aby otworzyć go w Projektancie i otworzyć przeglądarkę modelu, a następnie kliknij prawym przyciskiem myszy w Projektancie i wybierz pozycję "Dodaj Import funkcji".

![](tailspin-spyworks-part-7/_static/image1.png)

Spowoduje to otwarcie tego okna dialogowego.

![](tailspin-spyworks-part-7/_static/image2.png)

Wypełnij pola, jak widać powyżej, wybierając "SelectPurchasedWithProducts" i użyj nazwy procedury dla nazwy naszej funkcji zaimportowanej.

Kliknij przycisk "OK".

Po wykonaniu tej czynności można po prostu programować w odniesieniu do procedury składowanej, ponieważ możemy użyć dowolnego innego elementu w modelu.

W naszym folderze "Controls" Utwórz nową kontrolkę użytkownika o nazwie AlsoPurchased. ascx.

Znaczniki dla tej kontrolki będą wyglądały na bardzo zaznajomione z kontrolką PopularItems.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

Istotną różnicą jest to, że nie buforowanie danych wyjściowych, ponieważ element, który ma być renderowany, będzie różnić się w zależności od produktu.

Identyfikator ProductId będzie "właściwości" kontrolki.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

W programie obsługi zdarzeń zdarzenia PreRender EED do wykonania trzy rzeczy.

1. Upewnij się, że identyfikator ProductID jest ustawiony.
2. Sprawdź, czy istnieją jakieś produkty, które zostały zakupione w ramach bieżącego.
3. Wyprowadzanie niektórych elementów jako określonych w #2.

Zwróć uwagę na to, jak łatwo jest wywołać procedurę składowaną za pomocą modelu.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

Po ustaleniu, że istnieje również "zakup", można po prostu powiązać wzmacniak z wynikami zwróconymi przez zapytanie.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

Jeśli nie zostały jeszcze zakupione elementy, po prostu wyświetlamy inne popularne elementy z naszego katalogu.

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

Aby wyświetlić elementy "kupione również", Otwórz stronę ProductDetails. aspx i przeciągnij kontrolkę AlsoPurchased z Eksploratora rozwiązań, aby była wyświetlana w tym miejscu w znaczniku.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

Spowoduje to utworzenie odwołania do kontrolki w górnej części strony ProductDetails.

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

Ponieważ kontrolka użytkownika AlsoPurchased wymaga numeru ProductId, ustawimy Właściwość ProductID formantu przy użyciu instrukcji eval względem bieżącego elementu modelu danych strony.

![](tailspin-spyworks-part-7/_static/image3.png)

Po skompilowaniu i uruchomieniu teraz i przejściu do produktu zobaczysz "kupione również elementy".

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [Poprzednie](tailspin-spyworks-part-6.md)
> [dalej](tailspin-spyworks-part-8.md)

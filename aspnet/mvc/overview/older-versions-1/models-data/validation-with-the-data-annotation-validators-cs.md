---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: Weryfikacja przy użyciu modułów weryfikacji adnotacji danychC#() | Microsoft Docs
author: microsoft
description: Skorzystaj z spinacza model adnotacji danych, aby przeprowadzić walidację w aplikacji ASP.NET MVC. Dowiedz się, jak używać różnych typów modułu sprawdzania poprawności...
ms.author: riande
ms.date: 05/29/2009
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: e154384c08adf0c14920afff85e983a67b41707c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542134"
---
# <a name="validation-with-the-data-annotation-validators-c"></a>Walidacja przy użyciu modułów walidacji adnotacji danych (C#)

przez [firmę Microsoft](https://github.com/microsoft)

> Skorzystaj z spinacza model adnotacji danych, aby przeprowadzić walidację w aplikacji ASP.NET MVC. Dowiedz się, jak używać różnych typów atrybutów modułu sprawdzania poprawności i korzystać z nich w Entity Framework firmy Microsoft.

W tym samouczku dowiesz się, jak za pomocą modułów walidacji adnotacji danych przeprowadzić walidację w aplikacji ASP.NET MVC. Zaletą korzystania z modułów weryfikacji adnotacji danych jest umożliwienie wykonywania walidacji po prostu przez dodanie jednego lub większej liczby atrybutów — takich jak wymagany lub atrybut StringLength — do właściwości klasy.

Aby można było używać modułów sprawdzania poprawności adnotacji danych, należy pobrać spinacz modelu adnotacji danych. Możesz pobrać przykładowy spinacz modelu adnotacji danych z witryny sieci Web CodePlex, klikając [tutaj](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).

Ważne jest, aby zrozumieć, że spinacz modelu adnotacji danych nie jest oficjalną częścią Microsoft ASP.NET platformy MVC. Chociaż spinacz modelu adnotacji danych został utworzony przez zespół Microsoft ASP.NET MVC, firma Microsoft nie oferuje oficjalnej pomocy technicznej dla spinacza modelu adnotacji danych opisanego i użytego w tym samouczku.

## <a name="using-the-data-annotation-model-binder"></a>Używanie spinacza modelu adnotacji danych

Aby można było użyć spinacza modelu adnotacji danych w aplikacji ASP.NET MVC, należy najpierw dodać odwołanie do zestawu Microsoft. Web. MVC. DataAnnotations. dll i system. ComponentModel. DataAnnotations. dll. Wybierz menu opcji **projekt, Dodaj odwołanie**. Następnie kliknij kartę **Przeglądaj** i przejdź do lokalizacji, w której pobrano (i rozpakowane) przykład spinacza modelu adnotacji danych (patrz **rysunek 1**).

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

**Rysunek 1**. Dodawanie odwołania do spinacza modelu adnotacji danych ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](validation-with-the-data-annotation-validators-cs/_static/image3.png))

Wybierz zestaw Microsoft. Web. MVC. DataAnnotations. dll i system. ComponentModel. DataAnnotations. dll, a następnie kliknij przycisk **OK** .

Nie można użyć zestawu System. ComponentModel. DataAnnotations. dll dołączonego do programu .NET Framework Service Pack 1 ze spinaczem modelu adnotacje danych. Musisz użyć wersji zestawu System. ComponentModel. DataAnnotations. dll dołączonego do przykładowego pliku spinacza modelu adnotacji danych.

Na koniec należy zarejestrować spinacz modelu DataAnnotations w pliku Global. asax. Dodaj następujący wiersz kodu do aplikacji\_Start () obsługi zdarzeń, aby aplikacja\_Start () wyglądała następująco:

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

Ten wiersz kodu rejestruje ataAnnotationsModelBinder jako domyślny spinacz modelu dla całej aplikacji ASP.NET MVC.

## <a name="using-the-data-annotation-validator-attributes"></a>Używanie atrybutów modułu sprawdzania poprawności adnotacji danych

Gdy używasz spinacza modelu adnotacje danych, użyj atrybutów modułu sprawdzania poprawności, aby przeprowadzić walidację. Przestrzeń nazw System. ComponentModel. DataAnnotations zawiera następujące atrybuty modułu sprawdzania poprawności:

- Zakres — umożliwia sprawdzenie, czy wartość właściwości jest pomiędzy określonym zakresem wartości.
- RegularExpression — umożliwia sprawdzenie, czy wartość właściwości jest zgodna z określonym wzorcem wyrażenia regularnego.
- Wymagane — umożliwia oznaczenie właściwości zgodnie z potrzebami.
- StringLength — pozwala określić maksymalną długość właściwości ciągu.
- Walidacja — Klasa podstawowa dla wszystkich atrybutów modułu sprawdzania poprawności.

> [!NOTE] 
> 
> Jeśli Twoje potrzeby weryfikacji nie są spełnione przez żadną z standardowych modułów sprawdzania poprawności, zawsze istnieje możliwość utworzenia niestandardowego atrybutu modułu sprawdzania poprawności przez dziedziczenie nowego atrybutu modułu walidacji z bazowego atrybutu walidacji.

Klasa produktu na liście **1** ilustruje sposób używania tych atrybutów modułu sprawdzania poprawności. Właściwości Name, Description i CenaJednostkowa są oznaczone jako wymagane. Właściwość Name musi mieć długość ciągu krótszą niż 10 znaków. Na koniec Właściwość CenaJednostkowa musi być zgodna ze wzorcem wyrażenia regularnego, który reprezentuje kwotę w walucie.

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

**Lista 1**: Models\Product.cs

Klasa Product ilustruje sposób użycia jednego dodatkowego atrybutu: atrybut DisplayName. Atrybut DisplayName umożliwia modyfikowanie nazwy właściwości, gdy właściwość jest wyświetlana w komunikacie o błędzie. Zamiast wyświetlania komunikatu o błędzie "pole CenaJednostkowa jest wymagane" można wyświetlić komunikat o błędzie "pole Cena jest wymagane".

> [!NOTE] 
> 
> Jeśli chcesz całkowicie dostosować komunikat o błędzie wyświetlany przez moduł sprawdzania poprawności, możesz przypisać niestandardowy komunikat o błędzie do właściwości ErrorMessage modułu sprawdzania poprawności, takiej jak: `<Required(ErrorMessage:="This field needs a value!")>`

Możesz użyć klasy Product na liście **1** za pomocą akcji kontrolera Create () w temacie **list 2**. Ta akcja kontrolera ponownie wyświetla widok tworzenie, gdy model zawiera wszystkie błędy.

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

**Lista 2**: Controllers\ProductController.vb

Na koniec możesz utworzyć widok na **liście 3** , klikając prawym przyciskiem myszy akcję Utwórz () i wybierając opcję menu **Dodaj widok**. Utwórz widok o jednoznacznie określonym typie z klasą produktu jako Klasa modelu. Wybierz pozycję **Utwórz** z listy rozwijanej Wyświetl zawartość (patrz **rysunek 2**).

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

**Rysunek 2**. Dodawanie widoku tworzenia

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

**Lista 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Usuń pole ID z formularza tworzenie wygenerowanego przez opcję menu **Dodaj widok** . Ponieważ pole ID odpowiada kolumnie tożsamości, nie należy zezwalać użytkownikom na wprowadzanie wartości dla tego pola.

Jeśli przesyłasz formularz do tworzenia produktu i nie wprowadzisz wartości wymaganych pól, zostaną wyświetlone komunikaty o błędach walidacji na **rysunku 3** .

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

**Rysunek 3**. brakujące pola wymagane

Po wprowadzeniu nieprawidłowej kwoty w walucie zostanie wyświetlony komunikat o błędzie na **rysunku 4** .

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

**Ilustracja 4**. nieprawidłowa kwota waluty

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Używanie modułów weryfikacji adnotacji danych z Entity Framework

Jeśli używasz Entity Framework firmy Microsoft do wygenerowania klas modelu danych, nie możesz zastosować atrybutów modułu sprawdzania poprawności bezpośrednio do klas. Ponieważ Entity Framework Designer generuje klasy modelu, wszelkie zmiany wprowadzone w klasach modelu zostaną nadpisywane przy następnym wprowadzeniu zmian w projektancie.

Jeśli chcesz użyć modułów walidacji z klasami wygenerowanymi przez Entity Framework, musisz utworzyć klasy meta data. Przed zastosowaniem modułów walidacji do rzeczywistej klasy należy zastosować do klasy meta danych.

Załóżmy na przykład, że utworzono klasę filmu przy użyciu Entity Framework (patrz **rysunek 5**). Wyobraź sobie, że na pewno chcesz zmienić właściwości tytuł filmu i Director właściwości. W takim przypadku można utworzyć klasę klasy częściowej i metadanych w liście **4**.

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

**Rysunek 5**: Klasa filmu wygenerowana przez Entity Framework

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

**Lista 4**: Models\Movie.cs

Plik w **liście 4** zawiera dwie klasy o nazwie Movie i MovieMetaData. Klasa filmu jest klasą częściową. Odpowiada klasie częściowej wygenerowanej przez Entity Framework, która jest zawarta w pliku datamode. Designer. vb.

Obecnie platforma .NET Framework nie obsługuje właściwości częściowych. W związku z tym nie istnieje sposób zastosowania atrybutów modułu walidacji do właściwości klasy filmowej zdefiniowanej w pliku datamode. Designer. vb przez zastosowanie atrybutów modułu walidacji do właściwości klasy filmowej zdefiniowanej w pliku na liście **4**.

Zwróć uwagę, że Klasa filmu częściowej jest uzupełniona atrybutem datadatatype, który wskazuje na klasę MovieMetaData. Klasa MovieMetaData zawiera właściwości serwera proxy dla właściwości klasy filmu.

Atrybuty modułu sprawdzania poprawności są stosowane do właściwości klasy MovieMetaData. Właściwości title, Director i DateReleased są oznaczone jako wymagane właściwości. Właściwość Director musi mieć przypisany ciąg zawierający mniej niż 5 znaków. Na koniec atrybut DisplayName jest stosowany do właściwości DateReleased, aby wyświetlić komunikat o błędzie, np. pole Data wydania jest wymagane. zamiast błędu "pole DateReleased jest wymagane".

> [!NOTE] 
> 
> Zwróć uwagę, że właściwości serwera proxy w klasie MovieMetaData nie muszą reprezentować tych samych typów co odpowiednie właściwości w klasie filmu. Na przykład właściwość Director jest właściwością ciągu w klasie Movie i właściwości Object w klasie MovieMetaData.

Strona na **rysunku 6** ilustruje komunikaty o błędach zwracane po wprowadzeniu nieprawidłowych wartości właściwości filmu.

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

**Ilustracja 6**. używanie modułów walidacji z Entity Framework ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](validation-with-the-data-annotation-validators-cs/_static/image14.png))

## <a name="summary"></a>Podsumowanie

W ramach tego samouczka nauczysz się, jak wykorzystać spinacz modelu adnotacji danych, aby przeprowadzić walidację w aplikacji ASP.NET MVC. Wiesz już, jak używać różnych typów atrybutów modułu sprawdzania poprawności, takich jak atrybuty wymagane i StringLength. Wiesz również, jak używać tych atrybutów podczas pracy z programem Microsoft Entity Framework.

> [!div class="step-by-step"]
> [Poprzednie](validating-with-a-service-layer-cs.md)
> [dalej](creating-model-classes-with-the-entity-framework-vb.md)

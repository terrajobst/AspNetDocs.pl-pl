---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Weryfikowanie przy użyciu interfejsu IDataErrorInfoC#() | Microsoft Docs
author: StephenWalther
description: Stephen Walther pokazuje, jak wyświetlać niestandardowe komunikaty o błędach walidacji, implementując interfejs IDataErrorInfo w klasie modelu.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 938b180da02b1963acffd021d18621d75d1d0447
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542561"
---
# <a name="validating-with-the-idataerrorinfo-interface-c"></a>Walidacja z użyciem interfejsu IDataErrorInfo (C#)

Autor [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther pokazuje, jak wyświetlać niestandardowe komunikaty o błędach walidacji, implementując interfejs IDataErrorInfo w klasie modelu.

Celem tego samouczka jest wyjaśnienie jednego podejścia do wykonywania walidacji w aplikacji ASP.NET MVC. Dowiesz się, jak uniemożliwić komuś przesłanie formularza HTML bez podawania wartości dla wymaganych pól formularza. W tym samouczku dowiesz się, jak przeprowadzić walidację za pomocą interfejsu IErrorDataInfo.

## <a name="assumptions"></a>Założenia

W tym samouczku użyjemy bazy danych MoviesDB i tabeli bazy danych filmów. Ta tabela zawiera następujące kolumny:

<a id="0.5_table01"></a>

| **Nazwa kolumny** | **Typ danych** | **Zezwalaj na wartości null** |
| --- | --- | --- |
| Identyfikator | int | Fałsz |
| Tytuł | Nvarchar(100) | Fałsz |
| Dyrektor ds. | Nvarchar(100) | Fałsz |
| DateReleased | DateTime | Fałsz |

W tym samouczku użyjemy Entity Framework firmy Microsoft do wygenerowania klas modelu bazy danych. Klasa filmu wygenerowana przez Entity Framework zostanie wyświetlona na rysunku 1.

[![jednostki filmu](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**Ilustracja 01**: jednostka filmu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))

> [!NOTE] 
> 
> Aby dowiedzieć się więcej o używaniu Entity Framework do generowania klas modelu bazy danych, zobacz mój Samouczek zatytułowany Tworzenie klas modelu z Entity Framework.

## <a name="the-controller-class"></a>Klasa kontrolera

Używamy kontrolera macierzystego do wyświetlania filmów i tworzenia nowych filmów. Kod dla tej klasy znajduje się na liście 1.

**Lista 1 — Controllers\HomeController.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

Klasa kontrolera macierzystego na liście 1 zawiera dwie akcje create (). Pierwsza akcja wyświetla formularz HTML służący do tworzenia nowego filmu. Druga akcja Create () wykonuje rzeczywiste wstawianie nowego filmu do bazy danych. Druga akcja Create () jest wywoływana, gdy formularz wyświetlany przez pierwszą akcję Utwórz () zostanie przesłany do serwera.

Należy zauważyć, że druga akcja Create () zawiera następujące wiersze kodu:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

Właściwość IsValid zwraca wartość false, jeśli wystąpił błąd walidacji. W takim przypadku zostanie wyświetlony widok tworzenie, który zawiera formularz HTML służący do tworzenia filmu.

## <a name="creating-a-partial-class"></a>Tworzenie klasy częściowej

Klasa filmu jest generowana przez Entity Framework. Kod dla klasy filmów można zobaczyć, jeśli rozwiniesz plik MoviesDBModel. edmx w oknie Eksplorator rozwiązań i otworzysz plik MoviesDBModel.Designer.cs w edytorze kodu (patrz rysunek 2).

[![kod dla jednostki filmu](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**Ilustracja 02**. kod dla jednostki filmu ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))

Klasa filmu jest klasą częściową. Oznacza to, że można dodać kolejną klasę częściową o tej samej nazwie, aby zwiększyć funkcjonalność klasy filmu. Będziemy dodawać nasze logiki walidacji do nowej klasy częściowej.

Dodaj klasę z listy 2 do folderu modele.

**Lista 2 — Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

Należy zauważyć, że Klasa na liście 2 zawiera modyfikator *częściowy* . Wszelkie metody lub właściwości dodawane do tej klasy stają się częścią klasy filmu wygenerowanej przez Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Dodawanie metod OnChanging i onchangeed

Gdy Entity Framework generuje klasę jednostki, Entity Framework automatycznie dodaje metody częściowe do klasy. Entity Framework generuje OnChanged i OnChanged metody, które odpowiadają każdej właściwości klasy.

W przypadku klasy film Entity Framework tworzy następujące metody:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

Metoda onzmiana jest wywoływana bezpośrednio przed zmianą odpowiedniej właściwości. Metoda OnChanged jest wywoływana bezpośrednio po zmianie właściwości.

Możesz skorzystać z tych metod częściowych, aby dodać logikę walidacji do klasy film. Klasa aktualizacji filmów na liście 3 sprawdza, czy tytuł i właściwości dyrektora mają przypisane niepuste wartości.

> [!NOTE] 
> 
> Metoda częściowa to metoda zdefiniowana w klasie, która nie jest wymagana do wdrożenia. Jeśli nie zaimplementujesz metody częściowej, kompilator usunie sygnaturę metody i wszystkie wywołania metody, aby nie było kosztów wykonywania skojarzonych z metodą częściową. W edytorze Visual Studio Code można dodać metodę częściową, wpisując słowo kluczowe *częściowe* , po którym następuje spacja, aby wyświetlić listę części do wdrożenia.

**Lista 3 — Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

Na przykład w przypadku próby przypisania pustego ciągu do właściwości title, komunikat o błędzie zostanie przypisany do słownika o nazwie błędy \_.

W tym momencie nic się nie dzieje, gdy przypiszesz pusty ciąg do właściwości title i zostanie dodany błąd do pola prywatne błędy \_. Musimy zaimplementować interfejs IDataErrorInfo, aby uwidocznić te błędy walidacji w strukturze ASP.NET MVC.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementowanie interfejsu IDataErrorInfo

Interfejs IDataErrorInfo został częścią programu .NET Framework od momentu pierwszej wersji. Ten interfejs jest bardzo prostym interfejsem:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

Jeśli klasa implementuje interfejs IDataErrorInfo, platforma MVC ASP.NET będzie używać tego interfejsu podczas tworzenia wystąpienia klasy. Na przykład akcja Create () na kontrolerze głównym akceptuje wystąpienie klasy Movie:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

Struktura ASP.NET MVC tworzy wystąpienie filmu przesłane do akcji Create () przy użyciu spinacza modelu (DefaultModelBinder). Model spinacza jest odpowiedzialny za tworzenie wystąpienia obiektu filmu przez powiązanie pól formularza HTML z wystąpieniem obiektu filmu.

DefaultModelBinder wykrywa, czy Klasa implementuje interfejs IDataErrorInfo. Jeśli klasa implementuje ten interfejs, spinacz modelu wywoła IDataErrorInfo. ten indeksator dla każdej właściwości klasy. Jeśli indeksator zwróci komunikat o błędzie, spinacz modelu automatycznie dodaje ten komunikat o błędzie do stanu modelu.

DefaultModelBinder również sprawdza Właściwość IDataErrorInfo. Error. Ta właściwość jest przeznaczona do reprezentowania błędów walidacji specyficznych dla niewłaściwości skojarzonych z klasą. Na przykład możesz chcieć wymusić regułę walidacji, która zależy od wartości wielu właściwości klasy filmu. W takim przypadku należy zwrócić błąd walidacji z właściwości Error.

Zaktualizowana Klasa filmów w liście 4 implementuje interfejs IDataErrorInfo.

**Lista 4-Models\Movie.cs (implementuje IDataErrorInfo)**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

W przypadku listy 4 Właściwość indeksatora sprawdza zbieranie \_błędów, aby sprawdzić, czy zawiera on klucz odpowiadający nazwie właściwości przesłanej do indeksatora. Jeśli nie ma błędu walidacji skojarzonego z właściwością, zwracany jest pusty ciąg.

Nie musisz modyfikować kontrolera macierzystego w dowolny sposób, aby użyć zmodyfikowanej klasy filmów. Strona wyświetlana na rysunku 3 ilustruje, co się dzieje, gdy nie wprowadzono wartości dla pól tytuł lub dyrektor.

[![tworzenia metod akcji automatycznie](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**Ilustracja 03**: formularz z brakującymi wartościami ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))

Zauważ, że wartość DateReleased jest weryfikowana automatycznie. Ponieważ właściwość DateReleased nie akceptuje wartości NULL, DefaultModelBinder generuje błąd walidacji dla tej właściwości automatycznie, gdy nie ma wartości. Jeśli chcesz zmodyfikować komunikat o błędzie dla właściwości DateReleased, musisz utworzyć spinacz modelu niestandardowego.

## <a name="summary"></a>Podsumowanie

W tym samouczku przedstawiono sposób użycia interfejsu IDataErrorInfo do generowania komunikatów o błędach walidacji. Najpierw tworzymy częściową klasę filmu, która rozszerza funkcjonalność częściowej klasy filmowej wygenerowanej przez Entity Framework. Następnie dodaliśmy logikę sprawdzania poprawności do klasy filmów OnTitleChanging () i OnDirectorChanging () metod częściowych. Na koniec zaimplementowano interfejs IDataErrorInfo, aby uwidocznić te komunikaty weryfikacyjne w strukturze ASP.NET MVC.

> [!div class="step-by-step"]
> [Poprzednie](performing-simple-validation-cs.md)
> [dalej](validating-with-a-service-layer-cs.md)

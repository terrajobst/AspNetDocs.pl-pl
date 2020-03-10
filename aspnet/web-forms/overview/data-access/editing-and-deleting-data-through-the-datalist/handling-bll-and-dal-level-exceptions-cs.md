---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
title: Obsługa wyjątków na poziomie LOGIKI biznesowej i DAL (C#) | Microsoft Docs
author: rick-anderson
description: W tym samouczku dowiesz się, jak tactfully obsługę wyjątków zgłoszonych podczas edycji przepływu pracy aktualizacji elementu DataList.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: f8fd58e2-f932-4f08-ab3d-fbf8ff3295d2
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: 35ff60be6ed67ea8d1bf226ae70f590100597757
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78610951"
---
# <a name="handling-bll--and-dal-level-exceptions-c"></a>Obsługa wyjątków na poziomie warstwy logiki biznesowej i warstwy dostępu do danych (C#)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_CS.exe) lub [Pobierz plik PDF](handling-bll-and-dal-level-exceptions-cs/_static/datatutorial38cs1.pdf)

> W tym samouczku dowiesz się, jak tactfully obsługę wyjątków zgłoszonych podczas edycji przepływu pracy aktualizacji elementu DataList.

## <a name="introduction"></a>Wprowadzenie

W [omówieniu edytowania i usuwania danych w](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) samouczku elementu DataList utworzyliśmy element DataList, który oferuje proste edytowanie i usuwanie funkcji. Chociaż w pełni funkcjonalny, był trudno przyjazny dla użytkownika, ponieważ wystąpił błąd podczas procesu edytowania lub usuwania spowodował nieobsługiwany wyjątek. Na przykład pominięcie nazwy produktu lub, podczas edytowania produktu, wprowadzenie wartości ceny bardzo przystępne!, zgłasza wyjątek. Ponieważ ten wyjątek nie jest przechwytywany w kodzie, jest on bąbelkowy do środowiska uruchomieniowego ASP.NET, które następnie wyświetla szczegóły wyjątku s na stronie sieci Web.

Zgodnie z opisem w samouczku [Obsługiwanie logiki biznesowej i dal na stronie ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) , jeśli wyjątek jest zgłaszany z głębokości logiki biznesowej lub warstw dostępu do danych, szczegóły wyjątku są zwracane do elementu ObjectDataSource, a następnie do widoku GridView. Znaleźliśmy, jak bezpiecznie obsługiwać te wyjątki, tworząc `Updated` lub `RowUpdated` obsługi zdarzeń dla elementu ObjectDataSource lub GridView, sprawdzając wyjątek, a następnie wskazując, że wyjątek został obsłużony.

Nasze samouczki DataList nie używają jednak elementu ObjectDataSource do aktualizowania i usuwania danych. Zamiast tego pracujemy bezpośrednio nad LOGIKI biznesowej. Aby wykrywać wyjątki pochodzące z LOGIKI biznesowej lub DAL, musimy zaimplementować kod obsługi wyjątków w kodzie zakrytym przez naszą stronę ASP.NET. W tym samouczku dowiesz się, jak więcej tactfully obsługi wyjątków zgłoszonych podczas edytowalnego przepływu pracy aktualizacji elementu DataList.

> [!NOTE]
> *Omówienie edytowania i usuwania danych w* samouczku elementu DataList omawiamy różne techniki edytowania i usuwania danych z elementu DataList, a niektóre z nich są używane do aktualizowania i usuwania. W przypadku zastosowania tych technik można obsłużyć wyjątki z LOGIKI biznesowej lub DAL za pośrednictwem programu ObjectDataSource s `Updated` lub `Deleted` obsługi zdarzeń.

## <a name="step-1-creating-an-editable-datalist"></a>Krok 1. Tworzenie edytowalnej elementu DataList

Przed przeprowadzeniem dalszej obsługi wyjątków, które występują w trakcie aktualizowania przepływu pracy, należy najpierw utworzyć edytowalną element DataList. Otwórz stronę `ErrorHandling.aspx` w folderze `EditDeleteDataList`, Dodaj do projektanta element DataList, ustaw jej Właściwość `ID` na `Products`i Dodaj nowy element ObjectDataSource o nazwie `ProductsDataSource`. Skonfiguruj element ObjectDataSource, aby używał metody `ProductsBLL` Class s `GetProducts()` do wybierania rekordów; Ustaw listę rozwijaną na kartach Wstawianie, aktualizowanie i usuwanie na (brak).

[![zwrócić informacje o produkcie przy użyciu metody getProducts ()](handling-bll-and-dal-level-exceptions-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-cs/_static/image1.png)

**Rysunek 1**. Zwracanie informacji o produkcie przy użyciu metody `GetProducts()` ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-bll-and-dal-level-exceptions-cs/_static/image3.png))

Po ukończeniu działania kreatora ObjectDataSource program Visual Studio automatycznie utworzy `ItemTemplate` dla elementu DataList. Zamień ten element na `ItemTemplate`, w którym wyświetlana jest nazwa i cena produktu i zawiera przycisk Edytuj. Następnie utwórz `EditItemTemplate` za pomocą kontrolki sieci Web TextBox, aby uzyskać nazwę i cenę oraz przyciski Aktualizuj i Anuluj. Na koniec Ustaw Właściwość DataList s `RepeatColumns` na 2.

Po wprowadzeniu tych zmian znaczniki deklaratywne strony powinny wyglądać podobnie do poniższego. Sprawdź podwójne, aby upewnić się, że przyciski Edytuj, Anuluj i Aktualizuj mają swoje `CommandName` właściwości, które mają odpowiednio wartość Edytuj, Anuluj i Aktualizuj.

[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample1.aspx)]

> [!NOTE]
> W tym samouczku stan widoku DataList s musi być włączony.

Poświęć chwilę na wyświetlenie postępu w przeglądarce (patrz rysunek 2).

[![każdy produkt zawiera przycisk Edytuj](handling-bll-and-dal-level-exceptions-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-cs/_static/image4.png)

**Rysunek 2**. Każdy produkt zawiera przycisk Edytuj ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-bll-and-dal-level-exceptions-cs/_static/image6.png))

Obecnie przycisk Edytuj powoduje tylko ogłaszanie zwrotne, ale jeszcze nie umożliwia edytowania produktu. Aby włączyć edytowanie, musimy utworzyć obsługę zdarzeń dla zdarzeń `EditCommand`, `CancelCommand`i `UpdateCommand` dla elementu DataList. Zdarzenia `EditCommand` i `CancelCommand` po prostu zaktualizują Właściwość DataList s `EditItemIndex` i ponownie wiążą dane z:

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample2.cs)]

Obsługa zdarzeń `UpdateCommand` jest nieco większa. Musi on zostać odczytany w `ProductID` edytowanego produktu z kolekcji `DataKeys` wraz z nazwą produktu i ceną z pól tekstowych w `EditItemTemplate`, a następnie wywołać metodę `ProductsBLL` klasy s `UpdateProduct` przed zwróceniem elementu DataList do jego stanu sprzed edycji.

Na razie użyj tylko tego samego kodu z programu obsługi zdarzeń `UpdateCommand` w temacie *Omówienie edytowania i usuwania danych w* samouczku elementu DataList. Dodamy kod, aby bezpiecznie obsłużyć wyjątki w kroku 2.

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample3.cs)]

W przypadku nieprawidłowych danych wejściowych, które mogą mieć postać nieprawidłowo sformatowanej ceny jednostkowej, niedozwolona wartość ceny jednostkowej, taka jak-$5,00, lub pominięcie nazwy produktu, zostanie zgłoszony wyjątek. Ponieważ program obsługi zdarzeń `UpdateCommand` nie zawiera żadnego kodu obsługi wyjątków w tym momencie, wyjątek będzie bąbelkowy do środowiska uruchomieniowego ASP.NET, gdzie będzie wyświetlany użytkownikowi końcowemu (zobacz rysunek 3).

![Gdy wystąpi nieobsługiwany wyjątek, użytkownik końcowy widzi stronę błędu](handling-bll-and-dal-level-exceptions-cs/_static/image7.png)

**Rysunek 3**. po wystąpieniu nieobsługiwanego wyjątku użytkownik końcowy widzi stronę błędu

## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Krok 2. bezpieczne obsługiwanie wyjątków w programie obsługi zdarzeń elementu UpdateCommand

Podczas aktualizowania przepływu pracy mogą wystąpić wyjątki w obsłudze zdarzeń `UpdateCommand`, LOGIKI biznesowej lub DAL. Na przykład jeśli użytkownik wprowadzi cenę zbyt kosztowną, instrukcja `Decimal.Parse` w programie obsługi zdarzeń `UpdateCommand` zgłosi wyjątek `FormatException`. Jeśli użytkownik pominie nazwę produktu lub jeśli cena ma wartość ujemną, DAL zgłosi wyjątek.

Gdy wystąpi wyjątek, chcemy wyświetlić komunikat informacyjny w obrębie samej strony. Dodaj kontrolkę sieci Web etykieta do strony, której `ID` jest ustawiona na `ExceptionDetails`. Skonfiguruj tekst etykieta s do wyświetlania na czerwono, bardzo dużą, pogrubioną i kursywną czcionką, przypisując jej Właściwość `CssClass` do `Warning`j klasy CSS, która jest zdefiniowana w pliku `Styles.css`.

Gdy wystąpi błąd, chcemy, aby etykieta była wyświetlana tylko raz. Oznacza to, że na kolejnych ogłaszaniu zwrotnych komunikat ostrzegawczy etykieta s powinien zniknąć. Można to osiągnąć przez wyczyszczenie właściwości etykieta s `Text` lub ustawienia jej właściwości `Visible`, aby `False` w obsłudze zdarzeń `Page_Load` (jak zostało to zrobione w samouczku [Obsługa wyjątków logiki biznesowej i dal na stronie ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) ) lub przez wyłączenie obsługi stanu widoku etykiet s. Niech s Użyj tej ostatniej opcji.

[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample4.aspx)]

Po podniesieniu wyjątku przypiszemy szczegóły wyjątku do właściwości `Text` formantu etykiety `ExceptionDetails`. Ze względu na to, że stan widoku jest wyłączony, po kolejnych ogłaszaniu zwrotnym zmiany programistyczne `Text` właściwości s zostaną utracone, przywracając z powrotem do domyślnego tekstu (pustego ciągu), ukrywając komunikat ostrzegawczy.

Aby określić, kiedy wystąpił błąd w celu wyświetlenia przydatnego komunikatu na stronie, musimy dodać blok `Try ... Catch` do programu obsługi zdarzeń `UpdateCommand`. Część `Try` zawiera kod, który może prowadzić do wyjątku, podczas gdy blok `Catch` zawiera kod, który jest wykonywany w celu wypróbowania wyjątku. Zapoznaj się z sekcją podstawowe informacje o [obsłudze wyjątków](https://msdn.microsoft.com/library/2w8f0bss.aspx) w dokumentacji .NET Framework, aby uzyskać więcej informacji na temat `Try ... Catch` bloku.

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample5.cs)]

Gdy wyjątek dowolnego typu jest zgłaszany przez kod w bloku `Try`, rozpocznie się wykonywanie kodu `Catch` blok s. Typ wyjątku, który jest generowany `DbException`, `NoNullAllowedException`, `ArgumentException`i tak dalej, zależy od tego, co dokładnie wytrącł błąd w pierwszym miejscu. Jeśli wystąpi problem na poziomie bazy danych, zostanie zgłoszony `DbException`. Jeśli zostanie wprowadzona niedozwolona wartość dla pól `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`lub `ReorderLevel`, zostanie zgłoszony `ArgumentException`, jak dodaliśmy kod w celu sprawdzenia poprawności tych wartości pól w klasie `ProductsDataTable` (zobacz samouczek [Tworzenie warstwy logiki biznesowej](../introduction/creating-a-business-logic-layer-cs.md) ).

Firma Microsoft może dostarczyć bardziej pomocne wyjaśnienie użytkownikowi końcowemu, opierając się na tekście komunikatu w typie przechwyconego wyjątku. Poniższy kod, który został użyty w prawie identycznym formularzu z [wyjątkiem obsługi wyjątków logiki biznesowej-i dal w samouczku strony ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) , zawiera ten poziom szczegółowości:

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample6.cs)]

Aby ukończyć ten samouczek, po prostu wywołaj metodę `DisplayExceptionDetails` z bloku `Catch`, przechodząc do wystąpienia `Exception` przechwycone (`ex`).

Po zablokowaniu `Try ... Catch` użytkownicy są wyświetlani z bardziej informacyjnym komunikatem o błędzie, tak jak rysunki 4 i 5. Należy pamiętać, że w przypadku wyjątku element DataList pozostaje w trybie edycji. Jest to spowodowane tym, że po wystąpieniu wyjątku przepływ sterowania zostanie natychmiast przekierowany do bloku `Catch`, pomijając kod, który zwraca wartość DataList do jego stanu sprzed edycji.

[![komunikat o błędzie jest wyświetlany, gdy użytkownik pomija wymagane pole](handling-bll-and-dal-level-exceptions-cs/_static/image9.png)](handling-bll-and-dal-level-exceptions-cs/_static/image8.png)

**Rysunek 4**. komunikat o błędzie jest wyświetlany, gdy użytkownik pomija wymagane pole ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-bll-and-dal-level-exceptions-cs/_static/image10.png))

[![komunikat o błędzie jest wyświetlany, gdy zostanie wyświetlona ujemna cena](handling-bll-and-dal-level-exceptions-cs/_static/image12.png)](handling-bll-and-dal-level-exceptions-cs/_static/image11.png)

**Rysunek 5**. komunikat o błędzie jest wyświetlany w przypadku podania ujemnej ceny ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](handling-bll-and-dal-level-exceptions-cs/_static/image13.png))

## <a name="summary"></a>Podsumowanie

Elementy GridView i ObjectDataSource zapewniają programy obsługi zdarzeń na poziomie, które zawierają informacje o wyjątkach, które zostały zgłoszone podczas przepływu pracy aktualizowania i usuwania, a także właściwości, które można ustawić, aby wskazać, czy wyjątek został działani. Te funkcje są jednak niedostępne podczas pracy z elementem DataList i bezpośrednio przy użyciu LOGIKI biznesowej. W zamian jest odpowiedzialny za implementację obsługi wyjątków.

W tym samouczku pokazano, jak dodać obsługę wyjątków do edytowalnego przepływu pracy aktualizacji typu DataList, dodając blok `Try ... Catch` do programu obsługi zdarzeń `UpdateCommand`. Jeśli wyjątek jest zgłaszany podczas aktualizowania przepływu pracy, wykonywany jest kod `Catch` blok s, wyświetlając przydatne informacje w etykiecie `ExceptionDetails`.

W tym momencie DataList nie podejmuje żadnych działań w celu zapobiegania wystąpieniu wyjątków w pierwszym miejscu. Mimo że wiadomo, że ujemna cena spowoduje wyjątek, nie Dodaliśmy jeszcze żadnych funkcji, aby uniemożliwić użytkownikowi wprowadzanie takich nieprawidłowych danych wejściowych. W naszym następnym samouczku dowiesz się, jak pomóc w zmniejszeniu wyjątków spowodowanych przez nieprawidłowe dane wprowadzane przez użytkownika przez dodanie kontrolek weryfikacji w `EditItemTemplate`.

Szczęśliwe programowanie!

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Wyjątki — zalecenia dotyczące projektowania](https://msdn.microsoft.com/library/ms298399.aspx)
- Błędy [rejestrowania modułów i programów obsługi (ELMAH)](http://workspaces.gotdotnet.com/elmah) (Biblioteka open source dla błędów rejestrowania)
- [Biblioteka Enterprise dla .NET Framework 2,0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (obejmuje blok aplikacji do zarządzania wyjątkami)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedmiu grup ASP/ASP. NET Books i założyciel of [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował z technologiami sieci Web firmy Microsoft od czasu 1998. Scott działa jako niezależny konsultant, trainer i składnik zapisywania. Jego Najnowsza książka to [*Sams ASP.NET 2,0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Można go osiągnąć w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem swojego blogu, który można znaleźć w [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania

Ta seria samouczków została sprawdzona przez wielu przydatnych recenzentów. Recenzent potencjalnych klientów dla tego samouczka został Krzysztof Pespisa. Chcesz przeglądać moje nadchodzące artykuły MSDN? Jeśli tak, upuść mi linię w [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](performing-batch-updates-cs.md)
> [dalej](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)

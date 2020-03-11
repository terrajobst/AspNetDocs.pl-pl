---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Obsługa relacji jednostek | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: be4948e5443a5eb4e1824c63dd0c445a7ee1928e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557464"
---
# <a name="handling-entity-relations"></a>Obsługa relacji jednostek

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://github.com/MikeWasson/BookService)

W tej sekcji opisano niektóre szczegóły dotyczące tego, jak dr ładuje powiązane jednostki i jak obsłużyć właściwości nawigacji cyklicznej w klasach modelu. (Ta sekcja zawiera informacje w tle i nie jest wymagana do ukończenia tego samouczka. Jeśli wolisz, przejdź do [części 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Ładowanie eager a ładowanie z opóźnieniem

W przypadku korzystania z programu EF z relacyjną bazą danych ważne jest, aby zrozumieć, jak dr ładuje powiązane dane.

Warto również zobaczyć zapytania SQL, które generuje program Dr. Aby śledzić SQL, Dodaj następujący wiersz kodu do konstruktora `BookServiceContext`:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Wysłanie żądania GET do/API/Books zwraca kod JSON podobny do następującego:

[!code-console[Main](part-4/samples/sample2.cmd)]

Można zobaczyć, że właściwość Author ma wartość null, mimo że książka zawiera prawidłowy AuthorId. To dlatego, że EF nie ładuje powiązanych jednostek autora. Dziennik śledzenia zapytania SQL potwierdza:

[!code-console[Main](part-4/samples/sample3.sql)]

Instrukcja SELECT pobiera z tabeli Books i nie odwołuje się do tabeli autora.

Poniżej przedstawiono metodę w klasie `BooksController`, która zwraca listę książek.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Zobaczmy, jak możemy zwrócić autora w ramach danych JSON. Istnieją trzy sposoby ładowania powiązanych danych w Entity Framework: ładowanie eager, ładowanie z opóźnieniem i jawne ładowanie. Każda Technika ma pewne kompromisy, dlatego ważne jest, aby zrozumieć, jak działają.

### <a name="eager-loading"></a>Ładowanie eager

W przypadku *ładowania eager*EF ładuje powiązane jednostki jako część początkowej kwerendy bazy danych. Aby przeprowadzić ładowanie eager, użyj metody rozszerzenia **System. Data. Entity. include** .

[!code-csharp[Main](part-4/samples/sample5.cs)]

To informuje Dr o uwzględnieniu danych autora w zapytaniu. Jeśli wprowadzisz tę zmianę i uruchomisz aplikację, teraz dane JSON wyglądają następująco:

[!code-console[Main](part-4/samples/sample6.cmd)]

Dziennik śledzenia pokazuje, że EF wykonał sprzężenie w tabelach książki i autora.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Ładowanie z opóźnieniem

W przypadku ładowania z opóźnieniem EF automatycznie ładuje powiązaną jednostkę, gdy właściwość nawigacji dla tej jednostki zostanie wykorzystana. Aby włączyć ładowanie z opóźnieniem, ustaw właściwość nawigacji na wirtualną. Na przykład w klasie Book:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Teraz Rozważmy następujący kod:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Gdy ładowanie z opóźnieniem jest włączone, uzyskanie dostępu do właściwości `Author` na `books[0]` powoduje, że program Dr będzie wysyłać zapytania do bazy danych dla autora.

Ładowanie z opóźnieniem wymaga przeprowadzenia wielu operacji związanych z bazą danych, ponieważ dr wysyła zapytanie przy każdym pobieraniu powiązanej jednostki. Ogólnie rzecz biorąc, ładowanie z opóźnieniem jest wyłączone dla obiektów, które można serializować. Serializator musi odczytać wszystkie właściwości modelu, które wyzwalają ładowanie powiązanych jednostek. Na przykład poniżej przedstawiono zapytania SQL, gdy EF serializować listę książek z włączonym ładowaniem z opóźnieniem. Można zobaczyć, że Dr wykonuje trzy osobne zapytania dla trzech autorów.

[!code-console[Main](part-4/samples/sample10.sql)]

W dalszym ciągu możesz chcieć użyć ładowania z opóźnieniem. Ładowanie eager może spowodować, że EF generuje bardzo złożone sprzężenia. Lub mogą być potrzebne powiązane jednostki dla małego podzestawu danych, a ładowanie z opóźnieniem będzie bardziej wydajne.

Jednym ze sposobów uniknięcia problemów serializacji jest Serializacja obiektów transferu danych (DTO) zamiast obiektów jednostek. Pokażę to podejście w dalszej części artykułu.

### <a name="explicit-loading"></a>Jawne ładowanie

Jawne ładowanie jest podobne do ładowania z opóźnieniem, z tą różnicą, że jawnie otrzymujesz powiązane dane w kodzie; nie odbywa się to automatycznie podczas uzyskiwania dostępu do właściwości nawigacji. Jawne ładowanie daje większą kontrolę nad tym, kiedy ładować powiązane dane, ale wymaga dodatkowego kodu. Aby uzyskać więcej informacji na temat bezpośredniego ładowania, zobacz [ładowanie powiązanych jednostek](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Właściwości nawigacji i odwołania cykliczne

Po zdefiniowaniu modelu książki i autora definiujełem właściwość nawigacji w klasie `Book` dla relacji Tworzenie książki, ale właściwość nawigacji nie została zdefiniowana w innym kierunku.

Co się stanie w przypadku dodania odpowiedniej właściwości nawigacji do klasy `Author`?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Niestety, spowoduje to utworzenie problemu podczas serializacji modeli. Jeśli załadujesz powiązane dane, tworzy cykliczny wykres obiektu.

![](part-4/_static/image1.png)

Gdy program formatujący JSON lub XML próbuje serializować grafu, zgłosi wyjątek. Dwa elementy formatującego generują różne komunikaty o wyjątkach. Oto przykład dla programu formatującego JSON:

[!code-console[Main](part-4/samples/sample12.cmd)]

Oto elementy formatujące XML:

[!code-xml[Main](part-4/samples/sample13.xml)]

Jednym z rozwiązań jest użycie DTO, które opisano w następnej sekcji. Alternatywnie można skonfigurować elementy formatujące JSON i XML do obsługi cykli wykresu. Aby uzyskać więcej informacji, zobacz [Obsługa cyklicznych odwołań do obiektów](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

W tym samouczku nie jest potrzebna właściwość nawigacji `Author.Book`, więc możesz ją pozostawić.

> [!div class="step-by-step"]
> [Poprzednie](part-3.md)
> [dalej](part-5.md)

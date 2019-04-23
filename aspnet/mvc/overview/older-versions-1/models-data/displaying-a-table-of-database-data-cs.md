---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Wyświetlanie tabeli danych bazy danych (C#) | Dokumentacja firmy Microsoft
author: microsoft
description: W tym samouczku pokazują I, wyświetlany jest zestaw rekordów bazy danych na dwa sposoby. Czy mogę Pokaż dwie metody formatowania zestaw rekordów bazy danych w formacie HTML danych...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 99b18de33e266adb626f4ab53ff20b1f52102900
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59417588"
---
# <a name="displaying-a-table-of-database-data-c"></a>Wyświetlanie tabeli danych bazy danych (C#)

przez [firmy Microsoft](https://github.com/microsoft)

[Pobierz plik PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> W tym samouczku pokazują I, wyświetlany jest zestaw rekordów bazy danych na dwa sposoby. Czy mogę wyświetlić dwie metody formatowania zestaw rekordów bazy danych w tabeli HTML. Po pierwsze I pokazują, jak można sformatować rekordów bazy danych bezpośrednio z poziomu widoku. Następnie I pokazują, jak można korzystać z zalet częściowych podczas formatowania rekordów bazy danych.


Celem tego samouczka jest wyjaśniają, jak można wyświetlić tabeli HTML danych bazy danych w aplikacji ASP.NET MVC. Po pierwsze dowiesz się, jak używać narzędzia do tworzenia szkieletów zawarte w Visual Studio do generowania widoku, który automatycznie wyświetla zestaw rekordów. Następnie dowiesz się, jak używać częściowym jako szablon, podczas formatowania rekordów bazy danych.

## <a name="create-the-model-classes"></a>Tworzenie klas modelu

Firma Microsoft zamierza wyświetlić zestaw rekordów z tabeli bazy danych filmów. Tabela bazy danych filmów zawiera następujące kolumny:

<a id="0.3_table01"></a>


| **Nazwa kolumny** | **Typ danych** | **Zezwalaj na wartości null** |
| --- | --- | --- |
| Id | int | False |
| Tytuł | Nvarchar(200) | False |
| Dyrektor ds. | NVarchar(50) | False |
| DateReleased | DataGodzina | False |


Aby móc przedstawić w tabeli filmów w naszej aplikacji ASP.NET MVC, należy utworzyć klasę modelu. W tym samouczku używamy Microsoft Entity Framework do tworzenia klas w naszym modelu.

> [!NOTE] 
> 
> W tym samouczku używamy Microsoft Entity Framework. Jednak ważne jest zrozumienie, czy można użyć szereg różnych technologie do interakcji z bazą danych z aplikacji ASP.NET MVC, w tym LINQ to SQL i NHibernate, ADO.NET.


Wykonaj następujące kroki, aby uruchomić Kreator modelu danych jednostki:

1. Kliknij prawym przyciskiem myszy folderu modeli w oknie Eksploratora rozwiązań i wybierz opcję menu **Dodaj, nowy element**.
2. Wybierz **danych** kategorii, a następnie wybierz **ADO.NET Entity Data Model** szablonu.
3. Nadaj nazwę modelu danych *MoviesDBModel.edmx* i kliknij przycisk **Dodaj** przycisku.

Po kliknięciu przycisku Dodaj zostanie wyświetlony Kreator modelu Entity Data Model (patrz rysunek 1). Wykonaj następujące kroki, aby zakończyć działanie kreatora:

1. W **wybierz zawartość modelu** kroku, wybierz pozycję **Generuj z bazy danych** opcji.
2. W **wybierz połączenie danych** kroku, należy użyć *MoviesDB.mdf* połączenia danych i nazwę *MoviesDBEntities* dla ustawień połączenia. Kliknij przycisk **dalej** przycisku.
3. W **wybierz obiekty bazy danych** kroku, rozwiń węzeł tabele, wybierz tabelę filmów. Wprowadź przestrzeń nazw *modeli* i kliknij przycisk **Zakończ** przycisku.


[![Tworzenie zapytań LINQ do klas SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)

**Rysunek 01**: Tworzenie zapytań LINQ do klas SQL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image2.png))


Po zakończeniu działania Kreator modelu Entity Data Model, zostanie otwarty projektant modelu danych jednostki. Projektant powinien być wyświetlany jednostki filmy (patrz rysunek 2).


[![Projektant modelu danych jednostki](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)

**Rysunek 02**: Projektant modelu danych jednostki ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image4.png))


Należy wprowadzić zmianę jeden, przed kontynuowaniem. Kreator modelu Entity Data generuje klasę modelu o nazwie *filmy* reprezentujący tabelę bazy danych filmów. Ponieważ będziemy korzystać do reprezentowania filmu konkretnej klasy filmy, należy zmodyfikować nazwę klasy, która ma być *filmu* zamiast *filmy* (pojedynczej zamiast liczba mnoga).

Kliknij dwukrotnie nazwę klasy na powierzchni projektowej i Zmień nazwę klasy z filmy filmu. Po wprowadzeniu tej zmiany, kliknij przycisk **Zapisz** przycisk (ikona dysku), aby wygenerować klasę filmu.

## <a name="create-the-movies-controller"></a>Tworzenie kontrolera filmy

Teraz, gdy mamy już sposobem reprezentowania naszych danych bazy danych, możemy utworzyć kontroler, który zwraca kolekcję filmów. W oknie Eksploratora rozwiązań w usłudze Visual Studio kliknij prawym przyciskiem myszy folder kontrolerów, a następnie wybierz opcję menu **Dodaj, kontroler** (zobacz rysunek 3).


[![Dodawanie kontrolera Menu](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)

**Rysunek 03**: Dodawanie kontrolera Menu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image6.png))


Gdy **Dodaj kontroler** zostanie wyświetlone okno dialogowe, wprowadź nazwę kontrolera MovieController (zobacz rysunek 4). Kliknij przycisk **Dodaj** przycisk, aby dodać nowy kontroler.


[![Okno dialogowe Dodawanie kontrolera](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)

**Rysunek 04**: Okno dialogowe Dodawanie kontrolera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image8.png))


Należy zmodyfikować akcję indeks() udostępnianych przez kontroler filmu, tak aby zwraca zestaw rekordów bazy danych. Tak, aby wyglądało kontrolera w ofercie 1, należy zmodyfikować kontrolera.

**Wyświetlanie listy 1 – Controllers\MovieController.cs**

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

W ofercie 1 klasa MoviesDBEntities jest używana do reprezentowania MoviesDB bazy danych. Aby użyć tej klasy, należy zaimportować obszar nazw MvcApplication1.Models następująco:

using MvcApplication1.Models;

Wyrażenie *jednostek. MovieSet.ToList()* zwraca zestaw wszystkich filmów z tabeli bazy danych filmów.

## <a name="create-the-view"></a>Tworzenie widoku

Najprostszym sposobem wyświetlania zestawu rekordów bazy danych w tabeli HTML jest korzystanie z zalet tworzenia szkieletów udostępniane przez program Visual Studio.

Kompiluj aplikację, wybierając opcję menu **twórz, Kompiluj rozwiązanie**. Należy skompilować aplikację przed otwarciem **Dodaj widok** okna dialogowego lub klas usługi danych nie będzie wyświetlane w oknie dialogowym.

Kliknij prawym przyciskiem myszy działanie indeks() i wybierz opcję menu **Dodaj widok** (zobacz rysunek 5).


[![Dodawanie widoku](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)

**Rysunek 05**: Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image10.png))


W **Dodaj widok** okno dialogowe, zaznacz pola wyboru **utworzyć widok silnie typizowane**. Wybierz klasę filmu jako **wyświetlić klasy danych**. Wybierz *listy* jako **wyświetlanie zawartości** (patrz rysunek 6). Zaznaczenie tych opcji spowoduje wygenerowanie silnie typizowane widoku, który wyświetla listę filmów.


[![Okno dialogowe dodawania widoku](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)

**Rysunek 06**: Okno dialogowe Dodawanie widoku ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image12.png))


Po kliknięciu **Dodaj** przycisk, widok w ofercie 2 jest generowany automatycznie. Ten widok zawiera kod wymagany do iterowania po kolekcji filmów i wyświetlane poszczególne właściwości filmu.

**Wyświetlanie listy 2 — Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

Możesz uruchomić aplikację, wybierając opcję menu **debugowania i Rozpocznij debugowanie** (lub naciskając klawisz F5). Uruchomiona jest aplikacja uruchomi program Internet Explorer. Jeśli przejdziesz do adresu URL /Movie, a następnie zobaczysz stronę na rysunku 7.


[![Tabelę filmy](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)

**Rysunek 07**: Tabela filmów ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](displaying-a-table-of-database-data-cs/_static/image14.png))


Jeśli nie potrzebujesz nic o wyglądzie siatki rekordów bazy danych na rysunku 7 można po prostu zmodyfikuj widok indeksu. Na przykład można zmienić *DateReleased* nagłówka do *Data wydania* przez zmodyfikowanie widoku indeksu.

## <a name="create-a-template-with-a-partial"></a>Tworzenie szablonu z częściowym

Jeśli widok pobiera zbyt skomplikowane, jest dobry pomysł, aby uruchomić podzielenie widoku na częściowych. Za pomocą częściowych ułatwia widoków do zrozumienia i utrzymania. Utworzymy częściowym, możemy użyć jako szablonu, aby sformatować listę rekordów bazy danych filmów.

Wykonaj następujące kroki, aby utworzyć częściowego:

1. Kliknij prawym przyciskiem myszy Views\Movie folder i wybierz opcję menu **Dodaj widok**.
2. Zaznacz pola wyboru *tworzenia widoku częściowego (.ascx)*.
3. Nadaj nazwę częściowego *MovieTemplate*.
4. Zaznacz pola wyboru **utworzyć widok silnie typizowane**.
5. Wybierz filmu jako *wyświetlić klasy danych*.
6. Wybierz pusty jako *wyświetlanie zawartości*.
7. Kliknij przycisk **Dodaj** przycisk, aby dodać części do projektu.

Po wykonaniu tych kroków, należy zmodyfikować MovieTemplate częściowe wyglądać lista 3.

**Listing 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

Partial w ofercie 3 zawiera szablon w pojedynczym wierszu rekordów.

Zmodyfikowany widok indeksu w ofercie 4 używa MovieTemplate częściowe.

**Wyświetlanie listy 4 — Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

Widok w ofercie 4 zawiera pętlę foreach, która wykonuje iterację przez wszystkie filmy. Dla każdego filmu częściowe MovieTemplate jest używany do formatowania filmu. MovieTemplate jest renderowany przez wywołanie metody pomocnika RenderPartial().

Zmodyfikowany widok indeksu powoduje wyświetlenie tej samej tabeli HTML rekordów bazy danych. Jednak widok został znacznie uproszczony.


Metoda RenderPartial() jest inne niż większość pozostałych metod pomocniczych, ponieważ nie zwraca ciąg. W związku z tym, należy wywołać przy użyciu metody RenderPartial() &lt;% Html.RenderPartial(); %&gt; zamiast &lt;% = Html.RenderPartial(); %&gt;.


## <a name="summary"></a>Podsumowanie

Celem tego samouczka było wyjaśniają, jak można wyświetlić zestaw rekordów bazy danych w tabeli HTML. Po pierwsze wiesz, jak można zwrócić zestaw rekordów bazy danych w ramach akcji kontrolera, wykorzystując Microsoft Entity Framework. Następnie omówiono na potrzeby tworzenia szkieletów programu Visual Studio wygenerowania widoku, który automatycznie wyświetla zbiór elementów. Na koniec pokazaliśmy ci, jak uprościć widok, korzystając z częściowego. Przedstawiono sposób użycia częściowym jako szablon, dzięki czemu można formatować każdy rekord bazy danych.

> [!div class="step-by-step"]
> [Poprzednie](creating-model-classes-with-linq-to-sql-cs.md)
> [dalej](performing-simple-validation-cs.md)

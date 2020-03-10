---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Wywoływanie interfejsu API sieci Web z aplikacji Windows PhoneC#8 () — ASP.NET 4. x
author: rmcmurray
description: 'Samouczek z kodem: Tworzenie aplikacji internetowego interfejsu API ASP.NET w ASP.NET 4. x, która udostępnia Katalog książek w aplikacji Windows Phone 8.'
ms.author: riande
ms.date: 10/09/2013
ms.custom: seoapril2019
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: c5da14a6856f551343b6fb14f0aedc659e792f6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614738"
---
# <a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Wywoływanie wzorca Web API z aplikacji Windows Phone 8 (C#)

Autorzy [Robert McMurraya](https://github.com/rmcmurray)

W tym samouczku dowiesz się, jak utworzyć kompletny scenariusz kompleksowy składający się z aplikacji internetowego interfejsu API ASP.NET, która udostępnia Katalog książek do aplikacji Windows Phone 8.

### <a name="overview"></a>Omówienie

Usługi RESTful Services, takie jak ASP.NET Web API, upraszczają tworzenie aplikacji opartych na protokole HTTP dla deweloperów przez abstrakcję architektury dla aplikacji po stronie serwera i klienta. Zamiast tworzenia własnościowego protokołu opartego na gnieździe w celu komunikacji deweloperzy interfejsu API sieci Web po prostu muszą opublikować wymagane metody HTTP dla swojej aplikacji (na przykład: GET, POST, PUT, DELETE) i deweloperzy aplikacji klienckich muszą używać tylko Metody HTTP, które są niezbędne dla swojej aplikacji.

W tym kompleksowym samouczku dowiesz się, jak za pomocą interfejsu API sieci Web utworzyć następujące projekty:

- W [pierwszej części tego samouczka](#STEP1)utworzysz aplikację internetowego interfejsu API ASP.NET, która obsługuje wszystkie operacje tworzenia, odczytu, aktualizacji i usuwania (CRUD) w celu zarządzania katalogiem książek. Ta aplikacja będzie używać [przykładowego pliku XML (Books. xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) w witrynie MSDN.
- W [drugiej części tego samouczka](#STEP2)utworzysz interaktywną aplikację Windows Phone 8, która pobiera dane z aplikacji internetowego interfejsu API.

#### <a name="prerequisites"></a>Wymagania wstępne

- Visual Studio 2013 z zainstalowanym zestawem SDK Windows Phone 8
- System Windows 8 lub nowszy w systemie 64-bitowym z zainstalowaną funkcją Hyper-V
- Aby uzyskać listę dodatkowych wymagań, zobacz sekcję *wymagania systemowe* na stronie pobierania [zestawu Windows Phone SDK 8,0](https://www.microsoft.com/download/details.aspx?id=35471) .

> [!NOTE]
> Jeśli zamierzasz przetestować łączność między interfejsem API sieci Web i Windows Phone 8 projektów w systemie lokalnym, musisz postępować zgodnie z instrukcjami zawartymi w temacie *[łączenie emulatora Windows Phone 8 z aplikacjami interfejsu API sieci Web na komputerze lokalnym](https://go.microsoft.com/fwlink/?LinkId=324014)* w celu skonfigurowania środowiska testowego.

<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Krok 1. Tworzenie projektu internetowego interfejsu API księgarni

Pierwszym krokiem tego kompleksowego samouczka jest utworzenie projektu interfejsu API sieci Web, który obsługuje wszystkie operacje CRUD; należy pamiętać, że w [kroku 2](#STEP2) tego samouczka dodasz projekt aplikacji Windows Phone do tego rozwiązania.

1. Otwórz **Visual Studio 2013**.
2. Kliknij kolejno pozycje **plik**, **Nowy**i **projekt**.
3. Po wyświetleniu okna dialogowego **Nowy projekt** rozwiń węzeł **zainstalowane**, a następnie pozycję **Szablony**, a następnie pozycję **Wizualizacja C#** , a następnie kliknij pozycję **Sieć Web**.

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Kliknij obraz, aby rozwinąć                                                                |

4. Wyróżnij **aplikację sieci Web ASP.NET**, wprowadź **księgarni** dla nazwy projektu, a następnie kliknij przycisk **OK**.
5. Gdy zostanie wyświetlone okno dialogowe **Nowy projekt ASP.NET** , wybierz szablon **internetowego interfejsu API** , a następnie kliknij przycisk **OK**.

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Kliknij obraz, aby rozwinąć                                                                |

6. Po otwarciu projektu interfejsu API sieci Web Usuń przykładowy kontroler z projektu:

    1. Rozwiń folder **controllers** w Eksploratorze rozwiązań.
    2. Kliknij prawym przyciskiem myszy plik **ValuesController.cs** , a następnie kliknij polecenie **Usuń**.
    3. Kliknij przycisk **OK** po wyświetleniu monitu o potwierdzenie usunięcia.
7. Dodaj plik danych XML do projektu interfejsu API sieci Web; Ten plik zawiera zawartość wykazu księgarni:

   1. Kliknij prawym przyciskiem myszy folder **\_danych aplikacji** w Eksploratorze rozwiązań, a następnie kliknij pozycję **Dodaj**, a następnie kliknij pozycję **nowy element**.
   2. Gdy zostanie wyświetlone okno dialogowe **Dodaj nowy element** , zaznacz szablon **pliku XML** .
   3. Nazwij plik **Books. XML**, a następnie kliknij przycisk **Dodaj**.
   4. Po otwarciu pliku **Books. XML** Zastąp kod w pliku XML z przykładowego pliku **Books. XML** w witrynie MSDN: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. Zapisz i zamknij plik XML.

8. Dodaj model księgarni do projektu interfejsu API sieci Web; Ten model zawiera logikę tworzenia, odczytu, aktualizacji i usuwania (CRUD) dla aplikacji księgarni:

   1. Kliknij prawym przyciskiem myszy folder **modele** w Eksploratorze rozwiązań, a następnie kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Klasa**.
   2. Gdy zostanie wyświetlone okno dialogowe **Dodaj nowy element** , Nazwij plik klasy **BookDetails.cs**, a następnie kliknij przycisk **Dodaj**.
   3. Po otwarciu pliku **BookDetails.cs** Zastąp kod w pliku następującym: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Zapisz i zamknij plik **BookDetails.cs** .

9. Dodaj kontroler księgarni do projektu interfejsu API sieci Web:

   1. Kliknij prawym przyciskiem myszy folder **controllers** w Eksploratorze rozwiązań, a następnie kliknij pozycję **Dodaj**, a następnie kliknij pozycję **kontroler**.
   2. Po wyświetleniu okna dialogowego **Dodawanie szkieletu** zaznacz opcję **kontroler Web API 2 — pusty**, a następnie kliknij przycisk **Dodaj**.
   3. Gdy zostanie wyświetlone okno dialogowe **Dodaj kontroler** , nazwij kontroler **BooksController**, a następnie kliknij przycisk **Dodaj**.
   4. Po otwarciu pliku **BooksController.cs** Zastąp kod w pliku następującym: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Zapisz i zamknij plik **BooksController.cs** .

10. Utwórz aplikację interfejsu API sieci Web, aby sprawdzić pod kątem błędów.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Krok 2. Dodawanie projektu katalogu Windows Phone 8 księgarni

Następnym krokiem tego kompleksowego scenariusza jest utworzenie aplikacji katalogu dla Windows Phone 8. Ta aplikacja będzie używać szablonu *aplikacji Windows Phone* danych dla domyślnego interfejsu użytkownika i będzie używać aplikacji internetowego interfejsu API, która została utworzona w [kroku 1](#STEP1) tego samouczka jako źródła danych.

1. Kliknij prawym przyciskiem myszy rozwiązanie **księgarni** w Eksploratorze rozwiązań, a następnie kliknij przycisk **Dodaj**, a następnie **Nowy projekt**.
2. Po wyświetleniu okna dialogowego **Nowy projekt** rozwiń węzeł **zainstalowane**, a następnie **pozycję C#Wizualizacja** , a następnie **Windows Phone**.
3. Wyróżnij **Windows Phone aplikacji powiązanej z danymi**, wprowadź **BookCatalog** jako nazwę, a następnie kliknij przycisk **OK**.
4. Dodaj pakiet NuGet Json.NET do projektu **BookCatalog** :

    1. Kliknij prawym przyciskiem myszy pozycję **odwołania** dla projektu **BookCatalog** w Eksploratorze rozwiązań, a następnie kliknij polecenie **Zarządzaj pakietami NuGet**.
    2. Gdy zostanie wyświetlone okno dialogowe **Zarządzanie pakietami NuGet** , rozwiń sekcję **online** i podświetl pozycję **NuGet.org**.
    3. Wprowadź **JSON.NET** w polu wyszukiwania, a następnie kliknij ikonę wyszukiwania.
    4. Zaznacz **JSON.NET** w wynikach wyszukiwania, a następnie kliknij przycisk **Instaluj**.
    5. Po zakończeniu instalacji kliknij przycisk **Zamknij**.
5. Dodaj model **BookDetails** do projektu **BookCatalog** ; zawiera ogólny model klasy księgarni:

   1. Kliknij prawym przyciskiem myszy projekt **BookCatalog** w Eksploratorze rozwiązań, a następnie kliknij pozycję **Dodaj**, a następnie kliknij pozycję **Nowy folder**.
   2. Nazwij nowe **modele**folderów.
   3. Kliknij prawym przyciskiem myszy folder **modele** w Eksploratorze rozwiązań, a następnie kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Klasa**.
   4. Gdy zostanie wyświetlone okno dialogowe **Dodaj nowy element** , Nazwij plik klasy **BookDetails.cs**, a następnie kliknij przycisk **Dodaj**.
   5. Po otwarciu pliku **BookDetails.cs** Zastąp kod w pliku następującym: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Zapisz i zamknij plik **BookDetails.cs** .

6. Zaktualizuj klasę **MainViewModel.cs** , aby obejmowała funkcję komunikacji z aplikacją internetowego interfejsu API księgarni:

   1. Rozwiń folder **modele widoków** w Eksploratorze rozwiązań, a następnie kliknij dwukrotnie plik **MainViewModel.cs** .
   2. Po otwarciu pliku **MainViewModel.cs** Zastąp kod w pliku następującym poleceniem: należy pamiętać, że należy zaktualizować wartość stałej `apiUrl` z rzeczywistym adresem URL internetowego interfejsu API: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Zapisz i zamknij plik **MainViewModel.cs** .

7. Zaktualizuj plik **MainPage. XAML** , aby dostosować nazwę aplikacji:

   1. Kliknij dwukrotnie plik **MainPage. XAML** w Eksploratorze rozwiązań.
   2. Po otwarciu pliku **MainPage. XAML** Znajdź następujące wiersze kodu: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Zamień te wiersze na następujące: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Zapisz i zamknij plik **MainPage. XAML** .

8. Zaktualizuj plik **DetailsPage. XAML** , aby dostosować wyświetlane elementy:

   1. Kliknij dwukrotnie plik **DetailsPage. XAML** w Eksploratorze rozwiązań.
   2. Po otwarciu pliku **DetailsPage. XAML** Znajdź następujące wiersze kodu: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Zamień te wiersze na następujące: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Zapisz i zamknij plik **DetailsPage. XAML** .

9. Skompiluj aplikację Windows Phone, aby wyszukać błędy.

### <a name="step-3-testing-the-end-to-end-solution"></a>Krok 3. Testowanie kompleksowego rozwiązania

Jak wspomniano w sekcji *wymagania wstępne* w tym samouczku, podczas testowania łączności między interfejsem API sieci web i Windows Phone 8 projektów w systemie lokalnym, należy postępować zgodnie z instrukcjami podanymi w temacie *[łączenie emulatora systemu Windows Phone 8 z aplikacjami interfejsu API sieci Web na komputerze lokalnym](https://go.microsoft.com/fwlink/?LinkId=324014)* w celu skonfigurowania środowiska testowego.

Po skonfigurowaniu środowiska testowego należy ustawić aplikację Windows Phone jako projekt startowy. Aby to zrobić, zaznacz aplikację **BookCatalog** w Eksploratorze rozwiązań, a następnie kliknij pozycję **Ustaw jako projekt startowy**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Kliknij obraz, aby rozwinąć |

Po naciśnięciu klawisza F5 program Visual Studio uruchomi emulator Windows Phone, który wyświetli &quot;, poczekaj&quot; komunikat, gdy dane aplikacji zostaną pobrane z internetowego interfejsu API:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Kliknij obraz, aby rozwinąć |

Jeśli wszystko powiedzie się, powinien pojawić się wyświetlony wykaz:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Kliknij obraz, aby rozwinąć |

W przypadku wybrania dowolnego tytułu książki aplikacja wyświetli opis książki:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Kliknij obraz, aby rozwinąć |

Jeśli aplikacja nie może komunikować się z interfejsem API sieci Web, zostanie wyświetlony komunikat o błędzie:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Kliknij obraz, aby rozwinąć |

Jeśli naciśniesz komunikat o błędzie, zostaną wyświetlone wszystkie dodatkowe szczegóły dotyczące błędu:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Kliknij obraz, aby rozwinąć                                                                 |

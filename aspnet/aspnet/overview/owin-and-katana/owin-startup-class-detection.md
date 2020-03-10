---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Wykrywanie klasy startowej OWIN | Microsoft Docs
author: Praburaj
description: W tym samouczku pokazano, jak skonfigurować klasę uruchomieniową OWIN. Aby uzyskać więcej informacji na temat OWIN, zobacz Omówienie projektu Katana. Ten samouczek został...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: e1670c32049ec33dd4d1941a091a429d3929c452
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617048"
---
# <a name="owin-startup-class-detection"></a>Wykrywanie klasy początkowej interfejsu OWIN

> W tym samouczku pokazano, jak skonfigurować klasę uruchomieniową OWIN. Aby uzyskać więcej informacji na temat OWIN, zobacz [Omówienie projektu Katana](an-overview-of-project-katana.md). Ten samouczek został utworzony przez Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan i Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ).
>
> ## <a name="prerequisites"></a>Wymagania wstępne
>
> [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)

## <a name="owin-startup-class-detection"></a>Wykrywanie klasy początkowej interfejsu OWIN

 Każda aplikacja OWIN ma klasę uruchomieniową, w której można określić składniki dla potoku aplikacji. Istnieją różne sposoby łączenia klasy uruchamiania z środowiskiem uruchomieniowym w zależności od wybranego modelu hostingu (OwinHost, IIS i IIS-Express). Klasa startowa wyświetlana w tym samouczku może być używana w każdej aplikacji hostingowej. Klasę uruchomieniową można połączyć z środowiskiem uruchomieniowym hostingu przy użyciu jednej z następujących metod:

1. **Konwencja nazewnictwa**: Katana szuka klasy o nazwie `Startup` w przestrzeni nazw zgodnej z nazwą zestawu lub globalną przestrzenią nazw.
2. **OwinStartup — atrybut**: jest to podejście większości deweloperów do określenia klasy uruchamiania. Następujący atrybut ustawi klasę uruchomieniową na klasę `TestStartup` w przestrzeni nazw `StartupDemo`.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   Atrybut `OwinStartup` zastępuje konwencję nazewnictwa. Można również określić przyjazną nazwę z tym atrybutem, jednak użycie przyjaznej nazwy wymaga również użycia elementu `appSetting` w pliku konfiguracji.
3. **Element element appSetting w pliku konfiguracji**: element `appSetting` zastępuje atrybut `OwinStartup` i konwencję nazewnictwa. Można mieć wiele klas uruchamiania (z których każdy korzysta z `OwinStartup` atrybutu) i skonfigurować klasę uruchomieniową, która zostanie załadowana w pliku konfiguracji przy użyciu znacznika podobnego do poniższego:

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   Następujący klucz, który jawnie określa klasę startową i zestaw, można również użyć:

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   Poniższy kod XML w pliku konfiguracji określa przyjazną nazwę klasy startowej `ProductionConfiguration`.

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   Powyższe oznakowanie musi być używane z poniższym atrybutem `OwinStartup`, który określa przyjazną nazwę i powoduje uruchomienie klasy `ProductionStartup2`.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Aby wyłączyć odnajdywanie uruchamiania OWIN Dodaj `appSetting owin:AutomaticAppStartup` z wartością `"false"` w pliku Web. config.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Tworzenie aplikacji sieci Web ASP.NET przy użyciu uruchamiania programu OWIN

1. Utwórz pustą aplikację sieci Web Asp.Net i nadaj jej nazwę **StartupDemo**. -Zainstaluj `Microsoft.Owin.Host.SystemWeb` przy użyciu Menedżera pakietów NuGet. W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet**, a następnie **konsolę Menedżera pakietów**. Wprowadź następujące polecenie:

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. Dodaj klasę uruchomieniową OWIN. W programie Visual Studio 2017 kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Dodaj klasę**. w oknie dialogowym **Dodaj nowy element** wprowadź *Owin* w polu wyszukiwania, a następnie zmień nazwę na Startup.cs, a następnie wybierz pozycję **Dodaj**.

     ![](owin-startup-class-detection/_static/image1.png)

   Następnym razem, gdy chcesz dodać *klasę uruchomieniową Owin*, będzie ona dostępna w menu **Dodaj** .

     ![](owin-startup-class-detection/_static/image2.png)

   Alternatywnie możesz kliknąć prawym przyciskiem myszy projekt i wybrać polecenie **Dodaj**, a następnie wybrać pozycję **nowy element**, a następnie wybrać **klasę uruchomieniową Owin**.

     ![](owin-startup-class-detection/_static/image3.png)

- Zastąp wygenerowany kod w pliku *Startup.cs* następującym:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  `app.Use` wyrażenie lambda służy do rejestrowania określonego składnika pośredniczącego w potoku OWIN. W takim przypadku konfigurujemy Rejestrowanie żądań przychodzących przed odpowiedzią na żądanie przychodzące. Parametr `next` jest delegatem ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [zadanie](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt;) do następnego składnika w potoku. `app.Run` wyrażenie lambda przechwytuje potok do żądań przychodzących i udostępnia mechanizm odpowiedzi.
     > [!NOTE]
     > W powyższym kodzie odnosimy się do atrybutu `OwinStartup` i opieramy się na Konwencji uruchamiania klasy o nazwie `Startup`.-Naciśnij klawisz ***F5*** , aby uruchomić aplikację. Wielokrotnie Odświeżaj.

    ![](owin-startup-class-detection/_static/image4.png) Uwaga: Liczba wyświetlana w obrazach w tym samouczku nie jest zgodna z liczbą wyświetlaną. Ciąg milisekundy jest używany do wyświetlania nowej odpowiedzi po odświeżeniu strony.
  Informacje o śledzeniu można wyświetlić w oknie **danych wyjściowych** .

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Dodaj więcej klas uruchamiania

W tej sekcji dodamy kolejną klasę uruchomieniową. Do aplikacji można dodać wiele klas uruchomieniowych OWIN. Można na przykład utworzyć klasy uruchomieniowe do projektowania, testowania i produkcji.

1. Utwórz nową klasę uruchomieniową OWIN i nadaj jej nazwę `ProductionStartup`.
2. Zastąp wygenerowany kod następującym:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Naciśnij klawisz CONTROL F5, aby uruchomić aplikację. Atrybut `OwinStartup` określa, że produkcja jest uruchamiana.

    ![](owin-startup-class-detection/_static/image6.png)
4. Utwórz kolejną klasę uruchomieniową OWIN i nadaj jej nazwę `TestStartup`.
5. Zastąp wygenerowany kod następującym:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   Przeciążanie atrybutu `OwinStartup` powyżej określa `TestingConfiguration` jako *przyjazną* nazwę klasy uruchomieniowej.
6. Otwórz plik *Web. config* i Dodaj klucz uruchomienia aplikacji Owin, który określa przyjazną nazwę klasy startowej:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Naciśnij klawisz CONTROL F5, aby uruchomić aplikację. Element ustawień aplikacji przyjmuje poprzednią wartość, a Konfiguracja testu jest uruchamiana.

    ![](owin-startup-class-detection/_static/image7.png)
8. Usuń *przyjazną* nazwę z atrybutu `OwinStartup` w klasie `TestStartup`.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Zastąp klucz uruchomienia aplikacji OWIN w pliku *Web. config* następującym:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Przywróć atrybut `OwinStartup` w każdej klasie do domyślnego kodu atrybutu wygenerowanego przez program Visual Studio:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    Każdy z kluczy uruchamiania aplikacji OWIN poniżej spowoduje uruchomienie klasy produkcyjnej.

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    Ostatni klucz uruchomienia określa metodę konfiguracji uruchamiania. Następujący klucz uruchomienia aplikacji OWIN umożliwia zmianę nazwy klasy konfiguracji na `MyConfiguration`.

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Korzystanie z Owinhost. exe

1. Zastąp plik Web. config następującym znacznikiem:

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   Ostatni klucz usługi WINS, więc w tym przypadku `TestStartup` jest określony.
2. Zainstaluj Owinhost z PMC:

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Przejdź do folderu aplikacji (folderu zawierającego plik *Web. config* ) i w wierszu polecenia wpisz:

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   Zostanie wyświetlone okno polecenia:

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Uruchom przeglądarkę z adresem URL `http://localhost:5000/`.

    ![](owin-startup-class-detection/_static/image8.png)

   OwinHost zostały uznane za wymienione powyżej konwencje uruchamiania.
5. W oknie wiersza polecenia naciśnij klawisz ENTER, aby zakończyć OwinHost.
6. W klasie `ProductionStartup` Dodaj następujący atrybut OwinStartup, który określa przyjazną nazwę *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. W wierszu polecenia wpisz:

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   Załadowana jest Klasa startowa produkcji.
    ![](owin-startup-class-detection/_static/image9.png)

   Nasza aplikacja ma wiele klas startowych, a w tym przykładzie odroczono klasę uruchomieniową do załadowania do czasu wykonania.
8. Przetestuj następujące opcje uruchamiania środowiska uruchomieniowego:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]

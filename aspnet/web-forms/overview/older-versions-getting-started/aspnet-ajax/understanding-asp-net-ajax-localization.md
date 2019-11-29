---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Zrozumienie lokalizacji ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Lokalizacja to proces projektowania i integrowania pomocy technicznej dla określonego języka i kultury w aplikacji lub składniku aplikacji. Mikrofon...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 003e7939accd7a68dab97441b3d999bca835b85a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600861"
---
# <a name="understanding-aspnet-ajax-localization"></a>Objaśnienie lokalizacji kodu ASP.NET AJAX

przez [Scott Cate](https://github.com/scottcate)

[Pobierz plik PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> Lokalizacja to proces projektowania i integrowania pomocy technicznej dla określonego języka i kultury w aplikacji lub składniku aplikacji. Platforma Microsoft ASP.NET zapewnia szeroką obsługę lokalizacji dla standardowych aplikacji ASP.NET przez integrację standardowego modelu lokalizacji .NET; Microsoft AJAX Framework korzysta ze zintegrowanego modelu do obsługi różnorodnych scenariuszy, w których można przeprowadzić lokalizację.

## <a name="introduction"></a>Wprowadzenie

Technologia ASP.NET firmy Microsoft zapewnia model programowania zorientowany obiektowo i oparty na zdarzeniach oraz kieruje go do zalet skompilowanego kodu. Jednak model przetwarzania po stronie serwera ma kilka wad związanych z technologią, z których wiele może być rozwiązywana przez nowe funkcje zawarte w przestrzeni nazw System. Web. Extensions, które hermetyzują usługi Microsoft AJAX w .NET Framework 3,5. Te rozszerzenia umożliwiają obsługę wielu bogatych funkcji klienta, wcześniej dostępnych jako część rozszerzeń ASP.NET 2,0 AJAX, ale teraz jest częścią biblioteki klas podstawowych platformy. Kontrolki i funkcje w tej przestrzeni nazw obejmują częściowe renderowanie stron bez konieczności odświeżania pełnych stron, możliwość uzyskiwania dostępu do usług sieci Web za pośrednictwem skryptu klienta (w tym interfejsu API profilowania ASP.NET) oraz rozbudowanego interfejsu API po stronie klienta przeznaczonego do dublowania wielu schematy formantów widoczne w zestawie kontroli po stronie serwera ASP.NET.

Ten oficjalny dokument sprawdza funkcje lokalizacji obecne w bibliotece Microsoft AJAX Framework i Microsoft AJAX Script Library, w kontekście biznesowym potrzebnym do obsługi lokalizacji i przeglądania już zintegrowanej obsługi lokalizacji w sieci Web aplikacje udostępniane przez .NET Framework. Biblioteka skryptów AJAX firmy Microsoft korzysta z formatu pliku resx, który jest już używany przez aplikacje .NET, które zapewnia zintegrowaną obsługę IDE i typ zasobu możliwego do udostępnienia.

Niniejszy dokument jest oparty na wersji beta 2 Microsoft Visual Studio 2008. W tym dokumencie przyjęto również założenie, że użytkownik pracuje z programem Visual Studio 2008, a nie z Visual Web Developer Express, i zapewni wskazówki zgodne z interfejsem użytkownika programu Visual Studio. Niektóre przykłady kodu będą korzystać z szablonów projektów, które mogą być niedostępne w programie Visual Web Developer Express.

## <a name="the-need-for-localization"></a>*Potrzeba lokalizacji*

Szczególnie w przypadku deweloperów aplikacji dla przedsiębiorstw i deweloperów składników możliwość tworzenia narzędzi, które mogą być świadome różnic między kulturami i językami, staje się coraz bardziej potrzebna. Projektowanie składników z możliwością dostosowania do ustawień regionalnych klienta zwiększa produktywność deweloperów i zmniejsza ilość pracy wymaganej do adaptacji składnika do globalnie.

Lokalizacja to proces projektowania i integrowania pomocy technicznej dla określonego języka i kultury w aplikacji lub składniku aplikacji. Platforma Microsoft ASP.NET zapewnia szeroką obsługę lokalizacji dla standardowych aplikacji ASP.NET przez integrację standardowego modelu lokalizacji .NET; Microsoft AJAX Framework korzysta ze zintegrowanego modelu do obsługi różnorodnych scenariuszy, w których można przeprowadzić lokalizację. W przypadku programu Microsoft AJAX Framework skrypty mogą być lokalizowane przez wdrożenie ich w zestawach satelickich lub przy użyciu struktury statycznego systemu plików.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Osadzanie skryptów z zestawami satelitarnymi*

Zgodnie ze standardową strategią lokalizacji .NET Framework zasoby mogą być dołączone do zestawów satelickich. Zestawy satelickie zapewniają kilka korzyści w porównaniu z tradycyjnym uwzględnieniem zasobów w plikach binarnych — wszystkie te lokalizacje można aktualizować bez konieczności aktualizowania większego obrazu. dodatkowe lokalizacje można wdrożyć po prostu przez zainstalowanie zestawów satelickich w folder projektu i zestawy satelickie można wdrożyć bez powodowania ponownego ładowania głównego zestawu projektu. Szczególnie w projektach ASP.NET jest to korzystne, ponieważ może znacznie zmniejszyć ilość zasobów systemowych używanych przez aktualizacje przyrostowe, a co najmniej zakłóca użycie produkcyjnej witryny sieci Web.

Skrypty są osadzane w zestawach przez uwzględnienie ich w plikach Managed. resx (lub skompilowanych Resources), które są zawarte w zestawie w czasie kompilacji. Ich zasoby są następnie udostępniane aplikacji skryptu za pośrednictwem kodu generowanego przez środowisko uruchomieniowe AJAX, za pomocą atrybutów na poziomie zestawu

*Konwencje nazewnictwa dla osadzonych plików skryptów*

Zarządzanie skryptami w programie Microsoft AJAX Framework obsługuje wiele opcji związanych z wdrażaniem i testowaniem skryptów, a wytyczne są udostępniane, aby ułatwić korzystanie z tych opcji.

*Aby ułatwić debugowanie:*

Skrypty wydania (produkcyjne) nie powinny zawierać kwalifikatora `.debug` w nazwie pliku. Skrypty przeznaczone do debugowania powinny zawierać `.debug` w nazwie pliku.

*Aby ułatwić lokalizację:*

Skrypty neutralnych kultur nie powinny zawierać żadnych identyfikatorów kultur w nazwie pliku. W przypadku skryptów zawierających zlokalizowane zasoby kod języka ISO powinien być określony w nazwie pliku. Na przykład `es-CO` oznacza hiszpański, Kolumbia.

Poniższa tabela zawiera podsumowanie konwencji nazewnictwa plików z przykładami:

| Nazwa pliku | Znaczenie |
| --- | --- |
| Skrypt. js | Skrypt niezależny od wersji. |
| Skrypt. Debug. js | Skrypt niezależny od wersji Debug. |
| Skrypt. en-US. js | Wersja angielskojęzyczna, Stany Zjednoczone skrypt. |
| Script.debug.es-CO. js | Debugowanie w wersji hiszpańskiej, skrypt Kolumbia. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Przewodnik: Tworzenie zlokalizowanego, osadzonego skryptu

*Uwaga: ten Instruktaż wymaga użycia programu Visual Studio 2008 jako Visual Web Developer Express nie obejmuje szablonu projektu dla projektów biblioteki klas.*

1. Utwórz nowy projekt witryny sieci Web z zintegrowanymi rozszerzeniami AJAX ASP.NET. Utwórz inny projekt, projekt biblioteki klas w ramach rozwiązania o nazwie LocalizingResources.
2. Dodaj plik JScript o nazwie VerifyDeletion. js do projektu LocalizingResources, a także pliki zasobów resx o nazwie DeletionResources. resx i DeletionResources. es. resx. Pierwsza z nich będzie zawierać zasoby neutralne dla kultury; Ta ostatnia będzie zawierać zasoby języka hiszpańskiego.
3. Dodaj następujący kod do VerifyDeletion. js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Dla osób nieznających składni wyrażeń regularnych języka JavaScript tekst w pojedynczym ukośniku do przodu (w poprzednim przykładzie/FILENAME/jest przykładem) oznacza obiekt RegExp. Biblioteka MSDN zawiera obszerne odwołanie do języka JavaScript, a zasoby dotyczące obiektów natywnych JavaScript można znaleźć w trybie online.

1. Dodaj następujące ciągi zasobów do DeletionResources. resx: 

    **VerifyDelete**.: czy na pewno chcesz usunąć nazwę pliku?

    **Usunięte**: plik został usunięty.

1. Dodaj następujące ciągi zasobów do DeletionResources. es. resx: 

    **VerifyDelete**: Est Seguro DESEE quitar filename?

    **Usunięto**: NazwaPliku ha quitado.
2. Dodaj następujące wiersze kodu do pliku AssemblyInfo:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Dodaj odwołania do programu System. Web i system. Web. Extensions do projektu LocalizingResources.
2. Dodaj odwołanie do projektu LocalizingResources z projektu witryny sieci Web.
3. W polu Default. aspx w obszarze projekt witryny sieci Web zaktualizuj kontrolkę ScriptManager przy użyciu następującej dodatkowej adjustacji:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. W polu Default. aspx w dowolnym miejscu na stronie uwzględnij następujące znaczniki:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Naciśnij F5. Jeśli zostanie wyświetlony monit, Włącz debugowanie. Po załadowaniu strony naciśnij przycisk Usuń. Należy pamiętać, że zostanie wyświetlony monit w języku angielskim (chyba że komputer jest domyślnie ustawiony na preferowane zasoby języka hiszpańskiego) w celu potwierdzenia.
2. Zamknij okno przeglądarki i wróć do default. aspx. W dyrektywie nagłówka @Page Zastąp wartość autOfOr Culture i UICulture z es-ES. Ponownie naciśnij klawisz F5, aby ponownie uruchomić aplikację sieci Web w przeglądarce. Tym razem zostanie wyświetlony monit o usunięcie pliku w języku hiszpańskim:

[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-localization/_static/image3.png))

[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Kliknij, aby wyświetlić obraz o pełnym rozmiarze](understanding-asp-net-ajax-localization/_static/image6.png))

Należy zauważyć, że w tym przewodniku istnieje kilka wariantów. Na przykład skrypty mogą być rejestrowane przy użyciu kontrolki ScriptManager programistycznie podczas ładowania strony.

## <a name="including-a-static-script-file-structure"></a>*Dołączanie statycznej struktury pliku skryptu*

W przypadku używania statycznych plików skryptu do wdrożenia utracisz niektóre korzyści wynikające z korzystania z niewłaściwego schematu lokalizacji .NET. Przede wszystkim widoczne jest, że utracisz typ automatyczny wygenerowany z uwzględnieniem plików zasobów skryptu; na przykład w powyższym instruktażu zasoby zostały uwidocznione przy użyciu automatycznie generowanego typu zwanego komunikatem z formantu ScriptManager.

Istnieją jednak pewne korzyści wynikające z używania statycznej struktury plików skryptu. Aktualizacje mogą być wykonywane bez ponownej kompilacji i ponownego wdrożenia zestawów satelickich. można również użyć statycznej struktury plików, aby przesłonić osadzony skrypt, aby zintegrować niewielką część funkcji, która mogła nie zostać dostarczona ze składnikiem.

Firma Microsoft zaleca uniknięcie problemu kontroli wersji przez automatyczne generowanie zasobów skryptu podczas kompilacji projektu. W przypadku obsługi rozbudowanego kodu skryptu baza może stać się coraz trudniejsza, aby zapewnić, że zmiany kodu zostaną odzwierciedlone w każdym zlokalizowanym skrypcie. Alternatywnie można po prostu zachować jeden skrypt logiki i wiele skryptów lokalizacji, scalając pliki podczas kompilowania projektu.

Ze względu na to, że nie ma zasobów do deklaratywnego dołączenia, do statycznych plików skryptów należy się odwoływać przez dodanie `<asp:ScriptElement>` elementów jako elementu podrzędnego tagu `<Scripts>` formantu ScriptManager lub przez programowe dodanie obiektów `ScriptReference` do właściwości `Scripts` kontrolki `ScriptManager` na stronie w czasie wykonywania.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*Element ScriptManager i jego rola w lokalizacji*

Element ScriptManager udostępnia kilka automatycznych zachowań dla zlokalizowanych aplikacji:

- Automatycznie lokalizuje pliki skryptów na podstawie ustawień i konwencji nazewnictwa; na przykład ładuje skrypty obsługujące debugowanie w trybie debugowania i ładuje zlokalizowane skrypty na podstawie wyboru interfejsu użytkownika w przeglądarce.
- Umożliwia ona Definiowanie kultur, w tym kultur niestandardowych.
- Umożliwia kompresję plików skryptów za pośrednictwem protokołu HTTP.
- Umożliwia ona buforowanie skryptów w celu wydajnego zarządzania wieloma żądaniami.
- Dodaje warstwę operatora pośredniego do skryptów przez przekazanie ich przez zaszyfrowany adres URL.

Odwołania do skryptu można dodać do kontrolki ScriptManager programowo lub za pomocą znacznika deklaratywnego. Znaczniki deklaratywne są szczególnie przydatne podczas pracy z skryptami osadzonymi w zestawach innych niż sam projekt witryny sieci Web, ponieważ nazwa skryptu prawdopodobnie nie ulegnie zmianie, gdy poprawki są przekazywane w trybie push.

## <a name="summary"></a>Podsumowanie

Gdy aplikacje sieci Web rosną do większej liczby odbiorców, musi mieć możliwość osiągnięcia szerszej kultury, a społeczności staną się głównym modelem biznesowym. aplikacje sieci Web handlu elektronicznego muszą być w stanie korzystać z walut obcych, systemy zarządzania zawartością muszą być w stanie nie tylko przedstawić swoją zawartość, ale również wskazówki nawigacyjne i pola formularzy w innych językach, a firmy muszą wiedzieć, że to wymaganie pul.

.NET Framework wewnętrznie obsługuje zaawansowaną platformę lokalizacyjną, wykorzystując zestawy satelickie i pliki zasobów XML (resx), aby przedstawić jednolity sposób wyszukiwania ciągów zasobów i obrazów. Rozszerzenia AJAX ASP.NET, w tym Microsoft AJAX Framework i Microsoft AJAX Script Library, zapewniają obsługę tego modelu programowania w kodzie po stronie klienta, co pozwala na łatwe wyszukiwanie ciągów zasobów. Zestawy satelickie obsługują automatyczne dołączanie zasobów skryptów (rzeczywiste pliki. js) za pomocą ScriptResource. axd, o ile nazwy plików są zgodne z danym schematem nazewnictwa. Dzięki tej obsłudze rozszerzenia AJAX ASP.NET upraszczają lokalizację skryptów i globalizację aplikacji.

## <a name="bio"></a>*Materiał*

Scott Cate pracował z technologiami sieci Web firmy Microsoft od 1997 i jest prezydentem myKB.com ([www.myKB.com](http://www.myKB.com)), w którym wyspecjalizowany jest pisanie aplikacji opartych na ASP.NET, które są zgodne z podstawowymi rozwiązaniami oprogramowania. W witrynie Scotta można skontaktować się z pocztą e-mail na [scott.cate@myKB.com](mailto:scott.cate@myKB.com) lub w blogu w witrynie [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Poprzednie](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [dalej](understanding-asp-net-ajax-web-services.md)

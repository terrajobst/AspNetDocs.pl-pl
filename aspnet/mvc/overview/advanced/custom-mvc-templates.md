---
uid: mvc/overview/advanced/custom-mvc-templates
title: Niestandardowy szablon MVC | Microsoft Docs
author: joeloff
description: Utwórz szablon jako rozszerzenie VSIX.
ms.author: riande
ms.date: 12/10/2012
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 0603bc24e070e223551813f66a75889a2e46fd35
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616355"
---
# <a name="custom-mvc-template"></a>Niestandardowy szablon MVC

Autor [Jacques Eloff](https://github.com/joeloff)

W wersji programu MVC 3 Tools Update for Visual Studio 2010 wprowadzono osobny Kreator projektu dla projektów MVC. Zmiana była oparta o dwa czynniki. Najpierw wprowadzenie nowych szablonów w MVC 3 i obsługa dodatkowych aparatów wyświetlania, takich jak Razor, prowadzi do nadmiarowego okna dialogowego nowego projektu w programie Visual Studio. Drugi klient otrzymał pytanie o punkty rozszerzalności, a Kreator nowego projektu MVC zapewnia nam możliwość odpowiedzi na te żądania.

Dodawanie szablonów niestandardowych było procesem uciążliwy, który opierał się na korzystaniu z rejestru w celu wglądu nowych szablonów do Kreatora projektu MVC. Autor nowego szablonu musiał go otoczyć w pliku MSI, aby upewnić się, że w czasie instalacji zostaną utworzone niezbędne wpisy rejestru. Alternatywą jest utworzenie pliku ZIP zawierającego szablon i utworzenie przez użytkownika końcowego wymaganych wpisów rejestru.

Żadne z powyższych metod nie jest idealnym rozwiązaniem, dlatego zdecydowano się wykorzystać część istniejącej infrastruktury zapewnianej przez rozszerzenia [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) , aby ułatwić tworzenie, dystrybuowanie i instalowanie niestandardowych szablonów MVC, począwszy od MVC 4 dla programu Visual Studio 2012. Niektóre korzyści wynikające z tego podejścia to:

- Rozszerzenie VSIX może zawierać wiele szablonów, które obsługują różne języki (C# i Visual Basic) oraz wiele aparatów widoku (aspx i Razor).
- Rozszerzenie VSIX może kierować wiele jednostek SKU programu Visual Studio, w tym ekspresowych jednostek SKU.
- [Galeria Visual Studio](https://visualstudiogallery.msdn.microsoft.com/) ułatwia dystrybuowanie rozszerzenia do szerokiej grupy odbiorców.
- Rozszerzenia VSIX można uaktualnić, ułatwiając tworzenie poprawek i aktualizacji szablonów niestandardowych.

## <a name="prerequisites"></a>Wymagania wstępne

- Użytkownicy muszą znać Tworzenie szablonów projektu, w tym wymagane znaczniki dla plików vstemplate itp.
- Użytkownicy będą musieli mieć zainstalowaną Visual Studio Professional i nowszą wersję. Jednostki SKU Express nie obsługują tworzenia projektów VSIX.
- Zainstalowano [zestaw SDK programu Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30668) .

## <a name="example"></a>Przykład

Pierwszym krokiem jest utworzenie nowego projektu VSIX przy użyciu C# albo Visual Basic. Wybierz pozycję **plik > nowy projekt**, a następnie kliknij pozycję **rozszerzalność** w lewym okienku, a następnie wybierz **Projekt VSIX**.

![Nowy projekt](custom-mvc-templates/_static/image1.jpg)

Po utworzeniu projektu zostanie otwarty projektant VSIX.

![Metadane projektanta projektu](custom-mvc-templates/_static/image2.jpg)

Projektant może służyć do edytowania niektórych ogólnych właściwości rozszerzenia, które będą widoczne dla użytkowników podczas instalowania rozszerzenia lub przeglądania zainstalowanych rozszerzeń w programie Visual Studio (**narzędzia > rozszerzenia i aktualizacje**). Po zakończeniu ogólnych informacji kliknij **kartę Zainstaluj obiekty docelowe**.

![Cele instalacji projektanta projektu](custom-mvc-templates/_static/image3.jpg)

Ta karta służy do określania jednostek SKU i wersji programu Visual Studio, które są obsługiwane przez Twoje rozszerzenie. Zaznacz pole wyboru dla **tego VSIX jest zainstalowane dla wszystkich użytkowników** , aby włączyć instalacje dla komputera VSIX. Kliknij przycisk **New (nowy** ) po prawej stronie, aby dodać kolejne jednostki SKU, takie jak Web Developer Express (pliku VWD).

![Dodaj nowe miejsce docelowe instalacji](custom-mvc-templates/_static/image4.jpg)

Jeśli zamierzasz obsługiwać wszystkie jednostki SKU i nowsze (Professional, Premium i Ultimate), wystarczy wybrać minimalną jednostkę SKU w rodzinie, **Microsoft.VisualStudio.Pro**. Pamiętaj, aby zapisać wszystkie zmiany po ukończeniu instalacji.

![Cele instalacji projektanta projektu](custom-mvc-templates/_static/image5.jpg)

Karta **zasoby** służy do dodawania wszystkich plików zawartości do VSIX. Ze względu na to, że MVC wymaga niestandardowych metadanych, edytowanie nieprzetworzonego kodu XML pliku manifestu VSIX zamiast używania karty **składniki** do dodawania zawartości. Zacznij od dodania zawartości szablonu do projektu VSIX. Ważne jest, aby struktura folderu i zawartości była duplikatem układu projektu. Poniższy przykład zawiera cztery szablony projektów, które pochodzą z podstawowego szablonu projektu MVC. Upewnij się, że wszystkie pliki wchodzące w skład szablonu projektu (wszystko poniżej folderu ProjectTemplates) są dodawane do grupy elementów **zawartości** w pliku projektu VSIX i że każdy element zawiera zestaw metadanych **CopyToOutputDirectory** i **IncludeInVsix** , jak pokazano w poniższym przykładzie.

&lt;Content include =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;zawsze&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/Content&gt;

W przeciwnym razie środowisko IDE podejmie próbę skompilowania zawartości szablonu podczas kompilowania VSIX i prawdopodobnie pojawi się błąd. Pliki kodu w szablonach często zawierają specjalne [Parametry szablonu](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) używane przez program Visual Studio podczas tworzenia wystąpienia szablonu projektu i w związku z tym nie można go skompilować w IDE.

![Eksplorator rozwiązań](custom-mvc-templates/_static/image6.jpg)

Zamknij projektanta VSIX, a następnie kliknij prawym przyciskiem myszy plik **source. Extension. manifest** w **Eksplorator rozwiązań** a następnie wybierz pozycję **Otwórz za pomocą** i wybierz opcję **Edytor XML (tekst)** .

![Otwórz za pomocą okna dialogowego](custom-mvc-templates/_static/image7.jpg)

Utwórz element **&lt;elementy zawartości&gt;** i Dodaj **&lt;element&gt;elementów zawartości** dla każdego pliku, który musi być uwzględniony w VSIX. Atrybut **Type** każdego elementu **&lt;elementów zawartości&gt;** musi być ustawiony na **Microsoft. VisualStudio. MVC. Template**. Jest to niestandardowa przestrzeń nazw, która jest rozpoznawana tylko przez Kreatora projektu MVC. Dodatkowe informacje na temat struktury i układu pliku manifestu można znaleźć w dokumentacji schematu VSIX 2,0.

Po prostu dodanie plików do VSIX nie jest wystarczające do zarejestrowania szablonów za pomocą Kreatora MVC. Należy podać informacje takie jak nazwa szablonu, opis, Obsługiwane aparaty widoku i język programowania do kreatora MVC. Te informacje są przewożone w atrybutach niestandardowych skojarzonych z **&lt;&gt;elementu zawartości** dla każdego pliku **vstemplate** .

&lt;zasobów d:VsixSubPath =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Typ =&quot;Microsoft. VisualStudio. MVC. Template&quot;

d:Source = plik&quot;&quot;

Ścieżka =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType =&quot;MVC&quot;

Język =&quot;C#&quot;

ViewEngine =&quot;aspx&quot;

TemplateId =&quot;MyMvcApplication&quot;

Title =&quot;niestandardowe podstawowe aplikacje sieci Web&quot;

Opis =&quot;szablonu niestandardowego pochodzącego z podstawowej aplikacji sieci Web MVC (Razor)&quot;

Wersja =&quot;4,0&quot;/&gt;

Poniżej znajduje się objaśnienie atrybutów niestandardowych, które muszą być obecne:

- Wartość **ProjectType** musi być ustawiona na MVC.
- **Język** oznacza język programowania obsługiwany przez szablon. Prawidłowe wartości to C# lub VB.
- **ViewEngine** określa aparat widoku obsługiwany przez szablon, taki jak aspx lub Razor. Możesz określić wartość niestandardową dla tego pola.
- **TemplateID** jest używany do grupowania szablonów. Jeśli wartość jest zgodna z IDENTYFIKATORem istniejącego szablonu, zostaną zasłonięte szablony, które zostały wcześniej zarejestrowane w Kreatorze MVC.
- **Tytuł** określa Krótki opis wyświetlany w Kreatorze MVC poniżej każdego szablonu projektu.
- **Opis** określa bardziej pełny opis szablonu.

Po dodaniu wszystkich plików do manifestu i zapisaniu go można zauważyć, że na karcie **zasoby** w projektancie zostaną wyświetlone wszystkie pliki, ale nie atrybuty niestandardowe dodane do **&lt;elementu zawartości&gt;** elementów dla plików **vstemplate** .

![Zasoby projektanta projektu](custom-mvc-templates/_static/image8.jpg)

Wszystko, co już teraz, ma na celu skompilowanie projektu VSIX i zainstalowanie go.

Upewnij się, że wszystkie wystąpienia programu Visual Studio są zamknięte na komputerze, na którym zamierzasz przetestować rozszerzenie VSIX. Program Visual Studio skanuje nowe rozszerzenia podczas uruchamiania, więc jeśli IDE jest otwarty podczas instalowania VSIX, należy ponownie uruchomić program Visual Studio. W Eksploratorze kliknij dwukrotnie plik VSIX, aby uruchomić **Instalatora VSIX**, kliknij przycisk **Zainstaluj** , a następnie uruchom program Visual Studio.

![Instalator VSIX](custom-mvc-templates/_static/image9.jpg)

Z menu wybierz pozycję **narzędzia > rozszerzenia i aktualizacje** , aby upewnić się, że rozszerzenie zostało zainstalowane. Jeśli Instalator VSIX zgłosił błędy podczas instalacji rozszerzenia, można wyświetlić dziennik Instalatora VSIX, aby uzyskać więcej informacji. Dziennik jest zwykle tworzony w folderze **% temp%** użytkownika, który zainstalował rozszerzenie, na przykład **C:\Users\Bob\AppData\Local\Temp**.

![Rozszerzenia i aktualizacje](custom-mvc-templates/_static/image10.jpg)

Po zamknięciu okna można utworzyć projekt MVC 4, aby sprawdzić, czy nowe szablony są wyświetlane w Kreatorze MVC.

![Nowy projekt ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Ograniczenia

1. Kreator MVC nie obsługuje zlokalizowanych szablonów niestandardowych.
2. Kreator nie będzie zgłaszał błędów, jeśli nie można zlokalizować szablonów niestandardowych. Jeśli którykolwiek z wymaganych atrybutów niestandardowych nie jest obecny, szablon zostałby po prostu wykluczony z kreatora.

---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: ASP.NET MVC — Omówienie | Microsoft Docs
author: microsoft
description: Dowiedz się więcej o różnicach między aplikacjami aplikacji ASP.NET MVC i ASP.NET Web Forms. Dowiedz się, jak określić, kiedy należy utworzyć aplikację ASP.NET MVC.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: 73965c71f37de13e3813df089a253fde528ea7ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541560"
---
# <a name="aspnet-mvc-overview"></a>ASP.NET MVC — omówienie

przez [firmę Microsoft](https://github.com/microsoft)

> Dowiedz się więcej o różnicach między aplikacjami aplikacji ASP.NET MVC i ASP.NET Web Forms. Dowiedz się, jak określić, kiedy należy utworzyć aplikację ASP.NET MVC.

Wzorzec architektoniczny Model-View-Controller (MVC) oddziela aplikację do trzech głównych składników: modelu, widoku i kontrolera. Struktura ASP.NET MVC stanowi alternatywę dla wzorca formularzy sieci Web ASP.NET do tworzenia aplikacji sieci Web opartych na MVC. ASP.NET MVC to niewielka, wysoce testowalna struktura prezentacji, która (tak jak aplikacje oparte na formularzach sieci Web) jest zintegrowana z istniejącymi funkcjami platformy ASP.NET, takimi jak strony wzorcowe i uwierzytelnianie oparte na członkostwie. Struktura MVC jest zdefiniowana w przestrzeni nazw **System. Web. MVC** i jest podstawową, obsługiwaną częścią przestrzeni nazw **System. Web** .   
  
MVC to standardowy wzorzec projektowania, który zna już wielu deweloperów. Niektóre rodzaje aplikacji internetowych zyskają w przypadku użycia struktury MVC. Inne będą nadal korzystać z tradycyjnego wzorca aplikacji ASP.NET, który bazuje na formularzach sieci Web i odświeżaniu strony. W jeszcze innych rodzajach aplikacji internetowych będą łączone oba podejścia, bo jedno nie wyklucza drugiego.   
  
Struktura MVC obejmuje następujące składniki:

[![wywoływania akcji kontrolera, która oczekuje wartości parametru](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Ilustracja 01**: Wywoływanie akcji kontrolera, która oczekuje wartości parametru ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](asp-net-mvc-overview/_static/image2.png))

- **Modele**. Obiekty modelu są częściami aplikacji, które implementują logikę dla domeny danych aplikacji. Często modele obiektu pobierają stan modelu z bazy danych i zapisują go w niej. Na przykład obiekt produktu może pobrać informacje z bazy danych, na nim pracować, a następnie zapisać zaktualizowane informacje z powrotem w tabeli Products w SQL Server.

W małych aplikacjach model często stanowi separację koncepcyjną zamiast fizycznej. Na przykład, jeśli aplikacja odczytuje zestaw danych i wysyła je do widoku, aplikacja nie ma warstwy modelu fizycznego i skojarzonych klas. W takim przypadku zestaw danych przyjmuje rolę obiektu modelu.

- **Widoki**. Widoki to składniki, które wyświetlają interfejs użytkownika aplikacji. Zazwyczaj interfejs ten jest tworzony na podstawie danych modelu. Przykładem może być widok z tabeli Produkty, który wyświetla pola tekstowe, listy rozwijane i pola wyboru na podstawie bieżącego stanu obiektu produkty.

- **Kontrolery**. Kontrolery są składnikami, które obsługują interakcję z użytkownikiem, współpracują z modelem i ostatecznie wybierają widok do wyrenderowania, w którym ma zostać wyświetlony interfejs użytkownika. W aplikacji MVC widok służy wyłącznie do wyświetlania informacji. Za obsługę danych wprowadzanych przez użytkownika i interakcję z użytkownikiem odpowiada kontroler. Na przykład kontroler obsługuje wartości ciągu zapytania i przekazuje te wartości do modelu, który z kolei wysyła zapytania do bazy danych przy użyciu wartości.

Wzorzec MVC pomaga w tworzeniu aplikacji, w których różne aspekty (logika danych wejściowych, logika biznesowa i logika interfejsu użytkownika) są rozdzielone, gwarantując przy tym luźne powiązanie tych elementów. Wzorzec określa, w którym miejscu aplikacji ma się znajdować każdy rodzaj logiki. Logika interfejsu użytkownika jest powiązana z widokiem. Logika danych wejściowych jest powiązana z kontrolerem. Logika biznesowa jest powiązana z modelem. Taki rozdział pomaga w zarządzaniu złożonością podczas tworzenia aplikacji, ponieważ pozwala skupić się w danym momencie na jednym aspekcie implementacji. Na przykład można skoncentrować się na widoku bez polegania na logice biznesowej.   
  
Oprócz zarządzania złożonością wzorzec MVC pozwala także na łatwiejsze testowanie aplikacji niż ma to miejsce w przypadku aplikacji internetowych platformy ASP.NET opartych na formularzach internetowych. Przykładowo w aplikacji internetowej platformy ASP.NET opartej na formularzach internetowych pojedyncza klasa jest używana zarówno do wyświetlania danych wyjściowych, jak i odpowiadania na dane wprowadzane przez użytkownika. Pisanie zautomatyzowanych testów dla aplikacji platformy ASP.NET opartych na formularzach sieci Web może być skomplikowane, ponieważ przetestowanie jednej strony wymaga utworzenia wystąpień klasy strony, wszystkich jej kontrolek podrzędnych i dodatkowych klas zależnych w aplikacji. Ponieważ do uruchomienia strony trzeba utworzyć wystąpienia aż tylu klas, ciężko może być napisać testy skoncentrowane wyłącznie na pojedynczych częściach aplikacji. Testy dla aplikacji platformy ASP.NET opartych na formularzach sieci Web mogą więc być trudniejsze do zaimplementowania niż testy aplikacji MVC. Ponadto testy w aplikacji platformy ASP.NET opartej na formularzach sieci Web wymagają serwera sieci Web. Struktura MVC rozłącza składniki i intensywnie korzysta z interfejsów, co umożliwia testowanie pojedynczych składników w izolacji od reszty struktury.   
  
Luźne powiązanie między trzema głównymi składnikami aplikacji MVC wspiera także programowanie równoległe. Na przykład jeden deweloper może pracować w widoku, drugi deweloper może pracować na logice kontrolera, a trzeci deweloper może skupić się na logice biznesowej w modelu.

## <a name="deciding-when-to-create-an-mvc-application"></a>Podejmowanie decyzji o tym, kiedy należy utworzyć aplikację MVC

To, czy aplikacja internetowa ma zostać zaimplementowana za pomocą struktury ASP.NET MVC, czy modelu formularzy internetowych platformy ASP.NET, należy starannie rozważyć. Struktura MVC nie zastępuje modelu formularzy internetowych; w aplikacjach internetowych można korzystać z obu struktur. (Już istniejące aplikacje oparte na formularzach sieci Web będą działać dokładnie tak samo jak do tej pory).   
  
Przed podjęciem decyzji o tym, czy dla konkretnej witryny sieci Web ma być używana struktura MVC, czy model formularzy sieci Web, należy rozważyć zalety każdego z podejść.

### <a name="advantages-of-an-mvc-based-web-application"></a>Zalety aplikacji internetowych opartych na strukturze MVC

Struktura ASP.NET MVC ma następujące zalety:

- Ułatwia zarządzanie złożonością, dzieląc aplikację na model, widok i kontroler.
- Nie używa stanu widoku ani formularzy opartych na serwerach. To sprawia, że struktura MVC jest idealna dla deweloperów, którzy chcą w pełni kontrolować działanie aplikacji.
- Używa wzorca kontrolera frontowego, który przetwarza żądania aplikacji internetowej za pośrednictwem jednego kontrolera. To pozwala projektować aplikacje obsługujące złożoną infrastrukturę routingu. Aby uzyskać więcej informacji, zobacz [Front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Kontroler frontonu") w witrynie MSDN w sieci Web.
- Oferuje lepszą obsługę projektowania opartego na testach (test-driven development, TDD).
- Dobrze sprawdza się w przypadku aplikacji sieci Web, które są obsługiwane przez duże zespoły deweloperów i projektantów sieci Web, którzy potrzebują wysokiego poziomu kontroli nad zachowaniem aplikacji.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Zalety aplikacji internetowych opartych na formularzach internetowych

Struktura oparta na formularzach sieci Web ma następujące zalety:

- Obsługuje model zdarzeń zachowujący stan przez HTTP, co przydaje się przy projektowaniu branżowych aplikacji internetowych. Aplikacja oparta na formularzach sieci Web udostępnia dziesiątki zdarzeń, które są obsługiwane przez setki kontrolek serwerowych.
- Korzysta z wzorca kontrolera strony, który dodaje funkcje do pojedynczych stron. Aby uzyskać więcej informacji, zobacz [kontroler strony](https://go.microsoft.com/fwlink/?LinkId=106359 "Kontroler strony") w witrynie MSDN w sieci Web.
- Korzysta ona z widoku stanu lub formularzy opartych na serwerze, które ułatwiają zarządzanie informacjami o stanie.
- Sprawdza się w przypadku niewielkich zespołów projektantów witryn sieci Web oraz w przypadku projektantów, którzy chcą czerpać korzyści z dużej liczby składników dostępnych do szybkiego opracowywania aplikacji.
- Ogólnie rzecz biorąc, jest mniej skomplikowane do tworzenia aplikacji, ponieważ składniki (Klasa **strony** , kontrolki itd.) są ściśle zintegrowane i zwykle wymagają mniej kodu niż model MVC.

## <a name="features-of-the-aspnet-mvc-framework"></a>Funkcje struktury ASP.NET MVC

Struktura ASP.NET MVC ma następujące funkcje:

- Domyślnie separacja zadań aplikacji (logiki wejściowej, logiki biznesowej i logiki interfejsu użytkownika), testowania i programowania opartego na testach (TDD). Wszystkie główne kontrakty w strukturze MVC bazują na interfejsach i mogą być testowane za pomocą obiektów-makiet, które są symulowanymi obiektami imitującymi zachowanie rzeczywistych obiektów w aplikacji. Test jednostkowy aplikacji można przeprowadzić bez konieczności uruchamiania kontrolerów w procesie ASP .NET, co sprawia, że testy jednostkowe są szybkie i elastyczne. Wykorzystać można dowolną strukturę testowania jednostkowego zgodną z platformą .NET Framework.
- Rozszerzalna struktura obsługująca dodatki. Składniki struktury ASP.NET MVC zostały zaprojektowane w taki sposób, aby mogły być łatwo zastępowane lub dostosowywane. Można dodać własny aparat widoku, zasady routingu adresów URL, serializację parametrów metod akcji i inne składniki. Struktura ASP.NET MVC obsługuje też korzystanie z modeli kontenera Iniekcja zależności oraz Inwersja kontroli. Program DI pozwala na wstrzyknięcie obiektów do klasy, a nie poleganie na klasie w celu utworzenia samego obiektu. Inwersja kontroli określa, że jeśli obiekt wymaga innego obiektu, pierwszy obiekt powinien pobrać drugi ze źródła zewnętrznego, takiego jak plik konfiguracji. To ułatwia testowanie.
- Zaawansowany składnik mapowania adresów URL, który umożliwia tworzenie aplikacji, które mają zrozumiały i przeszukiwany adres URL. Adresy URL nie muszą zawierać rozszerzeń nazw plików i zostały zaprojektowane do obsługi wzorców nazewnictwa adresów URL pozwalających na przeprowadzanie optymalizacji dla aparatów wyszukiwania (SEO) i adresowania REST (Representational State Transfer).
- Obsługiwane jest używanie jako szablonów widoków kodu znaczników w istniejących plikach kodu znaczników stron platformy ASP.NET (plikach aspx), kontrolek użytkownika (plikach ascx) i stron wzorcowych (plikach master). Możesz użyć istniejących funkcji ASP.NET z platformą MVC ASP.NET, takich jak zagnieżdżone strony wzorcowe, wyrażenia w trybie online (&lt;% =%&gt;), deklaratywne kontrolki serwera, szablony, powiązania danych, lokalizacje i tak dalej.
- Obsługiwane są istniejące funkcje platformy ASP.NET. Struktura ASP.NET MVC pozwala korzystać z funkcji takich jak uwierzytelnianie oparte na formularzach i uwierzytelnianie systemu Windows, autoryzacja adresu URL, członkostwo i role, buforowanie wyjścia i danych, zarządzanie stanem sesji i profilu, monitorowanie kondycji, system konfiguracji i architektura dostawcy.

---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Instrukcje: dodawanie stron mobilnych do aplikacji ASP.NET Web Forms/MVC | Microsoft Docs'
author: rick-anderson
description: W tym artykule opisano różne sposoby obsługi stron zoptymalizowanych pod kątem urządzeń przenośnych z aplikacji ASP.NET Web Forms/MVC oraz zaproponowanie architektury i...
ms.author: riande
ms.date: 01/20/2011
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: 63c555358d06a9506bb5c8c993800c3307108192
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78572738"
---
# <a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Instrukcje: dodawanie stron dla urządzeń przenośnych do aplikacji ASP.NET Web Forms/MVC

> **Dotyczy**
> 
> - ASP.NET Web Forms w wersji 4,0
> - ASP.NET MVC w wersji 3,0
> 
> **Podsumowanie**
> 
> W tym artykule opisano różne sposoby obsługi stron zoptymalizowanych pod kątem urządzeń przenośnych z aplikacji ASP.NET Web Forms/MVC oraz zaproponowanie problemów dotyczących architektury i projektu, które należy wziąć pod uwagę w przypadku szerokiego zakresu urządzeń. W tym dokumencie wyjaśniono również, dlaczego kontrolki mobilne ASP.NET z ASP.NET 2,0 do 3,5 są już przestarzałe i omawiają niektóre nowoczesne alternatywy.

## <a name="contents"></a>Spis treści

- Omówienie
- Opcje architektury
- Wykrywanie przeglądarki i urządzeń
- Jak aplikacje ASP.NET Web Forms mogą prezentować strony specyficzne dla urządzeń przenośnych
- Jak aplikacje ASP.NET MVC mogą przedstawiać strony specyficzne dla urządzeń przenośnych
- Dodatkowe zasoby

Przykłady kodu do pobrania przedstawiające techniki tego oficjalnego dokumentu dla ASP.NET Web Forms i MVC można znaleźć w temacie [Mobile Apps & witryny z ASP.NET](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Omówienie

Urządzenia przenośne — telefony smartphone, telefony funkcji i tablety — Kontynuuj rozwój popularności jako środek do uzyskiwania dostępu do sieci Web. W przypadku wielu deweloperów sieci Web i firmowych sieci Web oznacza to, że coraz częściej jest zapewnienie doskonałego środowiska przeglądania dla Gości korzystających z tych urządzeń.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Jak wcześniejsze wersje programu ASP.NET obsługują przeglądarki mobilne

ASP.NET wersje 2,0 do 3,5 obejmowały *ASP.net Mobile Controls*: zestaw kontrolek serwera dla urządzeń przenośnych z przestrzeni nazw *System. Web. Mobile. dll* i *System. Web. UI. MobileControls* . Zestaw jest nadal uwzględniony w ASP.NET 4, ale jest przestarzały. Deweloperzy są doradzani do migrowania do bardziej nowoczesnych metod, takich jak opisane w tym dokumencie.

Powód, dla którego kontrolki mobilne ASP.NET zostały oznaczone jako przestarzałe, są zorientowane na telefony komórkowe, które były wspólne wokół 2005 i wcześniejszych. Kontrolki są przeznaczone głównie do renderowania WML lub cHTML znaczników (zamiast zwykłych HTML) dla przeglądarek WAP tej ery. Jednak WAP, WML i cHTML nie są już potrzebne w przypadku większości projektów, ponieważ język HTML stał się powszechnie wyznacznikiem dla przeglądarek mobilnych i klasycznych.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Wyzwania dotyczące obsługi urządzeń przenośnych dzisiaj

Mimo że przeglądarki mobilne są teraz niemal ogólnie obsługiwane w języku HTML, nadal będziesz mieć wiele wyzwań, gdy chcesz utworzyć wspaniałe środowiska do przeglądania mobilnego:

- ***Rozmiar ekranu*** — urządzenia przenośne różnią się znacznie w formie, a ich ekrany często są znacznie mniejsze niż monitory klasyczne. W związku z tym może być konieczne zaprojektowanie zupełnie różnych układów stron.
- ***Metody wejściowe*** — niektóre urządzenia mają konsole, niektóre mają pióro, inne używają dotyku. Konieczne może być uwzględnienie wielu mechanizmów nawigacji i metod wprowadzania danych.
- ***Zgodność ze standardami*** — wiele przeglądarek dla urządzeń przenośnych nie obsługuje najnowszych standardów HTML, CSS i JavaScript.
- ***Przepustowość*** — wydajność sieci komórkowej danych różni się w zależności od siebie, a niektórzy użytkownicy końcowi pobierają opłaty przez megabajty.

Nie istnieje żadne rozwiązanie o rozmiarze jedno-do-wszystkiego. aplikacja będzie musiała wyglądać i zachowywać się inaczej w zależności od urządzenia, do którego uzyskuje dostęp. W zależności od wybranego poziomu pomocy technicznej dla urządzeń przenośnych może to być większe wyzwanie dla deweloperów sieci Web niż w przypadku, gdy kiedykolwiek była "liczba" konfliktów przeglądarki pulpitu ".

Deweloperzy, którzy zbliżają się do obsługi przeglądarki mobilnej po raz pierwszy, często uważają, że są jedynie ważne, aby obsługiwać najnowsze i najbardziej zaawansowane telefony smartphone (np. Windows Phone 7, iPhone lub Android), prawdopodobnie ponieważ deweloperzy często są właścicielami urządzeniem. Jednak tańsze telefony są nadal niezwykle popularne, a ich właściciele wykorzystują je do przeglądania sieci Web — zwłaszcza w krajach, w których telefony komórkowe są łatwiejsze do pobrania niż połączenie szerokopasmowe. Firma będzie musiała zdecydować, jakiego zakresu urządzeń należy wspierać, biorąc pod uwagę jego prawdopodobnie klientów. Jeśli tworzysz broszurę online dla Spa możliwość zaprojektowania, możesz podjąć decyzję biznesową tylko dla zaawansowanych telefonów Smartphone, podczas gdy tworzysz system rezerwacji biletów dla kina, prawdopodobnie musisz mieć konto dla odwiedzających z mniejszą zaawansowaną funkcją telefonu.

## <a name="architectural-options"></a>Opcje architektury

Przed przekazaniem szczegółowych informacji technicznych ASP.NET Web Forms lub MVC należy zauważyć, że deweloperzy sieci Web ogólnie mają trzy główne opcje obsługi przeglądarek mobilnych:

1. ***Nic nie rób —*** Możesz po prostu utworzyć standardową, zorientowaną na komputerze aplikację sieci Web i polegać na przeglądarkach mobilnych do renderowania zadowalająco IT. 

    - **Zalety**: jest to najtańsza opcja implementowania i konserwowania — brak dodatkowej pracy
    - **Wadą**: zapewnia najgorszy interfejs użytkownika końcowego: 

        - Najnowsze smartfony mogą renderować kod HTML tak samo jak przeglądarka pulpitu, ale użytkownicy nadal będą zmuszeni do powiększania i przewijania w poziomie i w pionie, aby wykorzystać zawartość na małym ekranie. Jest to daleko od optymalnej.
        - Na starszych urządzeniach i telefonach funkcji może nie być możliwe renderowanie znaczników w zadowalający sposób.
        - Nawet na najnowszych urządzeniach tabletu (których ekrany mogą być tak duże jak ekrany laptopów), mają zastosowanie różne reguły interakcji. Dane wejściowe oparte na dotyku działają najlepiej z większymi przyciskami i łączami, a także nie ma możliwości przesuwania kursora myszy nad menu rozwijane.
2. ***Rozwiąż problem na kliencie* —** z starannym użyciem CSS i [ulepszaniem progresywnym](http://en.wikipedia.org/wiki/Progressive_enhancement) można utworzyć znaczniki, style i skrypty, które dostosowują się do dowolnej przeglądarki. Na przykład w przypadku [zapytań o multimedia w języku CSS 3](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/)można utworzyć układ wielokolumnowy, który przekształci się w jeden układ kolumn na urządzeniach, których ekrany są węższe niż wybrany próg. 

    - **Zalety**: Optymalizacja renderowania dla określonego urządzenia w użyciu, nawet w przypadku nieznanych przyszłych urządzeń w zależności od właściwości ekranu i danych wejściowych, które mają
    - **Zalety**: łatwe udostępnianie logiki po stronie serwera na wszystkich typach urządzeń — minimalne duplikowanie kodu lub nakładu pracy
    - **Niekorzyść**: urządzenia przenośne są inne niż urządzenia stacjonarne, które mogą naprawdę chcieć, aby strony mobilne były całkowicie różne od stron pulpitu, pokazując różne informacje. Takie wahania mogą być niewydajne lub niemożliwe do niezawodnego korzystania wyłącznie z CSS, szczególnie z uwzględnieniem, jak niespójnie starsze urządzenia interpretują reguły CSS. Jest to szczególnie prawdziwe w przypadku atrybutów CSS 3.
    - **Wadą**: nie obsługuje różnych logiki po stronie serwera i przepływów pracy dla różnych urządzeń. Nie można na przykład zaimplementować przepływu pracy wyewidencjonowywania koszyka dla użytkowników mobilnych za pomocą samego arkusza CSS.
    - **Wadą**: niewydajne użycie przepustowości. Może być konieczne przesłanie znaczników i stylów, które mają zastosowanie do wszystkich możliwych urządzeń, nawet jeśli urządzenie docelowe będzie używać tylko podzestawu tych informacji.
3. ***Rozwiąż problem na serwerze* —** Jeśli serwer wie, które urządzenie uzyskuje do niego dostęp — lub co najmniej cechuje to urządzenie, takie jak rozmiar ekranu i Metoda wejściowa, a także czy jest to urządzenie przenośne — może uruchamiać inną logikę i wyprowadzać różne znaczniki HTML. 

    - **Zalety:** Maksymalna elastyczność. Nie ma żadnego limitu, w jaki sposób można zmienić logikę po stronie serwera dla urządzeń przenośnych lub zoptymalizować adiustację pod kątem żądanego układu określonego dla urządzenia.
    - **Zalety:** Efektywne wykorzystanie przepustowości. Wystarczy przesłać informacje o znaczniku i stylu, które będą używane przez urządzenie docelowe.
    - **Wadą:** Czasami wymusza powtarzanie nakładu pracy lub kodu (np. Tworzenie podobnych, ale nieco różnych kopii stron formularzy sieci Web lub widoków MVC). Tam, gdzie to możliwe, zostanie wdrożona wspólna logika do warstwy lub usługi źródłowej, ale nadal niektóre części kodu interfejsu użytkownika lub znaczniki mogą być duplikowane, a następnie obsługiwane równolegle.
    - **Wadą:** Wykrywanie urządzeń nie jest proste. Wymaga listy lub bazy danych znanych typów urządzeń i ich właściwości (które mogą nie zawsze być idealnie aktualne) i nie gwarantuje dokładnego dopasowania każdego żądania przychodzącego. W tym dokumencie opisano niektóre opcje i ich pułapek w późniejszym czasie.

Aby uzyskać najlepsze wyniki, większość deweloperów może znaleźć opcje połączenia (2) i (3). Małe różnice stylistyczne najlepiej nadaje się na kliencie przy użyciu CSS lub nawet w języku JavaScript, a istotne różnice w zakresie danych, przepływu pracy lub znaczników są najbardziej efektywnie zaimplementowane w kodzie po stronie serwera.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Ten dokument koncentruje się na technikach po stronie serwera

Ponieważ ASP.NET Web Forms i MVC są głównie technologiami po stronie serwera, ten oficjalny dokument koncentruje się na technikach po stronie serwera, które umożliwiają tworzenie różnych znaczników i logiki dla przeglądarek mobilnych. Oczywiście można również połączyć ten element z dowolną techniką po stronie klienta (np. kwerendami z nośnikami CSS 3, udoskonaleniem kodu JavaScript), ale jest to bardziej kwestia projektowania sieci Web niż programowanie ASP.NET.

## <a name="browser-and-device-detection"></a>Wykrywanie przeglądarki i urządzeń

Kluczowym wymaganiem wstępnym dla wszystkich technik po stronie serwera w celu obsługi urządzeń przenośnych jest określenie, które urządzenie jest używane przez Gościa. W rzeczywistości jeszcze lepszym rozwiązaniem niż znajomość producenta i numer modelu tego urządzenia jest znajomość *cech charakterystycznych* urządzenia. Cechy mogą obejmować:

- Czy jest to urządzenie przenośne?
- Input — Metoda (mysz/klawiatura, dotyk, klawiatura, joystick,...)
- Rozmiar ekranu (fizycznie i w pikselach)
- Obsługiwane formaty multimediów i danych
- Etc.

Lepszym rozwiązaniem jest podejmowanie decyzji na podstawie cech charakterystycznych niż numer modelu, ponieważ następnie lepiej jest obsługiwać przyszłe urządzenia.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Za pomocą ASP. Wbudowana obsługa wykrywania przeglądarki sieci

ASP.NET Web Forms i deweloperzy MVC mogą natychmiast odnaleźć ważne cechy przeglądarki odwiedzającej, sprawdzając właściwości obiektu *Request. Browser* . Na przykład Zobacz

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ...a wielu innych

W tle platforma ASP.NET dopasowuje nagłówek HTTP przychodzącego *agenta użytkownika* (UA) do wyrażenia regularnego w zestawie plików XML definicji przeglądarki. Domyślnie platforma zawiera definicje dla wielu popularnych urządzeń przenośnych i można dodawać niestandardowe pliki definicji przeglądarki dla innych, które mają być rozpoznawane. Aby uzyskać więcej informacji, zobacz strony MSDN [ASP.NET Web Server Controls and Browser Capabilities](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Korzystanie z bazy danych urządzeń WURFL za pośrednictwem 51Degrees.mobi Foundation

Podczas gdy ASP. Wbudowana obsługa wykrywania przeglądarki sieci jest wystarczająca dla wielu aplikacji, jednak istnieją dwa główne przypadki, gdy może to być niewystarczające:

- ***Chcesz rozpoznać najnowsze urządzenia***(bez ręcznego tworzenia dla nich plików definicji przeglądarki). Należy pamiętać, że pliki definicji przeglądarki .NET 4 nie są wystarczająco aktualne, aby można było rozpoznać Windows Phone 7, telefony z systemem Android, przeglądarki programu Opera Mobile lub Apple iPad.
- ***Potrzebujesz bardziej szczegółowych informacji na temat możliwości urządzeń***. Może być konieczne poznanie metody wejściowej urządzenia (np. dotknięcie klawiatury) lub formatów dźwięku obsługiwanych przez przeglądarkę. Te informacje nie są dostępne w standardowych plikach definicji przeglądarki.

[Projekt WURFL ( *Wireless Universal Resource File* )](http://wurfl.sourceforge.net/) zawiera znacznie bardziej aktualne i szczegółowe informacje o urządzeniach przenośnych używanych obecnie.

Doskonałe wiadomości dla deweloperów platformy .NET to ASP. Funkcja wykrywania przeglądarki sieci jest rozszerzalna, dlatego można ją ulepszyć, aby wyeliminować te problemy. Można na przykład dodać do projektu bibliotekę Open Source [*51Degrees.mobi Foundation*](https://github.com/51Degrees/dotNET-Device-Detection) . Jest to ASP.NET IHttpModule lub dostawca możliwości przeglądarki (można go używać zarówno w przypadku formularzy sieci Web, jak i aplikacji MVC), które bezpośrednio odczytuje dane WURFL i przechwytuje je do ASP. Wbudowany mechanizm wykrywania przeglądarki sieci Web. Po zainstalowaniu modułu *żądanie. przeglądarka* będzie nagle zawierać bardziej dokładne i szczegółowe informacje: będzie poprawnie rozpoznawać wiele wcześniej wymienionych urządzeń i wyświetlać ich możliwości (w tym dodatkowe możliwości, takie jak Metoda wejściowa). Więcej informacji można znaleźć w dokumentacji projektu.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Jak aplikacje formularzy sieci Web mogą przedstawiać strony z konkretnymi urządzeniami przenośnymi

Domyślnie poniżej przedstawiono sposób wyświetlania nowej aplikacji formularzy sieci Web na typowych urządzeniach przenośnych:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Jasno, żaden układ nie wygląda dobrze przyjazny — ta strona została zaprojektowana dla dużego, zorientowanego na poziomie monitora, a nie dla małego ekranu zorientowanego na orientację pionową. Co możesz zrobić?

Zgodnie z opisem we wcześniejszej części tego dokumentu istnieje wiele sposobów dostosowywania stron do urządzeń przenośnych. Niektóre techniki są oparte na serwerze, inne działają na kliencie.

### <a name="creating-a-mobile-specific-master-page"></a>Tworzenie strony wzorcowej specyficznej dla urządzeń przenośnych

W zależności od wymagań można korzystać z tych samych formularzy sieci Web dla wszystkich odwiedzających, ale mają dwie osobne strony główne: jeden dla odwiedzających komputery, drugi dla odwiedzających dla urządzeń przenośnych. Dzięki temu można elastycznie zmieniać arkusz stylów CSS lub znaczniki HTML najwyższego poziomu, aby odpowiadały urządzeniom przenośnym, bez wymuszania duplikowania żadnej logiki strony.

Jest to bardzo proste. Na przykład można dodać program obsługi przed inicjalizacją, taki jak poniższy, do formularza sieci Web:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Teraz Utwórz stronę wzorcową o nazwie Mobile. Master w folderze najwyższego poziomu aplikacji, która będzie używana po wykryciu urządzenia przenośnego. W razie potrzeby Strona wzorcowa urządzenia przenośnego może odwoływać się do arkusza stylów CSS specyficznych dla urządzeń przenośnych. Osoby odwiedzające pulpit nadal będą widzieć domyślną stronę wzorcową, a nie na urządzeniu mobilnym.

### <a name="creating-independent-mobile-specific-web-forms"></a>Tworzenie niezależnych formularzy sieci Web dla urządzeń przenośnych

W celu zapewnienia maksymalnej elastyczności można znacznie nie tylko mieć osobne strony wzorcowe dla różnych typów urządzeń. Można zaimplementować dwa *całkowicie oddzielne zestawy stron formularzy sieci Web* — jeden zestaw dla przeglądarek klasycznych, inny zestaw dla urządzeń przenośnych. Jest to najlepsze rozwiązanie, jeśli chcesz przedstawić bardzo różne informacje lub przepływy pracy odwiedzającym dla urządzeń przenośnych. Pozostała część tej sekcji opisuje to podejście szczegółowo.

Przy założeniu, że masz już aplikację formularzy sieci Web zaprojektowaną pod kątem przeglądarek klasycznych, najprostszym sposobem, aby to zrobić, jest utworzenie podfolderu o nazwie "Mobile" w projekcie i skompilowanie stron mobilnych w tym miejscu. Można utworzyć całą podwitrynę z własnymi stronami wzorcowymi, arkuszami stylów i stronami przy użyciu wszystkich tych samych technik, które są używane dla innych aplikacji formularzy sieci Web. Nie trzeba tworzyć jeszcze odpowiednika dla *wszystkich* stron w witrynie klasycznej. Możesz wybrać podzbiór funkcji, który ma sens dla odwiedzających mobilnych.

Twoje strony mobilne mogą udostępniać wspólne zasoby statyczne (takie jak obrazy, pliki JavaScript lub CSS) przy użyciu zwykłych stron, jeśli chcesz. Ponieważ folder "mobilny" *nie* zostanie oznaczony jako oddzielna aplikacja, która jest hostowana w usługach IIS (tylko prosty podfolder stron formularzy sieci Web), będzie również udostępniać wszystkie takie same konfiguracje, dane sesji i inne infrastruktury, jak strony pulpitu.

> [!NOTE]
> Ponieważ takie podejście zwykle wiąże się z duplikowaniem kodu (strony mobilne są prawdopodobnie takie same ze stronami pulpitu), ważne jest, aby zastanowić się, że wszystkie typowe reguły biznesowe lub dane dostępu do danych zostaną wdrożone w udostępnionej warstwie lub usłudze źródłowej. W przeciwnym razie zostanie podwojony nakład pracy związany z tworzeniem i utrzymywaniem aplikacji.

#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Przekierowywanie odwiedzających użytkowników mobilnych do stron mobilnych

Często wygodnie jest przekierowywać odwiedzających do stron mobilnych tylko na *pierwszym* żądaniu w sesji przeglądania (a nie na każdym żądaniu w swojej sesji), ponieważ:

- Następnie możesz łatwo zezwolić odwiedzającym mobilnym na dostęp do stron pulpitu, jeśli chcesz, po prostu umieść link na stronie wzorcowej, który przejdzie do "wersji klasycznej". Gość nie zostanie przekierowany z powrotem do strony mobilnej, ponieważ nie jest to już pierwsze żądanie w swojej sesji.
- Pozwala to uniknąć ryzyka zakłócania żądań dla wszystkich zasobów dynamicznych współdzielonych między pulpitem i urządzeniami przenośnymi w witrynie (np. Jeśli masz wspólny formularz sieci Web, który jest wyświetlany na komputerze stacjonarnym i urządzeniach przenośnych, w elemencie IFRAME lub niektórych programach obsługi AJAX)

W tym celu można umieścić logikę przekierowania w **sesji\_metody startowej** . Na przykład Dodaj następującą metodę do pliku Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurowanie uwierzytelniania formularzy w celu uwzględnienia stron mobilnych

Należy pamiętać, że uwierzytelnianie formularzy wykonuje pewne założenia, w których można przekierować odwiedzających w trakcie procesu uwierzytelniania i po nim:

- Gdy użytkownik musi zostać uwierzytelniony, uwierzytelnianie formularzy przekieruje je do strony logowania pulpitu, niezależnie od tego, czy są one komputerem stacjonarnym czy użytkownikiem mobilnym (ponieważ ma tylko pojęcie *jednego* adresu URL logowania). Przy założeniu, że chcesz stylować stronę logowania mobilnego inaczej, musisz rozszerzyć stronę logowania pulpitu, aby przekierować użytkowników mobilnych na osobną stronę logowania do urządzeń przenośnych. Na przykład Dodaj następujący kod na stronie logowania do **pulpitu** : 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Po pomyślnym zalogowaniu się użytkownika uwierzytelnianie formularzy domyślnie przekieruje je do strony głównej pulpitu (ponieważ ma tylko koncepcję *jednej* strony domyślnej). Musisz wzmocnić swoją stronę logowania do urządzeń przenośnych, aby przekierować ją do strony głównej mobilnej po pomyślnym zalogowaniu. Na przykład Dodaj następujący kod do kodu strony logowania do **urządzeń przenośnych** : 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Ten kod zakłada, że strona ma kontrolkę serwer logowania o nazwie LoginUser, jak w domyślnym szablonie projektu.

### <a name="working-with-output-caching"></a>Praca z buforowaniem danych wyjściowych

Jeśli używasz buforowania danych wyjściowych, uważaj, aby użytkownik pulpitu mógł odwiedzać określony adres URL (powodując, że dane wyjściowe są buforowane), a następnie użytkownika mobilnego, który następnie odbiera zbuforowane dane wyjściowe na pulpicie. To ostrzeżenie ma wpływ na to, czy jesteś w trakcie zmieniania strony wzorcowej według typu urządzenia, czy implementacji całkowicie oddzielnych formularzy sieci Web dla każdego typu urządzenia.

Aby uniknąć tego problemu, można polecić ASP.NET w celu zmiany wpisu pamięci podręcznej w zależności od tego, czy osoba odwiedzająca korzysta z urządzenia przenośnego. Dodaj parametr VaryByCustom do deklaracji OutputCache strony w następujący sposób:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Następnie zdefiniuj *isMobileDevice* jako niestandardowy parametr pamięci podręcznej, dodając następujące zastąpienie metody do pliku Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Dzięki temu osoby odwiedzające nie odbierają danych wyjściowych wcześniej umieszczonych w pamięci podręcznej przez użytkownika pulpitu.

### <a name="a-working-example"></a>Przykład roboczy

Aby zapoznać się z tymi technikami, Pobierz [przykłady kodu tego dokumentu](https://docs.microsoft.com/aspnet/mobile/overview). Aplikacja Przykładowa formularzy sieci Web automatycznie przekierowuje użytkowników mobilnych do zestawu stron dla urządzeń przenośnych w podfolderze o nazwie Mobile. Znaczniki i style tych stron są lepiej zoptymalizowane pod kątem przeglądarek dla urządzeń przenośnych, ponieważ są one widoczne na następujących zrzutach ekranu:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Aby uzyskać więcej porad na temat optymalizowania znaczników i CSS w przeglądarkach dla urządzeń przenośnych, zobacz sekcję "Style mobilne strony dla przeglądarek mobilnych" w dalszej części tego dokumentu.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Jak aplikacje ASP.NET MVC mogą przedstawiać strony specyficzne dla urządzeń przenośnych

Ze względu na to, że wzorzec programu Model-View-Controller oddziela logikę aplikacji (w kontrolerach) z logiki prezentacji (w widokach), można wybrać jedną z następujących metod obsługi urządzeń przenośnych w kodzie po stronie serwera:

1. ***używać tych samych kontrolerów i widoków zarówno dla przeglądarek klasycznych, jak i mobilnych, ale renderuje widoki z różnymi układami Razor w zależności od typu urządzenia *.** Ta opcja działa najlepiej, jeśli są wyświetlane identyczne dane na wszystkich urządzeniach, ale po prostu chcą podawać różne arkusze stylów CSS lub zmieniać kilka elementów HTML najwyższego poziomu dla urządzeń przenośnych.
2. ***Używaj tych samych kontrolerów zarówno dla przeglądarek klasycznych, jak i mobilnych, ale Renderuj różne widoki w zależności od typu urządzenia***. Ta opcja działa najlepiej, jeśli są wyświetlane mniej więcej te same dane i zapewniają te same przepływy pracy dla użytkowników końcowych, ale chcemy renderować bardzo różne znaczniki HTML w celu dopasowania do używanego urządzenia.
3. ***utworzyć oddzielne obszary dla przeglądarek klasycznych i mobilnych, implementując niezależne kontrolery i widoki dla każdego *.** Ta opcja działa najlepiej, jeśli są wyświetlane różne ekrany zawierające różne informacje i prowadzące użytkownika przez różne przepływy pracy zoptymalizowane pod kątem ich typu urządzenia. Może to oznaczać powtarzanie kodu, ale można zminimalizować to przez rozpowszechnienie typowej logiki do warstwy podstawowej lub usługi.

Jeśli chcesz skorzystać z **pierwszej** opcji i różnią się tylko układem Razor dla każdego typu urządzenia, jest to bardzo proste. Zmodyfikuj plik \_ViewStart. cshtml w następujący sposób:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Teraz można utworzyć układ specyficzny dla urządzeń przenośnych o nazwie \_LayoutMobile. cshtml z użyciem struktury strony i reguł CSS zoptymalizowanych pod kątem urządzeń przenośnych.

Jeśli chcesz wykonać **drugą** opcję Renderuj całkowicie różne widoki zgodnie z typem urządzenia gościa, zobacz [wpis w blogu Scott Hanselman](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

Pozostała część tego dokumentu koncentruje się na **trzeciej** opcji — Tworzenie oddzielnych kontrolerów *i* widoków dla urządzeń przenośnych — dzięki temu można kontrolować, jaki podzestaw funkcji jest oferowany dla odwiedzających mobilnych.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Konfigurowanie obszaru mobilnego w aplikacji ASP.NET MVC

Możesz dodać obszar o nazwie "Mobile" do istniejącej aplikacji ASP.NET MVC w zwykły sposób: kliknij prawym przyciskiem myszy nazwę projektu w Eksplorator rozwiązań, a następnie wybierz polecenie Dodaj obszar à. Następnie można dodać kontrolery i widoki w taki sam sposób, jak w przypadku każdego innego obszaru w aplikacji ASP.NET MVC. Na przykład Dodaj do swojego obszaru przenośnego nowy kontroler o nazwie HomeController, który będzie pełnił rolę strony głównej dla odwiedzających mobilnych.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Upewnienie się, że adres URL/Mobile dociera do mobilnej strony głównej

Jeśli chcesz, aby adres URL/Mobile się do akcji index na HomeController w obszarze przenośnym, musisz wprowadzić dwa małe zmiany w konfiguracji routingu. Najpierw Zaktualizuj swoją klasę MobileAreaRegistration, tak aby HomeController jest domyślnym kontrolerem w obszarze mobilnym, jak pokazano w poniższym kodzie:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Oznacza to, że Strona główna urządzenia przenośnego będzie teraz dostępna w witrynie/Mobile, a nie/Mobile/Home, ponieważ "Strona główna" jest teraz niejawnie domyślną nazwą kontrolera dla stron mobilnych.

Następnie należy zauważyć, że przez dodanie drugiego HomeController do aplikacji (tj. urządzenia przenośnego, poza istniejącym pulpitem), zostanie uszkodzona zwykła Strona główna pulpitu. Wystąpił błąd "*znaleziono wiele typów zgodnych z kontrolerem o nazwie" Home "* ". Aby rozwiązać ten problem, zaktualizuj konfigurację routingu najwyższego poziomu (w Global.asax.cs), aby określić, że komputer stacjonarny HomeController powinien mieć priorytet w przypadku niejednoznaczności:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Teraz ten błąd zostanie wysunięty, a adres URL http:\/\/*yoursite*/będzie docierał do strony głównej pulpitu, a protokół http:\/\/*yoursite*/Mobile/zostanie osiągnięty na stronie głównej urządzenia przenośnego.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Przekierowywanie odwiedzających urządzenia przenośne do obszaru komórkowego

Istnieje wiele różnych punktów rozszerzalności w ASP.NET MVC, dlatego istnieje wiele możliwych sposobów iniekcji logiki przekierowania. Jedną z opcji zapełniania jest utworzenie atrybutu filtru [RedirectMobileDevicesToMobileArea], który wykonuje przekierowanie w przypadku spełnienia następujących warunków:

1. Jest to pierwsze żądanie w sesji użytkownika (tj. Session. IsNewSession jest równe true)
2. Żądanie pochodzi z przeglądarki mobilnej (tj., żądanie. browser. IsMobileDevice jest równe true)
3. Użytkownik nie zażądał jeszcze zasobu w obszarze mobilnym (tj. część *ścieżki* adresu URL nie zaczyna się od/Mobile).

Przykład do pobrania dołączony do tego oficjalnego dokumentu zawiera implementację tej logiki. Jest ona zaimplementowana jako filtr autoryzacji pochodzący z AuthorizeAttribute, co oznacza, że może działać poprawnie nawet w przypadku używania buforowania danych wyjściowych (w przeciwnym razie, jeśli osoba odwiedzająca komputer najpierw uzyskuje dostęp do określonego adresu URL, dane wyjściowe pulpitu mogą być buforowane, a następnie obsłużone do kolejne osoby odwiedzające Twoje urządzenia przenośne).

Ponieważ jest to filtr, można wybrać jeden z nich, aby zastosować go do określonych kontrolerów i akcji, na przykład,

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… można też zastosować je do wszystkich kontrolerów i akcji jako *Filtr globalny* MVC 3 w pliku Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

Przykład do pobrania pokazuje również, jak można utworzyć podklasy tego atrybutu, który przekierowuje do określonych lokalizacji w obszarze mobilnym. Oznacza to, na przykład, można:

- Zarejestruj filtr globalny, jak pokazano powyżej, który domyślnie wysyła odwiedzających do mobilnej strony głównej.
- Zastosuj również specjalny filtr [RedirectMobileDevicesToMobileProductPage] do akcji "Wyświetl produkt", która umożliwia odwiedzającym odwiedzanie urządzeń przenośnych do mobilnej wersji dowolnej strony produktu, której zażądał.
- Zastosuj również inne specjalne podklasy filtru do określonych akcji, przekierowując odwiedzających mobilnych do odpowiedniej strony mobilnej

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurowanie uwierzytelniania formularzy w celu uwzględnienia stron mobilnych

W przypadku korzystania z uwierzytelniania formularzy należy pamiętać, że gdy użytkownik musi się zalogować, automatycznie przekierowuje użytkownika do pojedynczego adresu URL logowania, który domyślnie jest **/Account/LogOn**. Oznacza to, że użytkownicy mobilni mogą zostać przekierowani do akcji logowania na pulpicie.

Aby uniknąć tego problemu, należy dodać logikę do akcji logowania do komputera stacjonarnego, aby ponownie przekierować użytkowników mobilnych do akcji "Logowanie na urządzeniu przenośnym". Jeśli używasz domyślnego szablonu aplikacji ASP.NET MVC, zaktualizuj akcję logowania elementu AccountController w następujący sposób:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… a następnie Zaimplementuj odpowiednią akcję "Zaloguj się" dla urządzenia przenośnego na kontrolerze o nazwie elementu AccountController w obszarze mobilnym.

### <a name="working-with-output-caching"></a>Praca z buforowaniem danych wyjściowych

W przypadku korzystania z filtru [OutputCache] należy wymusić, aby wpis pamięci podręcznej był zależny od typu urządzenia. Na przykład Napisz:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Następnie Dodaj następującą metodę do pliku Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Dzięki temu osoby odwiedzające nie odbierają danych wyjściowych wcześniej umieszczonych w pamięci podręcznej przez użytkownika pulpitu.

### <a name="a-working-example"></a>Przykład roboczy

Aby zapoznać się z tymi technikami, Pobierz ten dokument z kodem zawierającym [próbki](https://docs.microsoft.com/aspnet/mobile/overview). Przykład obejmuje aplikację ASP.NET MVC 3 (Release Candidate) ulepszoną w celu obsługi urządzeń przenośnych przy użyciu opisanych powyżej metod.

## <a name="further-guidance-and-suggestions"></a>Dalsze wskazówki i sugestie

Poniższa dyskusja dotyczy zarówno formularzy sieci Web, jak i deweloperów MVC, którzy korzystają z technik opisanych w tym dokumencie.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Ulepszanie logiki przekierowania przy użyciu programu 51Degrees.mobi Foundation

Logika przekierowania pokazana w tym dokumencie może być idealnie wystarczająca dla aplikacji, ale nie będzie działać, jeśli konieczne jest wyłączenie sesji lub z przeglądarkami mobilnymi, które odrzucają pliki cookie (nie mogą to być sesje), ponieważ nie będzie wiadomo, czy podane żądanie jest pierwszy z nich.

Wiesz już, jak program Open Source 51Degrees.mobi Foundation może poprawić dokładność ASP. Wykrywanie przeglądarki sieci. Ponadto ma wbudowaną możliwość przekierowania odwiedzających urządzenia przenośne do określonych lokalizacji skonfigurowanych w pliku Web. config. W zależności od sesji ASP.NET (i w związku z tym pliki cookie) jest możliwe przechowanie tymczasowego dziennika skrótów dla nagłówków HTTP i adresów IP odwiedzających, dlatego wie, czy każde żądanie jest pierwszym z danego Vistor.

Następujący element został dodany do sekcji fiftyOne w pliku Web. config przekieruje pierwsze żądanie ze wykrytego urządzenia przenośnego na stronę ~/Mobile/Default.aspx. Wszystkie żądania stron znajdujących się w folderze mobilnym *nie* będą przekierowywane niezależnie od typu urządzenia. Jeśli urządzenie przenośne było nieaktywne przez okres 20 minut lub więcej, urządzenie zostanie zapomniane, a kolejne żądania będą traktowane jako nowe na potrzeby przekierowania.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Aby uzyskać więcej informacji, zobacz [dokumentację programu 51Degrees.mobi Foundation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> *Możesz* użyć funkcji przekierowywania programu 51Degrees.mobi Foundation w aplikacjach ASP.NET MVC, ale musisz zdefiniować konfigurację przekierowania w postaci zwykłych adresów URL, a nie jako parametrów routingu lub przez umieszczenie filtrów MVC w akcjach. Jest to spowodowane tym, że (w czasie pisania) 51Degrees.mobi Foundation nie rozpoznaje filtrów ani routingu.

### <a name="disabling-transcoders-and-proxy-servers"></a>Wyłączanie transkoderów i serwerów proxy

Operatorzy sieci mobilnej mają dwa szerokie cele w podejściu do mobilnego Internetu:

1. Podaj możliwie tyle zawartość
2. Maksymalizuj liczbę klientów, którzy mogą udostępniać ograniczoną przepustowość sieci radiowej

Ze względu na to, że większość stron sieci Web została zaprojektowana pod kątem dużych ekranów o rozmiarze stacjonarnym i szybkich połączeń szerokopasmowych, wiele operatorów używa *transkoderów* lub *serwerów proxy* , które dynamicznie zmieniają zawartość sieci Web. Mogą modyfikować znaczniki HTML lub arkusze CSS na mniejszych ekranach (szczególnie w przypadku "telefonów z funkcjami", które nie korzystają z mocy obliczeniowej do obsługi złożonych układów) i mogą rekompresować obrazy (znacznie zmniejszając ich jakość), aby zwiększyć szybkość dostarczania stron.

Ale jeśli podjęto wysiłki w celu utworzenia wersji witryny zoptymalizowanej pod kątem urządzeń przenośnych, prawdopodobnie nie chcesz, aby operator sieci mógł się z nim zakłócać. Możesz dodać następujący wiersz do strony\_zdarzenia ładowania w dowolnym formularzu sieci Web ASP.NET:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

Lub w przypadku kontrolera ASP.NET MVC można dodać poniższe przesłonięcie metody, aby odnosił się do wszystkich akcji na tym kontrolerze:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

Wynikowy komunikat HTTP informuje transformacje zgodne ze standardem W3C i serwery proxy, aby nie można było zmieniać zawartości. Oczywiście nie ma gwarancji, że operatorzy sieci komórkowej będą respektują ten komunikat.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Style stron mobilnych dla przeglądarek mobilnych

Ten dokument wykracza poza zakres tego dokumentu w celu opisania szczegółowych informacji o tym, jakie rodzaje znaczników HTML działają poprawnie lub jakie techniki projektowania sieci Web maksymalizują użyteczność na konkretnych urządzeniach. Umożliwia znalezienie wystarczająco prostego układu zoptymalizowanego pod kątem ekranu o rozmiarze mobilnym, bez konieczności korzystania z niezawodnych wskazówek dotyczących pozycjonowania HTML lub CSS. Jedną z ważnych technik oznaczanie, że jest to *tag okienka ekranu*.

Niektóre nowoczesne przeglądarki dla urządzeń przenośnych — w miarę wyświetlania stron sieci Web przeznaczonych dla monitorów stacjonarnych, renderowanie strony na wirtualnej kanwie, nazywanej również "okienkiem ekranu" (np. w przypadku wirtualnego okienka ekranu jest 980 pikseli na telefonie iPhone i 850 pikseli szerokości w programie Opera Mobile — domyślnie), a następnie Skaluj wynik w dół w celu dopasowania do pikseli fizycznych ekranu. Użytkownik może następnie powiększyć i przesunąć wokół tego okienka ekranu. Pozwala to na wyświetlenie strony w przeglądarce w odpowiednim układzie, ale ma także wady, które wymuszają powiększanie i panoramowanie, co jest niewygodne dla użytkownika. Jeśli projektujesz aplikacje dla urządzeń przenośnych, lepiej jest zaprojektować go pod kątem wąskiego ekranu, aby nie było konieczne powiększanie ani przewijanie w poziomie.

Sposób poinformowania przeglądarki mobilnej o szerokości okienka ekranu, który powinien znajdować się za pomocą niestandardowego znacznika *okienka ekranu* . Na przykład, jeśli dodasz Poniższy kod do sekcji nagłówka strony,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… następnie Obsługa przeglądarek smartphone spowoduje przetworzenie układu strony na całej kanwie wirtualnej o 480 pikseli. Oznacza to, że jeśli elementy HTML definiują ich szerokość w warunkach procentowych, wartości procentowe będą interpretowane w odniesieniu do szerokości o 480 pikseli, a nie domyślnej szerokości okienka ekranu. W związku z tym użytkownik może mniej powiększać i kadrować w poziomie, znacznie ulepszając środowisko przeglądania mobilnego.

Jeśli chcesz, aby szerokość okienka ekranu była zgodna z pikselami fizycznymi urządzenia, możesz określić następujące elementy:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Aby ta funkcja działała poprawnie, nie można jawnie wymusić, aby elementy przekroczą tę Szerokość (np. przy użyciu atrybutu *Width* lub właściwości CSS). w przeciwnym razie w przeglądarce zostanie wymuszone użycie większego okienka ekranu niezależnie od siebie. Zobacz również: [więcej informacji na temat niestandardowego znacznika okienka ekranu](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Większość nowoczesnych telefonów Smartphone obsługuje *dwie orientacje*: mogą być przechowywane w trybie pionowym lub poziomym. Dlatego ważne jest, aby nie wprowadzać założeń o szerokości ekranu (w pikselach). Nie zakładaj nawet, że szerokość ekranu jest stała, ponieważ użytkownik może zmienić orientację urządzenia na stronie.

Starsze urządzenia z systemem Windows Mobile i BlackBerry mogą również akceptować następujące meta tagi w nagłówku strony, aby poinformować o tym, że zawartość została zoptymalizowana pod kątem urządzeń przenośnych i dlatego nie powinna być przekształcona.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Dodatkowe materiały

Aby zapoznać się z listą emulatorów i symulatorów urządzeń przenośnych, których można użyć do testowania aplikacji sieci Web ASP.NET Mobile, zobacz stronę [symulowanie popularnych urządzeń przenośnych do testowania](../mobile/device-simulators.md).

## <a name="credits"></a>Środki

- Autor podstawowy: Steven Sanderson
- Recenzenci/dodatkowi autorzy zawartości: Kuba Rosewell, Mikael Söderström, Scott Hanselman, Scott myśliwy

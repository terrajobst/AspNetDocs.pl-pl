---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
title: Podstawowe różnice między usługami IIS i ASP.NET Development Server (VB) | Microsoft Docs
author: rick-anderson
description: W przypadku lokalnego testowania aplikacji ASP.NET można korzystać z serwera ASP.NET Development Web. Jednak produkcyjna witryna sieci Web jest najprawdopodobniej pow...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 090e9205-52f3-4d72-ae31-44775b8b8421
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 880bb403e671446a77d7eebccf578a1dc714d1f9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74586524"
---
# <a name="core-differences-between-iis-and-the-aspnet-development-server-vb"></a>Podstawowe różnice między usługami IIS a programem ASP.NET Development Server (VB)

przez [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Pobierz kod](https://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_VB.zip) lub [Pobierz plik PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_vb.pdf)

> W przypadku lokalnego testowania aplikacji ASP.NET można korzystać z serwera ASP.NET Development Web. Jednak produkcyjna witryna sieci Web jest najbardziej prawdopodobną obsługą usług IIS. Istnieją pewne różnice między tym, jak te serwery sieci Web obsługują żądania, a różnice te mogą mieć ważne konsekwencje. Ten samouczek Eksplorowanie niektórych bardziej niemieckich różnic.

## <a name="introduction"></a>Wprowadzenie

Za każdym razem, gdy użytkownik odwiedza aplikację ASP.NET, jej przeglądarka wysyła żądanie do witryny sieci Web. To żądanie jest pobierane przez oprogramowanie serwera sieci Web, które koordynuje się ze środowiskiem uruchomieniowym ASP.NET, aby wygenerować i zwrócić zawartość dla żądanego zasobu. [Internetowy **i** nformation **S** ervices (IIS) ](http://en.wikipedia.org/wiki/Internet_Information_Services) to zestaw usług, które zapewniają wspólne funkcje internetowe dla serwerów z systemem Windows. Usługi IIS to najczęściej używany serwer sieci Web dla aplikacji ASP.NET w środowiskach produkcyjnych. najprawdopodobniej serwer sieci Web jest używany przez dostawcę hosta sieci Web do obsługi aplikacji ASP.NET. Usługi IIS mogą być również używane jako oprogramowanie serwera sieci Web w środowisku programistycznym, chociaż powoduje to zainstalowanie usług IIS i ich prawidłowe skonfigurowanie.

ASP.NET Development Server to alternatywna opcja serwera sieci Web dla środowiska programistycznego. jest ona dostarczana z usługą i jest zintegrowana z programem Visual Studio. Jeśli aplikacja sieci Web nie została skonfigurowana do korzystania z usług IIS, serwer programistyczny ASP.NET jest automatycznie uruchamiany i używany jako serwer sieci Web podczas pierwszego odwiedzania strony sieci Web z poziomu programu Visual Studio. Pokazowe aplikacje sieci Web, które zostały utworzone z powrotem w samouczku [*Określanie, które pliki muszą zostać wdrożone*](determining-what-files-need-to-be-deployed-vb.md) , były zarówno aplikacjami sieci Web opartymi na systemie plików, które nie zostały skonfigurowane do korzystania z usług IIS. W związku z tym podczas odwiedzania dowolnej z tych witryn sieci Web w programie Visual Studio jest używany serwer deweloperski ASP.NET.

W idealnym świecie środowiska deweloperskie i produkcyjne będą takie same. Jednak zgodnie z opisem w powyższym samouczku w środowiskach nie jest nietypowa sytuacja, w której środowiska mają różne ustawienia konfiguracji. Użycie innego oprogramowania serwera sieci Web w środowiskach dodaje kolejną zmienną, która musi zostać wzięta pod uwagę podczas wdrażania aplikacji. W tym samouczku omówiono kluczowe różnice między usługami IIS i ASP.NET Development Server. Ze względu na różnice występują sytuacje, w których kod, który działa bezproblemowo w środowisku programistycznym, zgłasza wyjątek lub działa inaczej w przypadku wykonywania w produkcji.

## <a name="security-context-differences"></a>Różnice kontekstu zabezpieczeń

Każde oprogramowanie serwera sieci Web obsługuje żądanie przychodzące, które kojarzy żądanie z określonym kontekstem zabezpieczeń. Te informacje kontekstu zabezpieczeń są używane przez system operacyjny w celu ustalenia, jakie działania są dopuszczalne przez żądanie. Na przykład strona ASP.NET może zawierać kod, który rejestruje część komunikatu w pliku na dysku. Aby ta strona ASP.NET mogła zostać wykonana bez błędu, kontekst zabezpieczeń musi mieć odpowiednie uprawnienia na poziomie systemu plików, czyli uprawnienia do zapisu w tym pliku.

Serwer programistyczny ASP.NET kojarzy żądania przychodzące z kontekstem zabezpieczeń aktualnie zalogowanego użytkownika. Jeśli użytkownik jest zalogowany na pulpicie jako administrator, strony ASP.NET obsługiwane przez serwer deweloperski ASP.NET będą mieć takie same uprawnienia dostępu jak administrator. Jednak żądania ASP.NET obsługiwane przez usługi IIS są skojarzone z określonym kontem komputera. Domyślnie konto komputera usługi sieciowej jest używane przez usługi IIS w wersji 6 i 7, chociaż dostawca hosta sieci Web mógł skonfigurować unikatowe konto dla każdego klienta. Co więcej, Twój dostawca hosta sieci Web mógł mieć ograniczone uprawnienia do tego konta komputera. Wynika to z faktu, że strony sieci Web, które są wykonywane bez błędów w środowisku programistycznym, mogą generować wyjątki związane z autoryzacją, które są hostowane w środowisku produkcyjnym.

Aby wyświetlić ten typ błędu w akcji, utworzyłem stronę w witrynie sieci Web przeglądy książki, która tworzy plik na dysku, który przechowuje najnowszą datę i godzinę osoby, która przegląda *ASP.NET 3,5 w 24-godzinnym* przeglądzie. Aby wykonać te czynności, Otwórz stronę `~/Tech/TYASP35.aspx` i Dodaj następujący kod do programu obsługi zdarzeń `Page_Load`:

[!code-vb[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample1.vb)]

> [!NOTE]
> [Metoda`File.WriteAllText`](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) tworzy nowy plik, jeśli nie istnieje, a następnie zapisuje określoną zawartość. Jeśli plik już istnieje, jego istniejąca zawartość jest zastępowana.

Przejdź do strony *nauka ASP.NET 3,5 w 24-godzinnej* witrynie przeglądu w środowisku programistycznym przy użyciu serwera deweloperskiego ASP.NET. Przy założeniu, że użytkownik jest zalogowany na komputerze przy użyciu konta, które ma odpowiednie uprawnienia do tworzenia i modyfikowania pliku tekstowego w katalogu głównym aplikacji sieci Web, przegląd książki jest wyświetlany tak samo jak poprzednio, ale za każdym razem, gdy strona jest odwiedzana, Data i godzina i adres IP użytkownika są przechowywane w pliku `LastTYASP35Access.txt`. Wskaż w przeglądarce ten plik; powinien zostać wyświetlony komunikat podobny do przedstawionego na rysunku 1.

[![plik tekstowy zawiera ostatnią datę i godzinę odwiedzenia przeglądu książki&lt;](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image1.png)

**Rysunek 1**: plik tekstowy zawiera ostatnią datę i godzinę odwiedzenia przeglądu księgi ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image3.png))

Wdróż aplikację sieci Web w środowisku produkcyjnym, a następnie odwiedź zawartą *naukę w witrynie ASP.NET 3,5 na stronie przeglądu książki na 24 godziny* . W tym momencie powinna zostać wyświetlona strona przegląd książki jako normalna lub komunikat o błędzie przedstawiony na rysunku 2. Niektórzy dostawcy hosta sieci Web przyznają uprawnienia do zapisu dla anonimowego konta ASP.NET komputera, w tym przypadku strona będzie działała bez błędu. Jeśli jednak dostawca hosta sieci Web zabroni dostępu do zapisu dla konta anonimowego, zostanie zgłoszony [wyjątek`UnauthorizedAccessException`](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) , gdy strona `TYASP35.aspx` próbuje zapisać bieżącą datę i godzinę do pliku `LastTYASP35Access.txt`.

[![domyślne konto komputera używane przez usługi IIS nie ma uprawnień do zapisu w systemie plików](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image4.png)

**Rysunek 2**: domyślne konto komputera używane przez usługi IIS nie ma uprawnień do zapisu w systemie plików ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image6.png))

Dobrą nowością jest to, że większość dostawców hosta sieci Web posiada pewne uprawnienia do określania uprawnień systemu plików w witrynie sieci Web. Przyznaj Anonimowemu kontu ASP.NET dostęp do zapisu w katalogu głównym, a następnie ponownie odwiedź stronę przegląd książki. (W razie potrzeby skontaktuj się z dostawcą hosta sieci Web, aby uzyskać pomoc dotyczącą przyznawania uprawnień do zapisu domyślnego konta ASP.NET). Tym razem strona powinna zostać załadowana bez błędu, a plik `LastTYASP35Access.txt` powinien zostać utworzony pomyślnie.

W tym miejscu jest to spowodowane tym, że serwer programistyczny ASP.NET działa pod innym kontekstem zabezpieczeń niż IIS, istnieje możliwość, że strony ASP.NET, które odczytują lub zapisują w systemie plików, odczytują lub zapisują w dzienniku zdarzeń systemu Windows albo odczytują lub zapisują w systemie Windows  rejestr będzie działał zgodnie z oczekiwaniami podczas opracowywania, ale generować wyjątki w środowisku produkcyjnym. Podczas kompilowania aplikacji sieci Web, która zostanie wdrożona w udostępnionym środowisku hostingu sieci Web, nie Odczytuj ani nie zapisuj w dzienniku zdarzeń ani w rejestrze systemu Windows. Należy również zwrócić uwagę na wszystkie strony ASP.NET, które odczytują lub zapisują w systemie plików, ponieważ może być konieczne przyznanie uprawnień odczytu i zapisu w odpowiednich folderach w środowisku produkcyjnym.

## <a name="differences-on-serving-static-content"></a>Różnice dotyczące obsługi zawartości statycznej

Inna podstawowa różnica między usługami IIS i ASP.NET Development Server to sposób obsługi żądań zawartości statycznej. Każde żądanie, które znajduje się na serwerze deweloperskim ASP.NET, niezależnie od tego, czy dla strony ASP.NET, obrazu, czy pliku JavaScript, jest przetwarzane przez środowisko uruchomieniowe ASP.NET. Domyślnie usługi IIS wywołujący środowisko uruchomieniowe ASP.NET, gdy żądanie dotyczy zasobu ASP.NET, takiego jak strona sieci Web ASP.NET, usługa sieci Web i tak dalej. Żądania dotyczące statycznych obrazów zawartości, plików CSS, plików języka JavaScript, plików PDF, plików ZIP i takich jak są pobierane przez usługi IIS bez udziału środowiska uruchomieniowego ASP.NET. (Możliwe jest nadanie usług IIS pracy z środowiskiem uruchomieniowym ASP.NET w przypadku obsługi zawartości statycznej. Aby uzyskać więcej informacji, zapoznaj się z sekcją "wykonywanie uwierzytelniania opartego na formularzach i uwierzytelnianie adresów URL w plikach statycznych w tym samouczku".

Środowisko uruchomieniowe ASP.NET wykonuje kilka kroków w celu wygenerowania żądanych zawartości, w tym uwierzytelniania (identyfikowania obiektu żądającego) i autoryzacji (określenie, czy obiekt żądający ma uprawnienia do wyświetlania żądanych treści). Popularną formą uwierzytelniania jest *uwierzytelnianie oparte na formularzach*, w którym użytkownik jest identyfikowany przez wprowadzenie ich poświadczeń — zwykle nazwy użytkownika i hasła do pól tekstowych na stronie sieci Web. Podczas sprawdzania poprawności poświadczeń witryna sieci Web przechowuje plik cookie *biletu uwierzytelniania* w przeglądarce użytkownika, który jest wysyłany wraz z każdym kolejnym żądaniem do witryny sieci Web i jest używany do uwierzytelniania użytkownika. Ponadto możliwe jest określenie reguł *autoryzacji adresów URL* , które określają, którzy użytkownicy mogą lub nie mogą uzyskać dostępu do określonego folderu. Wiele witryn sieci Web ASP.NET korzysta z uwierzytelniania opartego na formularzach i autoryzacji adresów URL do obsługi kont użytkowników i definiowania części lokacji, które są dostępne tylko dla uwierzytelnionych użytkowników lub użytkowników należących do określonej roli.

> [!NOTE]
> Dokładne badanie ASP. Uwierzytelnianie oparte na formularzach, Autoryzacja adresów URL i inne funkcje związane z kontami użytkowników usługi NET, należy zapoznać się z [samouczkami zabezpieczeń witryny sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

Weź pod uwagę witrynę sieci Web, która obsługuje konta użytkowników przy użyciu autoryzacji opartej na formularzach i ma folder, który korzysta z autoryzacji adresów URL, jest skonfigurowany tak, aby zezwalał tylko uwierzytelnionym użytkownikom. Załóżmy, że ten folder zawiera ASP.NET strony i pliki PDF oraz że zamiarem jest to, że tylko uwierzytelnieni użytkownicy mogą wyświetlać te pliki PDF.

Co się stanie, jeśli osoba odwiedzająca próbuje wyświetlić jeden z tych plików PDF, wprowadzając adres URL bezpośrednio na pasku adresu przeglądarki? Aby dowiedzieć się, Utwórzmy nowy folder w witrynie przeglądy książki, Dodaj pliki PDF i Skonfiguruj witrynę tak, aby korzystała z autoryzacji adresów URL, aby uniemożliwić użytkownikom anonimowym odwiedzanie tego folderu. Po pobraniu aplikacji demonstracyjnej zobaczysz, że został utworzony folder o nazwie `PrivateDocs` i dodano plik PDF z [samouczków zabezpieczeń witryny mój witryna sieci Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (jak można się zamontować!). Folder `PrivateDocs` zawiera również plik `Web.config`, który określa reguły autoryzacji adresów URL, aby odmówić anonimowych użytkowników:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample2.xml)]

Na koniec skonfiguruję aplikację sieci Web tak, aby korzystała z uwierzytelniania opartego na formularzach, aktualizując plik Web. config w katalogu głównym, zastępując:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample3.xml)]

Się

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample4.xml)]

Korzystając z serwera deweloperskiego ASP.NET, odwiedź witrynę i wprowadź bezpośredni adres URL do jednego z plików PDF na pasku adresu przeglądarki. Jeśli pobrano witrynę sieci Web powiązaną z tym samouczkiem, adres URL powinien wyglądać następująco: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Wprowadzenie tego adresu URL na pasku adresu powoduje, że przeglądarka wysyła żądanie do serwera ASP.NET Development dla tego pliku. Serwer programistyczny ASP.NET to żądanie do środowiska uruchomieniowego ASP.NET w celu przetworzenia. Ponieważ jeszcze nie zarejestrowano się, a ponieważ `Web.config` w folderze `PrivateDocs` jest skonfigurowany tak, aby odmówić dostępu anonimowego, środowisko uruchomieniowe ASP.NET automatycznie przekierowuje nam do strony logowania, `Login.aspx` (patrz rysunek 3). Podczas przekierowywania użytkownika do strony logowania ASP.NET zawiera parametr QueryString `ReturnUrl`, który wskazuje stronę, którą użytkownik próbuje wyświetlić. Po pomyślnym zalogowaniu się użytkownika można zwrócić do tej strony.

[![nieautoryzowani użytkownicy są automatycznie przekierowywani do strony logowania](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image7.png)

**Rysunek 3**. nieautoryzowani użytkownicy są automatycznie przekierowywani do strony logowania ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image9.png))

Teraz zobaczmy, jak to działa w środowisku produkcyjnym. Wdróż aplikację i wprowadź bezpośredni adres URL do jednego z plików PDF w folderze `PrivateDocs` w środowisku produkcyjnym. Zostanie wyświetlony komunikat z pytaniem do przeglądarki w celu wysłania żądania usług IIS dla tego pliku. Ponieważ wymagany jest plik statyczny, program IIS pobiera i zwraca plik bez wywoływania środowiska uruchomieniowego ASP.NET. W związku z tym nie zostało wykonane sprawdzanie autoryzacji adresów URL; zawartość prywatnego pliku PDF supposedly są dostępne dla każdego, kto zna bezpośredni adres URL do pliku.

[Użytkownicy anonimowi ![mogą pobrać prywatne pliki PDF, wprowadzając bezpośredni adres URL do pliku](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image10.png)

**Rysunek 4**. Użytkownicy anonimowi mogą pobrać prywatne pliki PDF, wprowadzając bezpośredni adres URL do pliku ([kliknij, aby wyświetlić obraz o pełnym rozmiarze](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image12.png))

### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Wykonywanie uwierzytelniania opartego na formularzach i uwierzytelnianie adresów URL w plikach statycznych przy użyciu usług IIS 7

Istnieje kilka technik, których można użyć do ochrony zawartości statycznej przed nieautoryzowanymi użytkownikami. Usługi IIS 7 wprowadziły *zintegrowany potok*, który cywilnego przepływ pracy usług IIS z przepływem pracy środowiska uruchomieniowego ASP.NET. W Nutshell można poinstruować usługi IIS, aby wywoływać moduły uwierzytelniania i autoryzacji środowiska uruchomieniowego ASP.NET do wszystkich żądań przychodzących (w tym zawartości statycznej, takiej jak pliki PDF). Skontaktuj się z dostawcą hosta sieci Web, aby dowiedzieć się, jak skonfigurować witrynę internetową tak, aby korzystała z zintegrowanego potoku.

Po skonfigurowaniu usług IIS do korzystania ze zintegrowanego potoku Dodaj następujące znaczniki do pliku `Web.config` w katalogu głównym:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample5.xml)]

Ten znacznik nakazuje usługom IIS 7 korzystanie z modułów uwierzytelniania i autoryzacji opartych na ASP.NET. Wdróż ponownie aplikację, a następnie ponownie odwiedź plik PDF. Tym razem, gdy usługi IIS obsługują żądanie, zapewnia logikę uwierzytelniania i autoryzacji środowiska uruchomieniowego ASP.NET w celu zbadania żądania. Ponieważ tylko uwierzytelnieni użytkownicy mają uprawnienia do wyświetlania zawartości w folderze `PrivateDocs`, anonimowe osoby odwiedzające są automatycznie przekierowywane do strony logowania (patrz z powrotem do rysunku 3).

> [!NOTE]
> Jeśli Twój dostawca hosta sieci Web nadal korzysta z usług IIS 6, nie można użyć funkcji zintegrowanego potoku. Obejście polega na umieszczeniu dokumentów prywatnych w folderze, który zabrania dostępu HTTP (np. `App_Data`), a następnie utworzyć stronę do obsłużenia tych dokumentów. Ta strona może być wywoływana `GetPDF.aspx`i ma przekazaną nazwę pliku PDF za pomocą parametru QueryString. Na stronie `GetPDF.aspx` należy najpierw sprawdzić, czy użytkownik ma uprawnienia do wyświetlania tego pliku, a jeśli tak, użyj metody [`Response.WriteFile(filePath)`](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) do wysłania zawartości żądanego pliku PDF z powrotem do żądającego klienta. Ta technika również będzie działała w przypadku usług IIS 7, jeśli nie chcesz włączyć zintegrowanego potoku.

## <a name="summary"></a>Podsumowanie

Aplikacje sieci Web w środowisku produkcyjnym są hostowane przy użyciu oprogramowania serwera sieci Web usług IIS firmy Microsoft. Jednak w środowisku programistycznym aplikacja może być hostowana przy użyciu usług IIS lub serwera deweloperskiego ASP.NET. W idealnym przypadku należy używać tego samego oprogramowania serwera sieci Web w obu środowiskach, ponieważ użycie innego oprogramowania dodaje kolejną zmienną w połączeniu. Jednak łatwość użycia serwera deweloperskiego ASP.NET sprawia, że jest to atrakcyjny wybór w środowisku programistycznym. Dobre wiadomości to fakt, że istnieje tylko kilka podstawowych różnic między usługami IIS a serwerem deweloperskim ASP.NET. w przypadku problemów z tymi różnicami można wykonać kroki w celu zapewnienia, że aplikacja działa i działa tak samo, niezależnie od naturalne.

Szczęśliwe programowanie!

### <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat tematów omówionych w tym samouczku, zapoznaj się z następującymi zasobami:

- [Integracja ASP.NET z usługami IIS 7,0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Korzystanie z uwierzytelniania forów ASP.NET w przypadku wszystkich typów zawartości w usługach IIS 7](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (wideo)
- [Serwery sieci Web w programie Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Poprzednie](common-configuration-differences-between-development-and-production-vb.md)
> [dalej](deploying-a-database-vb.md)

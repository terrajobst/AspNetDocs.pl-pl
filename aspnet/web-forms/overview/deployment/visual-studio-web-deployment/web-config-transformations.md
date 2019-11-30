---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'ASP.NET wdrażanie sieci Web przy użyciu programu Visual Studio: przekształcenia plików Web. config | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przez usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a9d39547c94a63003442ba6fe1257693dde24b05
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621788"
---
# <a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>ASP.NET wdrażanie sieci Web przy użyciu programu Visual Studio: przekształcenia plików Web. config

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przy użyciu programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje o serii, zobacz [pierwszy samouczek w serii](introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku pokazano, jak zautomatyzować proces zmiany pliku *Web. config* podczas wdrażania go w różnych środowiskach docelowych. Większość aplikacji ma ustawienia w pliku *Web. config* , które muszą być inne, gdy aplikacja jest wdrażana. Automatyzacja procesu wprowadzania tych zmian uniemożliwia wykonywanie ich ręcznie przy każdym wdrożeniu, co byłoby żmudnyme i podatne na błędy.

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Przekształcenia Web. config a Web Deploy parametry

Istnieją dwa sposoby automatyzacji procesu zmiany ustawień pliku *Web. config* : [przekształcenia Web. config](https://msdn.microsoft.com/library/dd465326.aspx) i [Web Deploy parametrów](https://msdn.microsoft.com/library/ff398068.aspx). Plik transformacji *Web. config* zawiera znacznik XML, który określa sposób zmiany pliku *Web. config* podczas jego wdrażania. Można określić różne zmiany dla określonych konfiguracji kompilacji i dla określonych profilów publikacji. Domyślną konfiguracją kompilacji są debugowanie i wydanie i można utworzyć niestandardowe konfiguracje kompilacji. Profil publikacji zwykle odpowiada środowisku docelowemu. (Więcej informacji o profilach publikowania można znaleźć w samouczku [wdrażanie do usług IIS jako środowisko testowe](deploying-to-iis.md) ).

Parametrów Web Deploy można użyć do określenia wielu różnych rodzajów ustawień, które muszą być skonfigurowane podczas wdrażania, w tym ustawień znalezionych w plikach *Web. config* . Gdy jest używany do określania zmian w pliku *Web. config* , Web Deploy parametry są bardziej skomplikowane do skonfigurowania, ale są przydatne, gdy nie znasz wartości do ustawienia do momentu wdrożenia. Na przykład w środowisku przedsiębiorstwa można utworzyć *pakiet wdrożeniowy* i przekazać go do osoby w dziale IT w celu zainstalowania jej w produkcji, a osoba musi mieć możliwość wprowadzania parametrów połączenia lub haseł, które nie są znane.

Aby zapoznać się z scenariuszem, który obejmuje ta seria samouczków, możesz uzyskać dostęp do wszystkich elementów, które należy wykonać w pliku *Web. config* , aby nie trzeba było używać parametrów Web Deploy. Skonfigurujesz niektóre przekształcenia, które różnią się w zależności od używanej konfiguracji kompilacji, a niektóre z nich różnią się w zależności od używanego profilu publikacji.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Określanie ustawień pliku Web. config na platformie Azure

Jeśli ustawienia pliku *Web. config* , które chcesz zmienić, znajdują się w `<connectionStrings>` lub `<appSettings>`, a Jeśli wdrażasz program w Web Apps Azure App Service, masz inną opcję automatyzacji zmian podczas wdrażania. Możesz wprowadzić ustawienia, które mają zostać zastosowane na platformie Azure, na karcie **Konfiguracja** na stronie Portal zarządzania dla aplikacji sieci Web (przewiń w dół do sekcji **Ustawienia aplikacji** i **Parametry połączenia** ). Podczas wdrażania projektu platforma Azure automatycznie stosuje zmiany. Aby uzyskać więcej informacji, zobacz [witryny sieci Web systemu Windows Azure: sposób działania ciągów aplikacji i parametrów połączenia](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Domyślne pliki transformacji

W **Eksplorator rozwiązań**rozwiń plik *Web. config* , aby wyświetlić pliki transformacji *Web. Debug. config* i *Web. release. config* , które są tworzone domyślnie dla dwóch domyślnych konfiguracji kompilacji.

![Web. config_transform_files](web-config-transformations/_static/image1.png)

Możesz tworzyć pliki transformacji dla niestandardowych konfiguracji kompilacji, klikając prawym przyciskiem myszy plik Web. config i wybierając pozycję **Dodaj transformacje konfiguracji** z menu kontekstowego. W tym samouczku nie trzeba tego robić, a opcja menu jest wyłączona, ponieważ nie utworzono żadnych niestandardowych konfiguracji kompilacji.

Później utworzysz trzy więcej plików transformacji, jeden z nich dla profilów publikacji test, proces przemieszczania i produkcji. Typowy przykład ustawienia, które można obsłużyć w pliku transformacji profilu publikowania, ponieważ zależy on od środowiska docelowego jest punktem końcowym WCF, który jest inny do testowania i produkcji. Pliki transformacji profilu publikowania można tworzyć w kolejnych samouczkach po utworzeniu profilów publikowania, z którymi się znajdują.

## <a name="disable-debug-mode"></a>Wyłącz tryb debugowania

Przykładem ustawienia, które zależy od konfiguracji kompilacji, a nie środowiska docelowego jest atrybut `debug`. W przypadku kompilacji wydania zwykle debugowanie jest wyłączone niezależnie od środowiska, w którym są wdrażane. W związku z tym Domyślnie szablony projektu programu Visual Studio tworzą pliki transformacji *Web. release. config* z kodem, który usuwa atrybut `debug` z elementu `compilation`. Oto domyślny *plik Web. release. config*: oprócz niektórych przykładowego kodu przekształcenia, który jest oznaczony jako komentarz, zawiera kod w `compilation` elemencie, który usuwa atrybut `debug`:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

Atrybut `xdt:Transform="RemoveAttributes(debug)"` określa, że chcesz, aby atrybut `debug` został usunięty z elementu `system.web/compilation` we wdrożonym pliku *Web. config* . Będzie to wykonywane za każdym razem, gdy zostanie wdrożona kompilacja wydania.

## <a name="limit-error-log-access-to-administrators"></a>Ogranicz dostęp do dziennika błędów do administratorów

Jeśli wystąpi błąd podczas uruchamiania aplikacji, zostanie wyświetlona strona błędu ogólnego zamiast wygenerowanej przez systemowej strony błędu, która używa [pakietu NuGet programu ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) do rejestrowania błędów i raportowania. Element `customErrors` w pliku *Web. config* aplikacji określa stronę błędu:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Aby wyświetlić stronę błędu, tymczasowo Zmień atrybut `mode` elementu `customErrors` z "RemoteOnly" na "on" i uruchom aplikację z programu Visual Studio. Zażądaj błędu przez zażądanie nieprawidłowego adresu URL, takiego jak *Studentsxxx. aspx*. Zamiast wygenerowanej przez program IIS strony błędu "nie można odnaleźć zasobu" zostanie wyświetlona strona *GenericErrorPage. aspx* .

![Strona błędu](web-config-transformations/_static/image2.png)

Aby wyświetlić dziennik błędów, Zastąp wszystko w adresie URL po numerze portu za pomocą biblioteki *ELMAH. axd* (na przykład `http://localhost:51130/elmah.axd`) i naciśnij klawisz ENTER:

![Strona ELMAH](web-config-transformations/_static/image3.png)

Nie zapomnij ustawić elementu `customErrors` z powrotem do trybu "RemoteOnly", gdy wszystko będzie gotowe.

Na komputerze deweloperskim wygodne jest umożliwienie bezpłatnego dostępu do strony dziennika błędów, ale w środowisku produkcyjnym, które byłyby zagrożeniem bezpieczeństwa. W przypadku lokacji produkcyjnej należy dodać regułę autoryzacji, która ogranicza dostęp do dziennika błędów do administratorów, i upewnić się, że ograniczenie działa tak, aby było ono również przetestowane i przemieszczane. W związku z tym jest to inna zmiana, która ma zostać zaimplementowana za każdym razem, gdy wdrażana jest kompilacja wydania, a więc należy do pliku *Web. release. config* .

Otwórz *plik Web. release. config* i Dodaj nowy element `location` bezpośrednio przed tagiem zamykającym `configuration`, jak pokazano poniżej.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

`Transform` wartość atrybutu "INSERT" powoduje, że ten element `location` zostanie dodany jako element równorzędny do wszystkich istniejących elementów `location` w pliku *Web. config* . (Istnieje już jeden `location` element, który określa reguły autoryzacji na stronie **kredyty aktualizacji** ).

Teraz możesz wyświetlić podgląd przekształcenia, aby upewnić się, że został poprawnie zakodowany.

W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy pozycję *Web. release. config* , a następnie kliknij pozycję **Podgląd transformacji**.

![Menu Przekształć Podgląd](web-config-transformations/_static/image4.png)

Zostanie otwarta strona, która wyświetla plik *Web. config* z lewej strony i opis wdrożonego pliku *Web. config* po prawej stronie z wyróżnionymi zmianami.

![Podgląd przekształcenia debugowania](web-config-transformations/_static/image5.png)

![Wersja zapoznawcza przekształcenia lokalizacji](web-config-transformations/_static/image6.png)

(W wersji zapoznawczej można zauważyć pewne dodatkowe zmiany, dla których nie zapisano transformacji: zwykle polega to na usunięciu białych znaków, które nie mają wpływu na funkcjonalność).

Podczas testowania lokacji po wdrożeniu należy również sprawdzić, czy reguła autoryzacji jest skuteczna.

> [!NOTE] 
> 
> **Uwaga dotycząca zabezpieczeń** Nigdy nie wyświetlaj informacji o błędach publicznie w aplikacji produkcyjnej lub przechowuj te informacje w lokalizacji publicznej. Osoby atakujące mogą użyć informacji o błędach w celu odnalezienia luk w zabezpieczeniach w lokacji. Jeśli używasz ELMAH w swojej aplikacji, skonfiguruj go w celu zminimalizowania zagrożeń bezpieczeństwa. Przykład ELMAH w tym samouczku nie powinien być uważany za zalecaną konfigurację. Jest to przykład, który został wybrany w celu zilustrowania, jak obsługiwać folder, w którym aplikacja musi być w stanie tworzyć pliki. Aby uzyskać więcej informacji, zobacz [Zabezpieczanie punktu końcowego programu ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).

## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Ustawienie, które będzie obsługiwane w plikach transformacji profilu publikowania

Typowym scenariuszem jest posiadanie ustawień pliku *Web. config* , które muszą się różnić w każdym środowisku, w którym jest wdrażana. Na przykład aplikacja, która wywołuje usługę WCF, może potrzebować innego punktu końcowego w środowisku testowym i produkcyjnym. Aplikacja firmy Contoso University obejmuje również ustawienie tego rodzaju. To ustawienie steruje widocznym wskaźnikiem na stronach witryny, które informuje o tym, które środowisko jest w programie, takie jak programowanie, testowanie lub produkcja. Wartość ustawienia określa, czy aplikacja będzie dołączać "(dev)" czy "(test)" do głównego nagłówka w *witrynie. główna* Strona wzorcowa:

![Wskaźnik środowiska](web-config-transformations/_static/image7.png)

Wskaźnik środowiska jest pomijany, gdy aplikacja jest uruchomiona w procesie przygotowywania lub produkcji.

Strony sieci Web firmy Contoso University odczytają wartość ustawioną w `appSettings` w pliku *Web. config* w celu określenia środowiska, w którym aplikacja jest uruchomiona:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

Wartość powinna być "test" w środowisku testowym i "prod" do celów przejściowych i produkcyjnych.

Następujący kod w pliku transformacji Zaimplementuj tę transformację:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

Wartość atrybutu `xdt:Transform` "SetAttributes" wskazuje, że celem tej transformacji jest zmiana wartości atrybutów istniejącego elementu w pliku *Web. config* . `xdt:Locator` wartość atrybutu "Match (klucz)" wskazuje, że element, który ma zostać zmodyfikowany, jest tym, którego atrybut `key` pasuje do atrybutu `key` określonego tutaj. Jedynym innym atrybutem elementu `add` jest `value`i to, co zostanie zmienione w wdrożonym pliku *Web. config* . Kod wyświetlany w tym miejscu powoduje, że atrybut `value` elementu `Environment` `appSettings` ma być ustawiony na "test" w pliku *Web. config* , który został wdrożony.

To przekształcenie należy do plików transformacji profilu publikowania, które jeszcze nie zostały utworzone. Należy utworzyć i zaktualizować pliki transformacji implementujące tę zmianę podczas tworzenia profilów publikowania dla środowisk testowych, przejściowych i produkcyjnych. Należy to zrobić w samouczkach [wdrażanie do usług IIS](deploying-to-iis.md) i [wdrażanie w środowisku produkcyjnym](deploying-to-production.md) .

> [!NOTE]
> Ponieważ to ustawienie znajduje się w `<appSettings>` elementu, istnieje inna alternatywa dla określania transformacji podczas wdrażania do Web Apps w Azure App Service zobacz [Określanie ustawień sieci Web. config na platformie Azure](#watransforms) wcześniej w tym temacie.

## <a name="setting-connection-strings"></a>Ustawianie parametrów połączenia

Mimo że domyślny plik transformacji zawiera przykład, który pokazuje, jak zaktualizować parametry połączenia, w większości przypadków nie trzeba konfigurować przekształceń parametrów połączenia, ponieważ można określić parametry połączenia w profilu publikowania. Należy to zrobić w samouczkach [wdrażanie do usług IIS](deploying-to-iis.md) i [wdrażanie w środowisku produkcyjnym](deploying-to-production.md) .

## <a name="summary"></a>Podsumowanie

Po wykonaniu tych czynności możesz wykonać przekształcenia w pliku *Web. config* przed utworzeniem profilów publikacji i zobaczyć wersję zapoznawczą tego, co będzie znajdować się w tym samym wdrożeniu.

![Wersja zapoznawcza przekształcenia lokalizacji](web-config-transformations/_static/image8.png)

W poniższym samouczku poznasz zadania konfigurowania wdrożenia wymagające ustawienia właściwości projektu.

## <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji na temat tematów objętych tym samouczkiem, zobacz [using Web. config Transformations to zmiana ustawień w docelowym pliku Web. config lub pliku App. config podczas wdrażania](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) w mapie zawartości Web Deployment dla programu Visual Studio i ASP.NET.

> [!div class="step-by-step"]
> [Poprzednie](preparing-databases.md)
> [dalej](project-properties.md)

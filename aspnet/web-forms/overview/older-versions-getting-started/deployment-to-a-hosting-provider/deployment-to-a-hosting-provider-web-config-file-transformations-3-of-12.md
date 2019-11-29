---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: przekształcenia pliku Web. config — 3 z 12 | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu stu Visual...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: fe71e6cfb0f4c5f1d99b326e9d90edb6c8c5feee
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600527"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Wdrażanie SQL Server Compact aplikacji sieci Web ASP.NET za pomocą programu Visual Studio lub Visual Web Developer: przekształcenia pliku Web. config — 3 z 12

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) projekt aplikacji sieci Web ASP.NET, który zawiera bazę danych SQL Server Compact przy użyciu programu Visual Studio 2012 RC lub Visual Studio Express 2012 RC dla sieci Web. Możesz również użyć programu Visual Studio 2010, jeśli zostanie zainstalowana aktualizacja publikacji w sieci Web. Aby zapoznać się z wprowadzeniem do serii, zobacz [pierwszy samouczek w serii](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Aby zapoznać się z samouczkiem zawierającym funkcje wdrażania wprowadzone po wydaniu wersji RC programu Visual Studio 2012, przedstawiono sposób wdrażania wersji SQL Server innych niż SQL Server Compact i przedstawiono sposób wdrażania programu w Azure App Service Web Apps, zobacz [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku pokazano, jak zautomatyzować proces zmiany pliku *Web. config* podczas wdrażania go w różnych środowiskach docelowych. Większość aplikacji ma ustawienia w pliku *Web. config* , które muszą być inne, gdy aplikacja jest wdrażana. Automatyzacja procesu wprowadzania tych zmian uniemożliwia wykonywanie ich ręcznie przy każdym wdrożeniu, co byłoby żmudnyme i podatne na błędy.

Przypomnienie: Jeśli zostanie wyświetlony komunikat o błędzie lub coś nie działa, gdy przejdziesz do samouczka, pamiętaj o sprawdzeniu [strony rozwiązywania problemów](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Przekształcenia Web. config a Web Deploy parametry

Istnieją dwa sposoby automatyzacji procesu zmiany ustawień pliku *Web. config* : [przekształcenia Web. config](https://msdn.microsoft.com/library/dd465326.aspx) i [Web Deploy parametrów](https://msdn.microsoft.com/library/ff398068.aspx). Plik transformacji *Web. config* zawiera znacznik XML, który określa sposób zmiany pliku *Web. config* podczas jego wdrażania. Można określić różne zmiany dla określonych konfiguracji kompilacji i dla określonych profilów publikacji. Domyślną konfiguracją kompilacji są debugowanie i wydanie i można utworzyć niestandardowe konfiguracje kompilacji. Profil publikacji zwykle odpowiada środowisku docelowemu. (Więcej informacji o profilach publikowania można znaleźć w samouczku [wdrażanie do usług IIS jako środowisko testowe](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) ).

Parametrów Web Deploy można użyć do określenia wielu różnych rodzajów ustawień, które muszą być skonfigurowane podczas wdrażania, w tym ustawień znalezionych w plikach *Web. config* . Gdy jest używany do określania zmian w pliku *Web. config* , Web Deploy parametry są bardziej skomplikowane do skonfigurowania, ale są przydatne, gdy nie znasz wartości do ustawienia do momentu wdrożenia. Na przykład w środowisku przedsiębiorstwa można utworzyć *pakiet wdrożeniowy* i przekazać go do osoby w dziale IT w celu zainstalowania jej w produkcji, a osoba musi mieć możliwość wprowadzania parametrów połączenia lub haseł, które nie są znane.

Aby zapoznać się z scenariuszem omawianym w tym samouczku, należy znać wszystkie elementy, które należy wykonać w pliku *Web. config* , aby nie trzeba było używać parametrów Web Deploy. Skonfigurujesz niektóre przekształcenia, które różnią się w zależności od używanej konfiguracji kompilacji, a niektóre z nich różnią się w zależności od używanego profilu publikacji.

## <a name="creating-transformation-files-for-publish-profiles"></a>Tworzenie plików transformacji dla profilów publikowania

W **Eksplorator rozwiązań**rozwiń plik *Web. config* , aby wyświetlić pliki transformacji *Web. Debug. config* i *Web. release. config* , które są tworzone domyślnie dla dwóch domyślnych konfiguracji kompilacji.

![Web. config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Możesz tworzyć pliki transformacji dla niestandardowych konfiguracji kompilacji, klikając prawym przyciskiem myszy plik Web. config i wybierając pozycję **Dodaj transformacje konfiguracji** z menu kontekstowego, ale na potrzeby tego samouczka nie trzeba tego robić.

Potrzebujesz dwóch więcej plików transformacji, aby skonfigurować zmiany, które są związane z miejscem docelowym wdrożenia, a nie z konfiguracją kompilacji. Typowym przykładem tego rodzaju ustawienia jest punkt końcowy programu WCF, który jest inny do testowania i produkcji. W kolejnych samouczkach utworzysz Profile publikowania o nazwie test i produkcja, więc potrzebujesz pliku *Web. test. config* oraz pliku *Web. Product. config* .

Pliki transformacji, które są powiązane z profilami publikowania, muszą zostać utworzone ręcznie. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt ContosoUniversity i wybierz polecenie **Otwórz folder w Eksploratorze Windows**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

W **Eksploratorze Windows**wybierz plik *Web. release. config* , skopiuj plik, a następnie wklej jego dwie kopie. Zmień nazwy tych kopii *Web. Product. config* i *Web. test. config*, a następnie zamknij **Eksploratora Windows**.

W **Eksplorator rozwiązań**kliknij pozycję **Odśwież** , aby wyświetlić nowe pliki.

Wybierz nowe pliki, kliknij prawym przyciskiem myszy, a następnie kliknij pozycję **Dołącz do projektu** w menu kontekstowym.

![Uwzględnianie plików konfiguracji testów i produkcji w projekcie](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Aby zapobiec wdrażaniu tych plików, zaznacz je w **Eksplorator rozwiązań**, a następnie w oknie **Właściwości** Zmień wartość właściwości **Akcja kompilacji** z **zawartość** na **Brak**. (Pliki transformacji, które są oparte na konfiguracjach kompilacji, są automatycznie uniemożliwiane wdrożeniem).

Teraz można przystąpić do wprowadzania transformacji *Web. config* do plików transformacji *Web. config* .

## <a name="limiting-error-log-access-to-administrators"></a>Ograniczanie dostępu do dziennika błędów do administratorów

Jeśli wystąpi błąd podczas uruchamiania aplikacji, zostanie wyświetlona strona błędu ogólnego zamiast wygenerowanej przez systemowej strony błędu, która używa [pakietu NuGet programu ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) do rejestrowania błędów i raportowania. Element `customErrors` w pliku *Web. config* określa stronę błędu:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Aby wyświetlić stronę błędu, tymczasowo Zmień atrybut `mode` elementu `customErrors` z "RemoteOnly" na "on" i uruchom aplikację z programu Visual Studio. Zażądaj błędu przez zażądanie nieprawidłowego adresu URL, takiego jak *Studentsxxx. aspx*. Zamiast strony błędu "nie znaleziono strony" wygenerowanej przez usługi IIS zostanie wyświetlona strona *GenericErrorPage. aspx* .

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Aby wyświetlić dziennik błędów, należy zastąpić wszystkie elementy w adresie URL po numerze portu za pomocą biblioteki *ELMAH. axd* (na przykład zrzut ekranu, `http://localhost:51130/elmah.axd`) i nacisnąć klawisz ENTER:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Nie zapomnij ustawić elementu `customErrors` z powrotem do trybu "RemoteOnly", gdy wszystko będzie gotowe.

Na komputerze deweloperskim wygodne jest umożliwienie bezpłatnego dostępu do strony dziennika błędów, ale w środowisku produkcyjnym, które byłyby zagrożeniem bezpieczeństwa. W przypadku lokacji produkcyjnej można dodać regułę autoryzacji, która ogranicza dostęp do dziennika błędów tylko do administratorów, konfigurując transformację w pliku *Web. Product. config* .

Otwórz *plik Web. Product. config* i Dodaj nowy element `location` bezpośrednio po tagu otwierającym `configuration`, jak pokazano poniżej. (Pamiętaj, aby dodać tylko `location` element, a nie otaczającego znacznika, który jest wyświetlany tutaj tylko w celu zapewnienia pewnego kontekstu).

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

`Transform` wartość atrybutu "INSERT" powoduje, że ten element `location` zostanie dodany jako element równorzędny do wszystkich istniejących elementów `location` w pliku *Web. config* . (Istnieje już jeden `location` element, który określa reguły autoryzacji na stronie **kredyty aktualizacji** ). Podczas testowania lokacji produkcyjnej po wdrożeniu należy przeprowadzić test, aby sprawdzić, czy ta reguła autoryzacji jest skuteczna.

Nie musisz ograniczać dostępu do dziennika błędów w środowisku testowym, więc nie musisz dodawać tego kodu do pliku *Web. test. config* .

> [!NOTE] 
> 
> **Uwaga dotycząca zabezpieczeń** Nigdy nie wyświetlaj informacji o błędach publicznie w aplikacji produkcyjnej lub przechowuj te informacje w lokalizacji publicznej. Osoby atakujące mogą użyć informacji o błędach w celu odnalezienia luk w zabezpieczeniach w lokacji. Jeśli używasz programu ELMAH we własnej aplikacji, pamiętaj, aby zbadać sposoby, w których można skonfigurować program ELMAH, aby zminimalizować ryzyko związane z zabezpieczeniami. Przykład ELMAH w tym samouczku nie powinien być uważany za zalecaną konfigurację. Jest to przykład, który został wybrany w celu zilustrowania, jak obsługiwać folder, w którym aplikacja musi być w stanie tworzyć pliki.

## <a name="setting-an-environment-indicator"></a>Ustawianie wskaźnika środowiska

Typowym scenariuszem jest posiadanie ustawień pliku *Web. config* , które muszą się różnić w każdym środowisku, w którym jest wdrażana. Na przykład aplikacja, która wywołuje usługę WCF, może potrzebować innego punktu końcowego w środowisku testowym i produkcyjnym. Aplikacja firmy Contoso University obejmuje również ustawienie tego rodzaju. To ustawienie steruje widocznym wskaźnikiem na stronach witryny, które informuje o tym, które środowisko jest w programie, takie jak programowanie, testowanie lub produkcja. Wartość ustawienia określa, czy aplikacja będzie dołączać "(dev)" czy "(test)" do głównego nagłówka w *witrynie. główna* Strona wzorcowa:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

Wskaźnik środowiska jest pomijany, gdy aplikacja działa w środowisku produkcyjnym.

Strony sieci Web firmy Contoso University odczytają wartość ustawioną w `appSettings` w pliku *Web. config* w celu określenia środowiska, w którym aplikacja jest uruchomiona:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

Wartość powinna być "test" w środowisku testowym i "prod" w środowisku produkcyjnym.

Otwórz *plik Web. Product. config* i dodaj element `appSettings` bezpośrednio przed tagiem otwierającym elementu `location`, który został dodany wcześniej:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

Wartość atrybutu `xdt:Transform` "SetAttributes" wskazuje, że celem tej transformacji jest zmiana wartości atrybutów istniejącego elementu w pliku *Web. config* . `xdt:Locator` wartość atrybutu "Match (klucz)" wskazuje, że element, który ma zostać zmodyfikowany, jest tym, którego atrybut `key` pasuje do atrybutu `key` określonego tutaj. Jedynym innym atrybutem elementu `add` jest `value`i to, co zostanie zmienione w wdrożonym pliku *Web. config* . Ten kod powoduje, że atrybut `value` elementu `Environment` `appSettings` ma być ustawiony na wartość "prod" w pliku *Web. config* , który jest wdrażany w środowisku produkcyjnym.

Następnie Zastosuj tę samą zmianę w pliku *Web. test. config* , z wyjątkiem Ustaw `value` na "test" zamiast "prod". Gdy skończysz, sekcja `appSettings` w *pliku Web. test. config* będzie wyglądać podobnie do poniższego przykładu:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Wyłączanie trybu debugowania

W przypadku kompilacji wydania nie należy włączać debugowania niezależnie od środowiska, w którym są wdrażane. Domyślnie plik transformacji *Web. release. config* jest tworzony automatycznie przy użyciu kodu, który usuwa atrybut `debug` z elementu `compilation`:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

`Transform` atrybutu powoduje, że atrybut `debug` zostanie pominięty ze wdrożonego pliku *Web. config* po każdym wdrożeniu kompilacji wydania.

Ta sama transformacja jest w plikach transformacji testowej i produkcyjnej, ponieważ zostały one utworzone przez skopiowanie pliku transformacji wydania. Nie musisz go duplikować, więc Otwórz każdy z tych plików, Usuń element **kompilacji** i Zapisz i Zamknij każdy plik.

## <a name="setting-connection-strings"></a>Ustawianie parametrów połączenia

W większości przypadków nie trzeba konfigurować przekształceń parametrów połączenia, ponieważ można określić parametry połączenia w profilu publikowania. Ale występuje wyjątek podczas wdrażania bazy danych SQL Server Compact i używasz migracje Code First platformy Entity Framework do aktualizowania bazy danych na serwerze docelowym. W tym scenariuszu należy określić dodatkowe parametry połączenia, które będą używane na serwerze do aktualizowania schematu bazy danych. Aby skonfigurować tę transformację, Dodaj element **&lt;connectionStrings&gt;** bezpośrednio po tagu otwierającym **&gt;Configuration&lt;** w plikach *Web. test. config* i *Web. Production config* :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

Atrybut `Transform` określa, że te parametry połączenia zostaną dodane do elementu *connectionStrings* w wdrożonym pliku *Web. config* . (Proces publikowania automatycznie tworzy te dodatkowe parametry połączenia, jeśli nie istnieje, ale domyślnie atrybut **ProviderName** otrzymuje wartość `System.Data.SqlClient`, co nie działa w przypadku SQL Server Compact. Poprzez ręczne dodanie parametrów połączenia, proces wdrażania nie tworzy elementu parametrów połączenia z nieprawidłową nazwą dostawcy.)

Określono wszystkie przekształcenia *Web. config* , które są potrzebne do wdrożenia aplikacji z uczelnią w firmie Contoso do testowania i produkcji. W poniższym samouczku poznasz zadania konfigurowania wdrożenia wymagające ustawienia właściwości projektu.

## <a name="more-information"></a>Więcej informacji

Aby uzyskać więcej informacji na temat tematów objętych tym samouczkiem, zobacz scenariusz transformacji Web. config w [mapie zawartości wdrożenia ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Poprzednie](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [dalej](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)

---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Informacje o pliku projektu | Microsoft Docs
author: jrjlee
description: Pliki projektu Microsoft Build Engine (MSBuild) znajdują się na serca procesu kompilacji i wdrożenia. Ten temat rozpoczyna się od omówienia koncepcji programu MSBuild...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 419fe51aaf65bddcc2c50380f099f842a8d9439c
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445697"
---
# <a name="understanding-the-project-file"></a>Zrozumienie pliku projektu

Autor [Jason Lewandowski](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Pliki projektu Microsoft Build Engine (MSBuild) znajdują się na serca procesu kompilacji i wdrożenia. Ten temat rozpoczyna się od omówienia koncepcji programu MSBuild i pliku projektu. Opisuje on najważniejsze składniki, które należy wykonać podczas pracy z plikami projektu, i działa za pomocą przykładowego sposobu użycia plików projektu do wdrożenia rzeczywistych aplikacji.
> 
> Dowiesz się:
> 
> - Jak MSBuild używa plików projektu MSBuild do kompilowania projektów.
> - Sposób integracji programu MSBuild z technologiami wdrażania, takimi jak narzędzie Web Deployment Internet Information Services (IIS) (Web Deploy).
> - Jak zrozumieć najważniejsze składniki pliku projektu.
> - Jak można używać plików projektu do kompilowania i wdrażania złożonych aplikacji.

## <a name="msbuild-and-the-project-file"></a>MSBuild i plik projektu

Podczas tworzenia i kompilowania rozwiązań w programie Visual Studio program Visual Studio używa programu MSBuild do kompilowania każdego projektu w rozwiązaniu. Każdy projekt programu Visual Studio zawiera plik projektu programu MSBuild z rozszerzeniem pliku, które odzwierciedla typ projektu&#x2014;na przykład C# projekt (. csproj), projekt Visual Basic.NET (. vbproj) lub projekt bazy danych (. DBPROJ). W celu skompilowania projektu MSBuild musi przetworzyć plik projektu skojarzony z projektem. Plik projektu to dokument XML, który zawiera wszystkie informacje i instrukcje wymagane przez MSBuild w celu skompilowania projektu, takie jak zawartość do uwzględnienia, wymagania dotyczące platformy, informacje o wersji, ustawienia serwera sieci Web lub serwera bazy danych, a także zadania, które muszą zostać wykonane.

Pliki projektów programu MSBuild są oparte na [schemacie XML programu MSBuild](/visualstudio/msbuild/msbuild-project-file-schema-reference)i w związku z tym proces kompilacji jest całkowicie otwarty i niewidoczny. Ponadto nie trzeba instalować programu Visual Studio, aby używać aparatu&#x2014;MSBuild, plik wykonywalny MSBuild. exe jest częścią .NET Framework i można uruchomić go z poziomu wiersza polecenia. Jako programista możesz utworzyć własne pliki projektu programu MSBuild przy użyciu schematu XML programu MSBuild, aby nałożyć zaawansowanej i szczegółowej kontroli nad sposobem kompilowania i wdrażania projektów. Te pliki niestandardowego projektu działają w taki sam sposób jak pliki projektu, które program Visual Studio generuje automatycznie.

> [!NOTE]
> Można również użyć plików projektu MSBuild z usługą kompilacji zespołowej w Team Foundation Server (TFS). Na przykład można użyć plików projektu w scenariuszach ciągłej integracji (CI) do automatyzowania wdrażania w środowisku testowym, gdy nowy kod jest zaewidencjonowany. Aby uzyskać więcej informacji, zobacz [konfigurowanie Team Foundation Server na potrzeby zautomatyzowanego wdrażania w sieci Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).

### <a name="project-file-naming-conventions"></a>Konwencje nazewnictwa plików projektu

W przypadku tworzenia własnych plików projektu można użyć dowolnego rozszerzenia pliku. Jednak w celu ułatwienia innym użytkownikom zrozumienia rozwiązań należy używać tych wspólnych Konwencji:

- Użyj rozszerzenia. proj podczas tworzenia pliku projektu, który kompiluje projekty.
- Użyj rozszerzenia. targets podczas tworzenia pliku projektu wielokrotnego użytku do zaimportowania do innych plików projektu. Pliki z rozszerzeniem. targets zwykle nie kompilują niczego, po prostu zawierają instrukcje, które można zaimportować do plików. proj.

### <a name="integration-with-deployment-technologies"></a>Integracja z technologiami wdrażania

Jeśli pracujesz z projektami aplikacji sieci Web w programie Visual Studio 2010, takich jak ASP.NET Web Applications i ASP.NET MVC, wiesz, że te projekty obejmują wbudowaną obsługę pakowania i wdrażania aplikacji sieci Web w środowisku docelowym. Strony **Właściwości** dla tych projektów zawierają informacje o **pakiecie/publikacji sieci Web** i **pakiecie/publikacji SQL** , których można użyć w celu skonfigurowania sposobu pakowania i wdrażania składników aplikacji. Zostanie wyświetlona karta **pakowanie/publikowanie w sieci Web** :

![](understanding-the-project-file/_static/image1.png)

Podstawowa technologia odpowiadająca tym funkcjom jest znana jako potok publikowania w sieci Web (WPP). WPP zasadniczo zapewnia MSBuild i [Web Deploy](https://go.microsoft.com/?linkid=9805122) ze sobą, aby zapewnić kompletny proces kompilowania, tworzenia pakietów i wdrażania aplikacji sieci Web.

Dobra wiadomość polega na tym, że można korzystać z punktów integracji udostępnianych przez WPP podczas tworzenia niestandardowych plików projektu dla projektów sieci Web. W pliku projektu można uwzględnić instrukcje wdrażania, które umożliwiają Kompilowanie projektów, tworzenie pakietów wdrożeniowych sieci Web i instalowanie tych pakietów na serwerach zdalnych za pomocą pojedynczego pliku projektu i pojedynczego wywołania programu MSBuild. Możesz również wywołać wszystkie inne pliki wykonywalne jako część procesu kompilacji. Na przykład można uruchomić narzędzie wiersza polecenia VSDBCMD. exe, aby wdrożyć bazę danych z pliku schematu. W ramach tego tematu zobaczysz, jak korzystać z tych funkcji w celu spełnienia wymagań scenariuszy wdrażania przedsiębiorstwa.

> [!NOTE]
> Aby uzyskać więcej informacji na temat działania procesu wdrażania aplikacji sieci Web, zobacz artykuł [Omówienie wdrażania projektu aplikacji sieci web ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx).

## <a name="the-anatomy-of-a-project-file"></a>Anatomia pliku projektu

Przed przystąpieniem do procesu kompilacji bardziej szczegółowo warto zaznajomić się z podstawową strukturą pliku projektu programu MSBuild. Ta sekcja zawiera omówienie bardziej typowych elementów, które można napotkać podczas przeglądania, edytowania lub tworzenia pliku projektu. W szczególności dowiesz się:

- Jak używać *Właściwości* do zarządzania zmiennymi dla procesu kompilacji.
- Jak używać *elementów* do identyfikowania danych wejściowych procesu kompilacji, takich jak pliki kodu.
- Jak używać *obiektów docelowych* i *zadań* do dostarczania instrukcji wykonawczych do programu MSBuild przy użyciu *Właściwości* i *elementów* zdefiniowanych w innym miejscu w pliku projektu.

Spowoduje to wyświetlenie relacji między elementami klucza w pliku projektu programu MSBuild:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>Element projektu

Element [projektu](https://msdn.microsoft.com/library/bcxfsh87.aspx) jest elementem głównym każdego pliku projektu. Oprócz identyfikacji schematu XML dla pliku projektu, element **projektu** może zawierać atrybuty, aby określić punkty wejścia dla procesu kompilacji. Przykładowo w przykładowym [rozwiązaniu Contact Manager](the-contact-manager-solution.md)plik *Publish. proj* określa, że kompilacja powinna zacząć się od wywołania obiektu docelowego o nazwie **FullPublish**.

[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]

### <a name="properties-and-conditions"></a>Właściwości i warunki

Plik projektu zwykle musi udostępniać wiele różnych informacji w celu pomyślnego skompilowania i wdrożenia projektów. Te informacje mogą obejmować nazwy serwerów, parametry połączenia, poświadczenia, konfiguracje kompilacji, źródłową i docelową ścieżkę pliku oraz inne informacje, które mają być dołączone do obsługi dostosowywania. W pliku projektu właściwości muszą być zdefiniowane w obrębie elementu [Właściwości](https://msdn.microsoft.com/library/t4w159bs.aspx) . Właściwości programu MSBuild składają się z par klucz-wartość. W obrębie elementu **Property** , nazwa elementu definiuje klucz właściwości i zawartość elementu definiuje wartość właściwości. Na przykład można zdefiniować właściwości o nazwie **servername** i **ConnectionString** do przechowywania statycznej nazwy serwera i parametrów połączenia.

[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]

Aby pobrać wartość właściwości, użyj formatu *$ (propertyName)* . Na przykład aby pobrać wartość właściwości **servername** , należy wpisać:

[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]

> [!NOTE]
> Zobacz, jak i kiedy używać wartości właściwości w dalszej części tego tematu.

Osadzanie informacji jako właściwości statyczne w pliku projektu nie zawsze jest idealnym podejściem do zarządzania procesem kompilacji. W wielu scenariuszach warto uzyskać informacje od innych źródeł lub dać użytkownikowi informacje z poziomu wiersza polecenia. Program MSBuild pozwala określić dowolną wartość właściwości jako parametr wiersza polecenia. Na przykład użytkownik może podać wartość **serwera ServerName** , gdy w wierszu polecenia jest uruchomiony program MSBuild. exe.

[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]

> [!NOTE]
> Aby uzyskać więcej informacji na temat argumentów i przełączników, których można użyć z MSBuild. exe, zobacz [Dokumentacja wiersza polecenia programu MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

Możesz użyć tej samej składni właściwości, aby uzyskać wartości zmiennych środowiskowych i wbudowanych właściwości projektu. Wiele często używanych właściwości jest zdefiniowanych dla Ciebie i można ich używać w plikach projektu przez dołączenie odpowiedniej nazwy parametru. Na przykład aby pobrać bieżącą&#x2014;platformę projektu, na przykład **x86** lub **AnyCPU**&#x2014;, można dodać odwołanie do właściwości **$ (platform)** w pliku projektu. Aby uzyskać więcej informacji, zobacz [makra dla poleceń kompilacji i właściwości](https://msdn.microsoft.com/library/c02as0cs.aspx), [wspólne właściwości projektu MSBuild](https://msdn.microsoft.com/library/bb629394.aspx)i [właściwości zastrzeżone](https://msdn.microsoft.com/library/ms164309.aspx).

Właściwości są często używane w połączeniu z *warunkami*. Większość elementów programu MSBuild obsługuje atrybut **Condition** , który umożliwia określenie kryteriów, od których MSBuild powinien oceniać element. Rozważmy na przykład tę definicję właściwości:

[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]

Gdy program MSBuild przetwarza tę definicję właściwości, najpierw sprawdza, czy jest dostępna wartość właściwości **$ (OutputRoot)** . Jeśli wartość właściwości jest pusta&#x2014;inaczej mówiąc, użytkownik nie dostarczył wartości dla tej właściwości&#x2014;, warunek ma wartość **true** , a wartość właściwości jest ustawiona na **.. \Publish\Out**. Jeśli użytkownik podał wartość dla tej właściwości, warunek zwróci wartość **false** , a wartość właściwości statycznej nie jest używana.

Aby uzyskać więcej informacji na temat różnych sposobów określania warunków, zobacz [warunki MSBuild](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Elementy i grupy elementów

Jedną z ważnych ról pliku projektu jest Definiowanie danych wejściowych procesu kompilacji. Zazwyczaj te dane wejściowe to pliki&#x2014;kodu plików, pliki konfiguracji, pliki poleceń i inne pliki, które są potrzebne do przetworzenia lub kopiowania w ramach procesu kompilacji. W schemacie projektu MSBuild te dane wejściowe są reprezentowane przez elementy [elementu](https://msdn.microsoft.com/library/ms164283.aspx) . W pliku projektu elementy muszą być zdefiniowane w obrębie elementu [Item](https://msdn.microsoft.com/library/646dk05y.aspx) . Podobnie jak w przypadku elementów **Właściwości** , można nazwać element **elementu** . Jednak należy określić atrybut **include** , aby zidentyfikować plik lub symbol wieloznaczny, który reprezentuje element.

[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]

Określając **wiele elementów elementów o** tej samej nazwie, efektywnie tworzysz nazwaną listę zasobów. Dobrym sposobem wyświetlenia tego działania jest zaprojektowanie w jednym z plików projektu, które tworzy program Visual Studio. Na przykład plik *ContactManager. MVC. csproj* w przykładowym rozwiązaniu zawiera wiele grup elementów, z których każdy ma różne elementy o identycznej **nazwie.**

[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]

W ten sposób plik projektu nakazuje programowi MSBuild konstruowanie list plików, które muszą być przetwarzane w taki sam sposób, jak&#x2014;lista **odwołania** zawiera zestawy, które muszą być stosowane dla pomyślnej kompilacji, a lista **kompilacji** zawiera pliki kodu, które muszą zostać skompilowane, a lista **zawartości** zawiera zasoby, które muszą zostać skopiowane bez zmian. Dowiesz się, jak proces kompilacji odwołuje się do i używa tych elementów w dalszej części tego tematu.

Elementy elementu mogą również zawierać [ItemMetadata —](https://msdn.microsoft.com/library/ms164284.aspx) elementy podrzędne. Są to pary klucz-wartość zdefiniowane przez użytkownika i zasadniczo reprezentujące właściwości, które są specyficzne dla danego elementu. Na przykład, wiele elementów elementu **kompilowania** w pliku projektu zawiera **DependentUpon** elementy podrzędne.

[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]

> [!NOTE]
> Oprócz metadanych elementu utworzonego przez użytkownika, wszystkie elementy są przypisywane do różnych typowych metadanych podczas tworzenia. Aby uzyskać więcej informacji, zobacz [dobrze znane metadane elementu](https://msdn.microsoft.com/library/ms164313.aspx).

Można utworzyć elementy **elementu Item** w obrębie elementu **projektu** poziomu głównego lub w obrębie określonych elementów **docelowych** . Elementy **elementu Items** obsługują również atrybuty **warunków** , które umożliwiają dostosowanie danych wejściowych do procesu kompilacji zgodnie z warunkami, takimi jak konfiguracja projektu lub platforma.

### <a name="targets-and-tasks"></a>Cele i zadania

W schemacie programu MSBuild element [Task](https://msdn.microsoft.com/library/77f2hx1s.aspx) reprezentuje konkretną instrukcję kompilacji (lub zadanie). Program MSBuild zawiera wiele wstępnie zdefiniowanych zadań. Na przykład:

- Zadanie **copy** kopiuje pliki do nowej lokalizacji.
- Zadanie **CSC** wywołuje kompilator wizualny C# .
- Zadanie **VBC** wywołuje kompilator Visual Basic.
- Zadanie **exec** uruchamia określony program.
- Zadanie **komunikatu** zapisuje komunikat do rejestratora.

> [!NOTE]
> Aby uzyskać szczegółowe informacje o zadaniach, które są dostępne, zobacz odwołanie do [zadania programu MSBuild](https://msdn.microsoft.com/library/7z253716.aspx). Aby uzyskać więcej informacji o zadaniach, w tym o sposobach tworzenia własnych zadań niestandardowych, zobacz [zadania programu MSBuild](https://msdn.microsoft.com/library/ms171466.aspx).

Zadania muszą być zawsze zawarte w elementach [docelowych](https://msdn.microsoft.com/library/t50z2hka.aspx) . Element **docelowy** jest zestawem co najmniej jednego zadania, które są wykonywane sekwencyjnie, a plik projektu może zawierać wiele obiektów docelowych. Gdy chcesz uruchomić zadanie lub zestaw zadań, wywołasz element docelowy, który je zawiera. Załóżmy na przykład, że masz prosty plik projektu, który rejestruje komunikat.

[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]

Obiekt docelowy można wywołać z wiersza polecenia przy użyciu **/t** Switch, aby określić element docelowy.

[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]

Alternatywnie, można dodać atrybut **DefaultTargets —** do elementu **projektu** , aby określić elementy docelowe, które chcesz wywołać.

[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]

W takim przypadku nie trzeba określać elementu docelowego z wiersza polecenia. Można po prostu określić plik projektu, a MSBuild wywoła element docelowy **FullPublish** .

[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]

Oba elementy docelowe i zadania mogą zawierać atrybuty **warunku** . W związku z tym możesz pominąć wszystkie cele lub poszczególne zadania, jeśli zostaną spełnione określone warunki.

Ogólnie mówiąc, podczas tworzenia przydatnych zadań i obiektów docelowych należy odwołać się do właściwości i elementów, które zostały zdefiniowane w innym miejscu w pliku projektu:

- Aby użyć wartości właściwości, wpisz **$ (***PropertyName***)** , gdzie *PropertyName* jest nazwą elementu **Właściwości** lub nazwą parametru.
- Aby użyć elementu, wpisz **@ (***itemName***)** , gdzie *itemName* jest nazwą elementu **Item** .

> [!NOTE]
> Należy pamiętać, że jeśli tworzysz wiele elementów o tej samej nazwie, tworzysz listę. W przeciwieństwie do tworzenia wielu właściwości o tej samej nazwie, Ostatnia wartość właściwości zostanie zastąpiona wszystkimi poprzednimi właściwościami o tej samej nazwie&#x2014;, a właściwość może zawierać tylko jedną wartość.

Na przykład w pliku *Publish. proj* w przykładowym rozwiązaniu zapoznaj się z elementem docelowym **BuildProjects** .

[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]

W tym przykładzie można obserwować następujące kluczowe punkty:

- Jeśli parametr **BuildingInTeamBuild** jest określony i ma wartość **true**, żadne z zadań w tym obiekcie docelowym nie zostanie wykonane.
- Element docelowy zawiera pojedyncze wystąpienie zadania programu [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) . To zadanie pozwala tworzyć inne projekty MSBuild.
- Element **ProjectsToBuild** jest przesyłany do zadania. Ten element może reprezentować listę plików projektu lub rozwiązania, które zostały zdefiniowane przez elementy elementu **ProjectsToBuild** w grupie elementów. W tym przypadku element **ProjectsToBuild** odwołuje się do pojedynczego pliku rozwiązania.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Wartości właściwości przesłane do zadania programu **MSBuild** obejmują parametry o nazwach **OutputRoot** i **Configuration**. Te ustawienia są ustawiane na wartości parametrów, jeśli są one dostarczone lub statyczne wartości właściwości, jeśli nie są.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

Można też zobaczyć, że zadanie **MSBuild** wywołuje obiekt docelowy o nazwie **kompilacja**. Jest to jeden z kilku wbudowanych obiektów docelowych, które są szeroko używane w plikach projektu programu Visual Studio i są dostępne dla użytkownika w niestandardowych plikach projektu, takich jak **Kompilowanie**, **czyszczenie**, **Odbudowywanie**i **Publikowanie**. Dowiesz się więcej o używaniu elementów docelowych i zadań do kontrolowania procesu kompilacji oraz o zadaniu programu **MSBuild** w dalszej części tego tematu.

> [!NOTE]
> Aby uzyskać więcej informacji na temat obiektów docelowych, zobacz [elementy docelowe programu MSBuild](https://msdn.microsoft.com/library/ms171462.aspx).

## <a name="splitting-project-files-to-support-multiple-environments"></a>Dzielenie plików projektu do obsługi wielu środowisk

Załóżmy, że chcesz mieć możliwość wdrożenia rozwiązania w wielu środowiskach, takich jak serwery testowe, platformy przejściowe i środowiska produkcyjne. Konfiguracja może znacząco różnić się między tymi środowiskami&#x2014;, nie tylko pod względem nazw serwerów, parametrów połączenia i tak dalej, ale także w przypadku poświadczeń, ustawień zabezpieczeń i wielu innych czynników. W przypadku konieczności regularnego wykonywania tych czynności nie jest celowe Edytowanie wielu właściwości w pliku projektu przy każdym przełączeniu środowiska docelowego. Nie jest to idealne rozwiązanie wymagane do uzyskania nieskończonej listy wartości właściwości, które mają być dostarczone do procesu kompilacji.

Na szczęście istnieje alternatywa. Program MSBuild pozwala podzielić konfigurację kompilacji na wiele plików projektu. Aby zobaczyć, jak to działa, w przykładowym rozwiązaniu należy zauważyć, że istnieją dwa pliki niestandardowego projektu:

- *Publish. proj*, który zawiera właściwości, elementy i obiekty docelowe, które są wspólne dla wszystkich środowisk.
- *ENV-dev. proj*, który zawiera właściwości specyficzne dla środowiska deweloperskiego.

Teraz należy zauważyć, że plik *Publish. proj* zawiera element [Import](https://msdn.microsoft.com/library/92x05xfs.aspx) bezpośrednio poniżej tagu otwierającego **projektu** .

[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]

Element **Import** służy do importowania zawartości innego pliku projektu MSBuild do bieżącego pliku projektu MSBuild. W tym przypadku parametr **TargetEnvPropsFile** zawiera nazwę pliku projektu, który chcesz zaimportować. Możesz podać wartość dla tego parametru podczas uruchamiania programu MSBuild.

[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]

To efektywnie Scala zawartość dwóch plików w jeden plik projektu. Korzystając z tej metody, można utworzyć jeden plik projektu zawierający konfigurację uniwersalną kompilacji i wiele dodatkowych plików projektu zawierających właściwości specyficzne dla środowiska. W związku z tym po prostu uruchomienie polecenia z inną wartością parametru pozwala wdrożyć rozwiązanie w innym środowisku.

![](understanding-the-project-file/_static/image3.png)

Podzielenie plików projektu w ten sposób jest dobrym podejściem do przestrzegania. Umożliwia deweloperom wdrażanie w wielu środowiskach, uruchamiając pojedyncze polecenie, unikając duplikowania właściwości uniwersalnej kompilacji w wielu plikach projektu.

> [!NOTE]
> Aby uzyskać wskazówki dotyczące dostosowywania plików projektu specyficznych dla środowiska dla własnych środowisk serwerów, zobacz [Konfigurowanie właściwości wdrożenia dla środowiska docelowego](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="conclusion"></a>Wniosek

Ten temat zawiera ogólne wprowadzenie do plików projektów programu MSBuild i wyjaśniono, jak można utworzyć własne niestandardowe pliki projektu w celu kontrolowania procesu kompilacji. Wprowadzono również koncepcję dzielenia plików projektu na instrukcje uniwersalnej kompilacji i właściwości kompilacji specyficzne dla środowiska, aby ułatwić tworzenie i wdrażanie projektów w wielu miejscach docelowych.

W następnym temacie, [opisującym proces kompilacji](understanding-the-build-process.md), przedstawiono dokładniejsze informacje na temat sposobu użycia plików projektu do kontrolowania kompilowania i wdrażania, przechodzenia przez proces wdrażania rozwiązania o realistycznym poziomie złożoności.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać bardziej szczegółowe wprowadzenie do plików projektu i WPP, zobacz [wewnątrz Microsoft Build Engine: korzystanie z programu MSBuild i Team Foundation Build](http://amzn.com/0735645248) przez Sayed Ibrahim Hashimi i William BARTHOLOMEW, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Poprzedni](setting-up-the-contact-manager-solution.md)
> [Następny](understanding-the-build-process.md)

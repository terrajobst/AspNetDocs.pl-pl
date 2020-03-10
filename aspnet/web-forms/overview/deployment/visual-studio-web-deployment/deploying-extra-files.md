---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie dodatkowych plików | Microsoft Docs'
author: tdykstra
description: W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przez usin...
ms.author: riande
ms.date: 03/23/2015
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: eaa3141c22980f0c816e2f33b5597ac9fe69c23c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548413"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>ASP.NET wdrażanie w sieci Web przy użyciu programu Visual Studio: wdrażanie dodatkowych plików

Autor [Dykstra](https://github.com/tdykstra)

[Pobierz projekt początkowy](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> W tej serii samouczków pokazano, jak wdrożyć (opublikować) aplikację sieci Web ASP.NET w celu Azure App Service Web Apps lub do dostawcy hostingu innej firmy przy użyciu programu Visual Studio 2012 lub Visual Studio 2010. Aby uzyskać informacje o serii, zobacz [pierwszy samouczek w serii](introduction.md).

## <a name="overview"></a>Omówienie

W tym samouczku pokazano, jak wdrożyć potok publikacji w sieci Web w programie Visual Studio, aby wykonać dodatkowe zadanie podczas wdrażania. Zadaniem jest skopiowanie dodatkowych plików, które nie znajdują się w folderze projektu do docelowej witryny sieci Web.

W tym samouczku skopiujesz jeden dodatkowy plik: *robots. txt*. Chcesz wdrożyć ten plik do przemieszczania, ale nie do środowiska produkcyjnego. W samouczku [wdrażanie do produkcji](deploying-to-production.md) dodano ten plik do projektu i skonfigurowano profil publikowania produkcyjnego w celu wykluczenia go. W tym samouczku zobaczysz alternatywną metodę obsługi tej sytuacji, która będzie przydatna dla wszystkich plików, które chcesz wdrożyć, ale nie chcesz ich uwzględnić w projekcie.

## <a name="move-the-robotstxt-file"></a>Przenoszenie pliku robots. txt

Aby przygotować się do innej metody obsługi pliku *robots. txt*, w tej części samouczka przeniesiesz plik do folderu, który nie znajduje się w projekcie, i usuniesz pliki *robots. txt* ze środowiska przejściowego. Konieczne jest usunięcie pliku z przemieszczania, aby można było sprawdzić, czy nowa metoda wdrażania pliku w tym środowisku działa poprawnie.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy plik *robots. txt* , a następnie kliknij pozycję **Wyklucz z projektu**.
2. Korzystając z Eksploratora plików systemu Windows, Utwórz nowy folder w folderze rozwiązania i nadaj mu nazwę *ExtraFiles*.
3. Przenieś plik *robots. txt* z folderu projektu *ContosoUniversity* do folderu *ExtraFiles* .

    ![Folder ExtraFiles](deploying-extra-files/_static/image1.png)
4. Za pomocą narzędzia FTP Usuń plik *robots. txt* z tymczasowej witryny sieci Web.

    Alternatywnie można wybrać opcję **Usuń dodatkowe pliki w miejscu docelowym** w obszarze **Opcje publikowania plików** na karcie **Ustawienia** w profilu publikowania przemieszczania i ponownie opublikować ją w celu przygotowania.

## <a name="update-the-publish-profile-file"></a>Aktualizowanie pliku profilu publikowania

*Plik robots. txt* jest wymagany tylko w przypadku przemieszczania, więc należy zaktualizować tylko profil publikowania, aby wdrożyć go w ramach wdrożenia.

1. W programie Visual Studio Otwórz pozycję *tymczasowy. pubxml*.
2. Na końcu pliku przed tagiem zamykającym `</Project>` Dodaj następujący znacznik:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Ten kod tworzy nowy *obiekt docelowy* , który będzie zbierać dodatkowe pliki do wdrożenia. Element docelowy składa się z co najmniej jednego zadania, które będzie wykonywane przez program MSBuild na podstawie określonych kryteriów.

    Atrybut `Include` określa, że folder, w którym mają znajdować się pliki, jest *ExtraFiles*, znajduje się na tym samym poziomie co folder projektu. Program MSBuild zbierze wszystkie pliki z tego folderu i rekursywnie z dowolnego podfolderu (podwójna gwiazdka określa podfoldery cykliczne). Za pomocą tego kodu można umieścić wiele plików i plików w podfolderach w folderze *ExtraFiles* , a wszystkie zostaną wdrożone.

    `DestinationRelativePath` element określa, że foldery i pliki powinny być kopiowane do folderu głównego docelowej witryny sieci Web, w tej samej strukturze plików i folderów, co znajdują się w folderze *ExtraFiles* . Jeśli chcesz skopiować folder *ExtraFiles* , wartość `DestinationRelativePath` będzie *ExtraFiles\%(RecursiveDir)% (filename)% (rozszerzenie)* .
3. Na końcu pliku przed tagiem zamykającym `</Project>` Dodaj następujący znacznik, który określa, kiedy należy wykonać nowy element docelowy.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Ten kod powoduje, że nowy element docelowy `CustomCollectFiles` być wykonywany przy każdym wykonaniu elementu docelowego, który kopiuje pliki do folderu docelowego. Istnieje osobny cel dla tworzenia pakietu do opublikowania i wdrożenia, a nowy element docelowy jest wstrzykiwany w obu obiektach docelowych w przypadku, gdy użytkownik zdecyduje się na wdrożenie przy użyciu pakietu wdrożeniowego zamiast publikowania.

    Plik *. pubxml* wygląda teraz podobnie do poniższego przykładu:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Zapisz i zamknij plik *przemieszczania pubxml* .

## <a name="publish-to-staging"></a>Publikowanie do przemieszczania

Za pomocą jednego kliknięcia przycisku Publikuj lub w wierszu polecenia Opublikuj aplikację przy użyciu profilu przemieszczania.

Jeśli używasz publikowania jednym kliknięciem, możesz sprawdzić, czy w oknie **podglądu** zostanie skopiowany *plik robots. txt* . W przeciwnym razie użyj narzędzia FTP, aby sprawdzić, czy plik *robots. txt* znajduje się w folderze głównym witryny sieci Web po wdrożeniu.

## <a name="summary"></a>Podsumowanie

Ta seria samouczków dotyczy wdrażania aplikacji sieci Web ASP.NET w ramach dostawcy hostingu innej firmy. Aby uzyskać więcej informacji na temat tematów omówionych w tych samouczkach, zobacz [mapa zawartości wdrożenia ASP.NET](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Więcej informacji

Jeśli wiesz, jak korzystać z plików MSBuild, Możesz zautomatyzować wiele innych zadań wdrażania, pisząc kod w plikach *. pubxml* (dla zadań specyficznych dla profilu) lub pliku Project *. WPP. targets* (dla zadań, które mają zastosowanie do wszystkich profilów). Aby uzyskać więcej informacji na temat plików *. pubxml* i *. WPP. targets* , zobacz [How to: Edit Settings Deployment (pliki. pubxml) i plik. WPP. targets w projektach internetowych programu Visual Studio](https://msdn.microsoft.com/library/ff398069). Aby zapoznać się z podstawowymi informacjami o kodzie programu MSBuild, zobacz **anatomię pliku projektu** w [serii wdrożenia w przedsiębiorstwie: Omówienie pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Aby dowiedzieć się, jak korzystać z plików MSBuild do wykonywania zadań we własnych scenariuszach, zobacz tę książkę: [wewnątrz Microsoft Build Engine: korzystanie z programu MSBuild i Team Foundation Build](http://msbuildbook.com) przez Sayed Ibraham Hashimi i William Bartholomew.

## <a name="acknowledgements"></a>Podziękowania

Chcę podziękowanie następujące osoby, które złożyły znaczące wkłady w zawartość tej serii samouczków:

- [Alberto Poblacion, MVP &amp; MCT, Hiszpania](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, SPECJALISTa ds. projektowania platformy danych, Stany Zjednoczone
- Harsh Mittal, Microsoft
- [Jan Galloway](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Jan Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Włochy](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))
- [Scott myśliwy, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Poprzednie](command-line-deployment.md)
> [dalej](troubleshooting.md)

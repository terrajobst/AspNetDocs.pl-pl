---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publikowanie aplikacji na platformie Azure Azure App Service | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: a9a7b74a07c71b47253c0af304c7a26ffa4eaae5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622389"
---
# <a name="publish-the-app-to-azure-azure-app-service"></a>Publikowanie aplikacji w usłudze Azure Azure App Service

według [Jan Wasson](https://github.com/MikeWasson)

[Pobierz ukończony projekt](https://github.com/MikeWasson/BookService)

Ostatnim krokiem jest opublikowanie aplikacji na platformie Azure. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Publikuj**.

![](part-10/_static/image1.png)

Kliknięcie przycisku **Publikuj** wywołuje okno dialogowe **Publikowanie w sieci Web** . Jeśli zaznaczono opcję **host w chmurze** podczas pierwszego utworzenia projektu, połączenie i ustawienia są już skonfigurowane. W takim przypadku po prostu kliknij kartę **Ustawienia** i sprawdź, &quot;wykonaj migracje Code First&quot;. (Jeśli na początku nie sprawdzono **hosta w chmurze** , wykonaj kroki opisane w [następnej sekcji](#new-website)).

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Aby wdrożyć aplikację, kliknij pozycję **Opublikuj**. Postęp publikowania można wyświetlić w oknie **działanie publikowania w sieci Web** . (Z menu **Widok** wybierz pozycję **inne okna**, a następnie wybierz pozycję **aktywność publikacji w sieci Web**).

![](part-10/_static/image4.png)

Po zakończeniu wdrażania aplikacji w programie Visual Studio zostanie automatycznie uruchomiona przeglądarka domyślna, która przejdzie pod adres URL wdrożonej witryny sieci Web. Utworzona aplikacja jest teraz uruchomione w chmurze. Adres URL w pasku adresu przeglądarki pokazuje, że witryna jest ładowana z Internetu.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Wdrażanie w nowej witrynie sieci Web

Jeśli nie sprawdzono **hosta w chmurze** podczas pierwszego tworzenia projektu, można teraz skonfigurować nową aplikację sieci Web. W Eksplorator rozwiązań kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Publikuj**. Wybierz kartę **profil** , a następnie kliknij przycisk **Microsoft Azure Websites**. Jeśli nie jesteś zalogowany do Azure, wyświetli się monit o zalogowanie.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

W oknie dialogowym **istniejące witryny sieci Web** kliknij pozycję **Nowy**.

![](part-10/_static/image9.png)

Wprowadź nazwę lokacji. Wybierz swoją subskrypcję platformy Azure i region. W obszarze **serwer bazy danych**wybierz pozycję **Utwórz nowy serwer**lub wybierz istniejący serwer. Kliknij przycisk **Utwórz**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Kliknij kartę **Ustawienia** i sprawdź, &quot;wykonaj migracje Code First&quot;. Następnie kliknij przycisk **Publikuj**.

> [!div class="step-by-step"]
> [Wstecz](part-9.md)

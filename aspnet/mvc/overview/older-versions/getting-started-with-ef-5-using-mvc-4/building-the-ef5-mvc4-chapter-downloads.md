---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Tworzenie w rozdziale pliki do pobrania dla platformy MVC EF 5 4 samouczki | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 6b5d10ba9e878908953e999bd1fd44970acf4ca5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57078338"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Tworzenie w rozdziale pliki do pobrania dla platformy MVC EF 5 4 samouczki
====================
Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))

[Pobierz ukończony projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładową aplikację sieci web firmy Contoso University przedstawia sposób tworzenia aplikacji ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacji na temat tej serii samouczka, zobacz [pierwszym samouczku tej serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


## <a name="building-the-chapter-downloads"></a>Tworzenie materiałów do pobrania rozdziału

1. Pobierz i rozpakuj plik zip przykładowy projekt. W pakiecie do pobrania rozpakowany znajdziesz zip dodatkowe pliki — po jednym na zakończenie każdego działu.
2. Kliknij prawym przyciskiem myszy plik zip odpowiednią pozycję **właściwości**i kliknij przycisk **odblokowanie** przycisku.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Rozpakuj plik.
4. Kliknij dwukrotnie *CUx.sln* plik, aby uruchomić program Visual Studio.
5. Z **narzędzia** menu, kliknij przycisk **Menedżera pakietów NuGet**, następnie **Konsola Menedżera pakietów**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. Kliknij w konsoli Menedżera pakietów (PMC) **przywrócić**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Zamknij program Visual Studio.
8. Uruchom ponownie program Visual Studio, otwierając plik rozwiązania zamknięte w poprzednim kroku.
9. W konsoli Menedżera pakietów (PMC) wprowadź `Update-Database` polecenia:  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Jeśli zostanie wyświetlony następujący błąd:  
    >   
    >  *Termin "Update-Database" nie został rozpoznany jako nazwa polecenia cmdlet, funkcji, pliku skryptu lub program wykonywalny. Sprawdź pisownię nazwy lub jeśli ścieżka został uwzględniony, sprawdź, czy ścieżka jest poprawna i spróbuj ponownie.*  
    > Zamknij i ponownie uruchom program Visual Studio.

    Każdą migrację zostanie uruchomiony, a następnie uruchomi seed — metoda Teraz możesz uruchomić aplikację.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [Poprzednie](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)

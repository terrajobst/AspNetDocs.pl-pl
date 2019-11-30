---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Kompilowanie plików rozdziałów dla samouczków Dr 5 MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Code First Entity Framework 5 i programu Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 0e720b3e4c5d3b8f779afe3a6e2b47baa86eec4c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2019
ms.locfileid: "74592704"
---
# <a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Kompilowanie pobierania rozdziałów dla samouczków Dr 5 MVC 4

Autor [Rick Anderson]((https://twitter.com/RickAndMSFT))

[Pobierz ukończony projekt](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Przykładowa aplikacja internetowa Contoso University pokazuje, jak tworzyć aplikacje ASP.NET MVC 4 przy użyciu Entity Framework 5 Code First i programu Visual Studio 2012. Aby uzyskać informacje na temat serii samouczków, zobacz [pierwszy samouczek w serii](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

## <a name="building-the-chapter-downloads"></a>Tworzenie materiałów do pobrania rozdziału

1. Pobierz i rozpakuj przykładowy plik zip projektu. W niespakowanym pakiecie pobierania znajdziesz dodatkowe pliki zip, po jednym dla każdej z nich.
2. Kliknij prawym przyciskiem myszy żądany plik zip, kliknij polecenie **Właściwości**, a następnie kliknij przycisk **Odblokuj** .  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Rozpakuj plik.
4. Kliknij dwukrotnie plik *CUx. sln* , aby uruchomić program Visual Studio.
5. W menu **Narzędzia** kliknij kolejno pozycje **Menedżer pakietów NuGet**, **konsola Menedżera pakietów**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. W konsoli Menedżera pakietów (PMC) kliknij przycisk **Przywróć**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Zamknij program Visual Studio.
8. Uruchom ponownie program Visual Studio, otwierając plik rozwiązania, który został zamknięty w powyższym kroku.
9. W konsoli Menedżera pakietów (PMC) wprowadź `Update-Database` polecenie:  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Jeśli zostanie wyświetlony następujący błąd:  
    >   
    >  *Termin "Update-Database" nie jest rozpoznawany jako nazwa polecenia cmdlet, funkcji, pliku skryptu ani programu interoperacyjnego. Sprawdź pisownię nazwy lub, jeśli ścieżka została uwzględniona, sprawdź, czy ścieżka jest poprawna, i spróbuj ponownie.*  
    > Zamknij i uruchom ponownie program Visual Studio.

    Każda migracja zostanie uruchomiona, a następnie zostanie uruchomiona Metoda inicjatora. Teraz możesz uruchomić aplikację.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [Ubiegł](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)

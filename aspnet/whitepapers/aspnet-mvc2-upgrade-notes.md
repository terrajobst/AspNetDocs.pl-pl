---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Uaktualnianie aplikacji ASP.NET MVC 1,0 do ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: W tym dokumencie opisano sposób ręcznego uaktualniania i kreatora ASP.NET MVC 1,0 do ASP.NET MVC 2. Ten dokument jest również dostępny dla d...
ms.author: riande
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 27589f1b1c9d5038118e5ff0cc2e7cecae17d5ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637019"
---
# <a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Uaktualnianie aplikacji ASP.NET MVC 1.0 do wzorca ASP.NET MVC 2

> W tym dokumencie opisano sposób ręcznego uaktualniania i kreatora ASP.NET MVC 1,0 do ASP.NET MVC 2. Ten dokument jest również dostępny do [pobrania](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)

## <a name="introduction"></a>Wprowadzenie

ASP.NET MVC 2 można zainstalować równolegle z ASP.NET MVC 1,0 na tym samym serwerze. Dzięki temu deweloperzy aplikacji mogą elastycznie wybierać, kiedy uaktualnić aplikację ASP.NET MVC 1,0 do ASP.NET MVC 2.

Program Visual Studio 2010 zawiera kreatora, który uaktualnia istniejące projekty ASP.NET MVC 1,0 skompilowane z programem Visual Studio 2008 do ASP.NET MVC 2. Kreator uaktualniania jest inicjowany przez otwarcie projektu ASP.NET MVC 1,0 w programie Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Kreator uaktualniania programu ASP.NET MVC 1,0 w programie Visual Studio 2008 z dodatkiem SP1

Aby uaktualnić aplikację ASP.NET MVC 1,0 do ASP.NET MVC 2 w programie Visual Studio 2008 z dodatkiem SP1, użyj aplikacji MvcAppConverter (unsupportd). Możesz pobrać tę aplikację z następującego adresu URL:

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Ręczne uaktualnianie projektu ASP.NET MVC 1,0

Aby ręcznie uaktualnić istniejącą aplikację ASP.NET MVC 1,0 do wersji 2, wykonaj następujące kroki:

1. Utwórz kopię zapasową istniejącego projektu.
2. W edytorze tekstów Otwórz plik projektu (plik z rozszerzeniem. csproj lub. vbproj) i Znajdź element ProjectTypeGuid. Jako wartość tego elementu Zastąp identyfikator GUID {603c0e0b-db56-11dc-be95-000d561079b0} atrybutem {F85E285D-A4E0-4152-9332-AB1D724D3325}. Gdy wszystko będzie gotowe, wartość tego elementu powinna być następująca: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. W folderze głównym aplikacji sieci Web Edytuj plik Web. config. Wyszukaj ciąg system. Web. MVC, Version = 1.0.0.0 i Zamień wszystkie wystąpienia na wartość System. Web. MVC, Version = 2.0.0.0.
4. Powtórz poprzedni krok dla pliku Web. config znajdującego się w folderze widoki.
5. Otwórz projekt przy użyciu programu Visual Studio, a w **Eksplorator rozwiązań**rozwiń węzeł **odwołania** . Usuń odwołanie do System. Web. MVC (które wskazuje zestaw w wersji 1,0). Dodaj odwołanie do elementu System. Web. MVC (v 2.0.0.0).
6. Dodaj następujący element bindingRedirect do pliku Web. config w katalogu głównym aplikacji w sekcji kopii:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Utwórz nową pustą aplikację ASP.NET MVC 2. Skopiuj pliki z folderu skrypty nowej aplikacji do folderu skrypty istniejącej aplikacji.
8. Zaktualizuj istniejący plik CSS internetowego €™ s przy użyciu definicji stylów CSS w pliku site. css.
9. Skompiluj aplikację i uruchom ją. Jeśli wystąpią błędy, zapoznaj się z sekcją istotne zmiany na stronie [co nowego w ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) .

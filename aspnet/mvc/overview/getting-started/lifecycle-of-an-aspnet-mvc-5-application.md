---
uid: mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
title: Cykl życia aplikacji ASP.NET MVC 5 | Microsoft Docs
author: cephalin
description: Pobierz dokument PDF zawierający wykres cyklu życia aplikacji ASP.NET MVC 5. Ten dokument cyklu życia zawiera ogólny widok cyklu życia składnika MVC a...
ms.author: riande
ms.date: 02/28/2014
ms.assetid: 9c1e3a75-b644-4480-8326-11300b1ec4b3
msc.legacyurl: /mvc/overview/getting-started/lifecycle-of-an-aspnet-mvc-5-application
msc.type: authoredcontent
ms.openlocfilehash: f4a9b3fb61552b070db11fba617b5627fcd71cd5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582202"
---
# <a name="lifecycle-of-an-aspnet-mvc-5-application"></a>Cykl życia aplikacji ASP.NET MVC 5

za pomocą [połączonego Cephas](https://github.com/cephalin)

[Pobierz dokument PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf)

W tym miejscu można pobrać dokument PDF, który jest wykresem cyklu życia każdej aplikacji ASP.NET MVC 5, od odebrania żądania HTTP w celu wysłania odpowiedzi HTTP z powrotem do klienta. Jest ona zaprojektowana jako narzędzie edukacyjne dla osób, które są nowe do ASP.NET MVC, a także jako odniesienia do określonych aspektów aplikacji. Dokument PDF ma następujące funkcje:

- Odpowiednie etapy [elemencie HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication.aspx) , które ułatwiają zrozumienie, gdzie MVC integruje się z [cyklem życia aplikacji ASP.NET](https://msdn.microsoft.com/library/bb470252.aspx).
- Ogólny widok cyklu życia aplikacji MVC, w którym można zrozumieć główne etapy, w których każda aplikacja MVC przechodzi przez proces przetwarzania żądania.  
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image1.jpg)
- Widok szczegółów, który pokazuje szczegółowe informacje o potoku przetwarzania żądań. Można porównać widok wysokiego poziomu i widok szczegółów, aby zobaczyć, jak szczegóły cykl życia są gromadzone w różnych etapach. [Pobierz plik PDF](lifecycle-of-an-aspnet-mvc-5-application/_static/lifecycle-of-an-aspnet-mvc-5-application1.pdf) , aby wyświetlić większy widok.
    ![](lifecycle-of-an-aspnet-mvc-5-application/_static/image2.jpg)
- Umieszczanie i przeznaczenie wszystkich metod w obiekcie [kontrolera](https://msdn.microsoft.com/library/system.web.mvc.controller.aspx) w potoku przetwarzania żądań. Może się okazać, że nie trzeba przesłonić żadnej metody, ale ważne jest, aby zrozumieć swoją rolę w cyklu życia aplikacji, dzięki czemu można napisać kod na odpowiednim etapie cyklu życia dla danego efektu.
- Diagramy rozwiniętą pokazujące, w jaki sposób są wywoływane poszczególne typy filtrów (uwierzytelnianie, autoryzacja, Akcja i wynik).
- Łącze do przydatnego artykułu lub blogu z każdego punktu zainteresowania w widoku szczegółowym.

## <a name="next-steps"></a>Następne kroki

Czy ten dokument spełnia Twoje potrzeby? Doceniamy Twoją opinię. Jeśli masz jakieś pytania dotyczące cyklu życia ASP.NET MVC w aplikacji, [StackOverflow](http://stackoverflow.com/help) i [fora MVC ASP.NET](https://forums.asp.net/1146.aspx) są doskonałymi miejscami do zadawania. Obserwuj [mnie](https://twitter.com/Cephas_MSFT) w serwisie Twitter, aby otrzymywać aktualizacje dotyczące moich najnowszych samouczków.

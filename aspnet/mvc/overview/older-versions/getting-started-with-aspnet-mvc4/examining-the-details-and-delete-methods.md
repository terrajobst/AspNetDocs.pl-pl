---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
title: Badanie metod Details i Delete | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: 'Uwaga: Dostępne jest zaktualizowana wersja tego samouczka, która korzysta z platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej stosować i pokaz...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 11425ff3-09fc-4efa-be9a-b53bce503460
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 455bdfd4cfca65cadd94e8ce189c1a185a03805d
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65129879"
---
# <a name="examining-the-details-and-delete-methods"></a>Badanie metod Details i Delete

Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Jest dostępna zaktualizowana wersja tego samouczka [tutaj](../../getting-started/introduction/getting-started.md) używającej platformy ASP.NET MVC 5 i Visual Studio 2013. Jest bardziej bezpieczne, łatwiej wykonać i pokazuje więcej funkcji.

W tej części samouczka możesz omówione automatycznie generowanych `Details` i `Delete` metody.

## <a name="examining-the-details-and-delete-methods"></a>Badanie metod Details i Delete

Otwórz `Movie` kontrolera i zbadaj `Details` metody.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

Aparat tworzenia szkieletów MVC, utworzony z tą metodą akcji dodaje komentarz przedstawiający żądania HTTP, który wywołuje tę metodę. W tym przypadku jest `GET` żądanie przy użyciu trzech segmenty adresu URL, `Movies` kontrolera, `Details` metody i `ID` wartość.

Kod najpierw ułatwia wyszukiwanie danych przy użyciu `Find` metody. Ważna funkcja zabezpieczeń wbudowanych w metodzie jest kod sprawdza, czy `Find` znaleziono filmu metody, zanim kod próbuje wykonywać żadnych czynności z nim. Na przykład haker może spowodować błędy do witryny, zmieniając adres URL utworzony przez łącza z `http://localhost:xxxx/Movies/Details/1` na wartość podobną `http://localhost:xxxx/Movies/Details/12345` (lub inną wartość, która nie zawiera rzeczywistych filmu). Jeśli nie zaznaczono dla filmu o wartości null, null filmu spowoduje błąd bazy danych.

Sprawdź `Delete` i `DeleteConfirmed` metody.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Należy pamiętać, że `HTTP Get``Delete` metoda nie powoduje usunięcia określonego filmu, zwraca widok filmu w trakcie którego można przesłać (`HttpPost`) usuwanie... Wykonywanie operacji usuwania w odpowiedzi na polecenie GET żądania (lub służącego wykonywania operacji Edytuj, Utwórz operacji lub innej operacji, które zmieniają dane) otwiera lukę w zabezpieczeniach. Aby uzyskać więcej informacji na ten temat, zobacz wpis w blogu Autor: Stephen Walther [46 Porada # w programie ASP.NET MVC — nie używaj usunąć łącza, ponieważ mogą tworzyć luki w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

`HttpPost` Nosi nazwę metody, która powoduje usunięcie danych `DeleteConfirmed` zapewnienie metodą HTTP POST unikatowy podpis lub nazwy. Poniżej przedstawiono podpisy dwóch metod:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Środowisko uruchomieniowe języka wspólnego (CLR) wymaga przeciążonej metody ma unikatowy parametr podpisu (tej samej nazwie metoda, ale inną listę parametrów). Jednak w tym miejscu należy dwie metody usuwania — jeden dla GET--i jeden dla wpisu czy obie pozycje mają taki sam podpis parametru. (Obaj użytkownicy muszą zaakceptować pojedyncze liczby całkowite jako parametr.)

Aby posortować tę możliwość, można zrobić kilka rzeczy. Jeden to nadać różne nazwy metody. To mechanizm tworzenia szkieletów została w poprzednim przykładzie. Jednak wprowadza mały problem: ASP.NET mapuje segmentów adresu URL do metody akcji według nazwy, a jeśli zmienisz metodę, routing zwykle nie można znaleźć tej metody. Rozwiązanie jest widoczny w tym przykładzie jest dodanie `ActionName("Delete")` atrybutu `DeleteConfirmed` metody. Skutecznie wykonuje to mapowanie systemu routingu, aby adres URL, który zawiera <em>/Delete/</em>dla wpisu znajdzie żądanie `DeleteConfirmed` metody.

Innym typowym sposobem uniknięcia problemu z metod, które mają identyczne nazwy i wzory podpisów jest sztucznie Zmień podpis metody POST, aby uwzględnić Nieużywany parametr. Na przykład niektórzy deweloperzy dodać typ parametru `FormCollection` który jest przekazywany do metody POST, a następnie po prostu nie używaj parametru:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Podsumowanie

Masz teraz kompletnej aplikacji ASP.NET MVC, która przechowuje dane w lokalnej bazie danych bazy danych. Można utworzyć, Odczyt, aktualizowanie, usuwanie i wyszukaj filmy.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Następne kroki

Po skompilowane i przetestowane aplikacji sieci web, następnym krokiem jest udostępnić go innym osobom korzystanie przez Internet. Aby to zrobić, należy wdrożyć ją do dostawcy usług hosta sieci web. Firma Microsoft oferuje bezpłatny internetowy hostowanie do 10 witryn sieci web w [bezpłatne konto wersji próbnej usługi Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Czy zasugerować obok wykonaj Moje samouczek [wdrażanie aplikacji zabezpieczenia platformy ASP.NET MVC z członkostwa, uwierzytelnianiem OAuth i bazy danych SQL Database to a Windows Azure Web Site](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Doskonałe samouczek jest poziomu pośredniego Tom Dykstra [Tworzenie modelu danych Entity Framework dla aplikacji ASP.NET MVC](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [Witryna StackOverflow](http://stackoverflow.com/help) i [fora platformy ASP.NET MVC](https://forums.asp.net/1146.aspx) są doskonałym umieszcza zadawać pytania. Postępuj zgodnie z [mnie](https://twitter.com/RickAndMSFT) w serwisie twitter, dzięki czemu można uzyskać aktualizacje na Moje najnowsze samouczki.

Opinia jest powitalnej.

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Poprzednie](adding-validation-to-the-model.md)

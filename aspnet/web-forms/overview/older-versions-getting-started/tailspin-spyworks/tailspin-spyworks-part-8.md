---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 'Część 8: ostateczne strony, obsługa wyjątków i wnioski | Microsoft Docs'
author: JoeStagner
description: Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 8 dodaje stronę kontaktu, stronę i wyjątek...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 707dc9d87ae324a7897c971a451e40bc54c96cb3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586885"
---
# <a name="part-8-final-pages-exception-handling-and-conclusion"></a>Część 8. Końcowe strony, obsługa wyjątków i podsumowanie

Jan [Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks ilustruje, w jaki sposób bardzo jest prosta, aby tworzyć zaawansowane, skalowalne aplikacje dla platformy .NET. W tym artykule przedstawiono sposób korzystania z doskonałych nowych funkcji w programie ASP.NET 4 do tworzenia sklepu online, w tym zakupów, wyewidencjonowywania i administrowania.
> 
> Ta seria samouczków zawiera szczegółowe informacje na temat wszystkich kroków podjętych w celu skompilowania przykładowej aplikacji Tailspin Spyworks. Część 8 dodaje stronę kontaktu, stronę i obsługę wyjątków. Jest to wniosek dotyczący serii.

## <a id="_Toc260221680"></a>Strona kontaktu (wysyłanie wiadomości e-mail z ASP.NET)

Utwórz nową stronę o nazwie ContactName. aspx

Korzystając z projektanta, Utwórz następującą postać specjalnej uwagi, aby uwzględnić ToolkitScriptManager i kontrolkę Edytor z AjaxControlToolkit. .

![](tailspin-spyworks-part-8/_static/image1.jpg)

Kliknij dwukrotnie przycisk "Prześlij", aby wygenerować procedurę obsługi zdarzeń kliknięcia w pliku związanym z kodem i zaimplementować metodę wysyłania informacji kontaktowych jako wiadomości e-mail.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

Ten kod wymaga, aby plik Web. config zawierał wpis w sekcji konfiguracji, który określa serwer SMTP, który ma być używany do wysyłania wiadomości e-mail.

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>Informacje o stronie

Utwórz stronę o nazwie about. aspx i Dodaj dowolną zawartość, którą lubisz.

## <a id="_Toc260221682"></a>Globalna procedura obsługi wyjątków

Wreszcie w całej aplikacji wystąpiły wyjątki i istnieją nieprzewidziane sytuacje, w których zimnie powodowało Nieobsłużone wyjątki w aplikacji sieci Web.

Nigdy nie chcemy wyświetlać nieobsłużonego wyjątku dla Gości witryny sieci Web.

![](tailspin-spyworks-part-8/_static/image2.jpg)

Oprócz tego Nieobsłużone wyjątki w środowisku użytkownika jaszczurów mogą być również problemem z zabezpieczeniami.

Aby rozwiązać ten problem, zaimplementujmy globalny program obsługi wyjątków.

Aby to zrobić, Otwórz plik Global. asax i zanotuj następujący wstępnie wygenerowany program obsługi zdarzeń.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

Dodaj kod, aby zaimplementować aplikację\_procedurę obsługi błędów w następujący sposób.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

Następnie Dodaj stronę o nazwie Error. aspx do rozwiązania i Dodaj ten fragment kodu znaczników.

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

Teraz na stronie,\_obsłudze zdarzeń ładowania, Wyodrębnij komunikaty o błędach z obiektu request.

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>Stwierdzeni

Zaobserwowano, że ASP.NET WebForms ułatwiają tworzenie zaawansowanej witryny sieci Web z dostępem do baz danych, członkostwem, AJAX itp. szybko.

Miejmy nadzieję ten samouczek zawiera narzędzia, które są potrzebne, aby rozpocząć tworzenie własnych aplikacji ASP.NET WebForms!

> [!div class="step-by-step"]
> [Wstecz](tailspin-spyworks-part-7.md)

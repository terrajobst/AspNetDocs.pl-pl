---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: Badanie metod edycji i widoku edycji | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/06/2019
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: 6cef963910b957e8b4ad7c7909385f6dbdff95c1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582440"
---
# <a name="examining-the-edit-methods-and-edit-view"></a>Badanie metod edycji i widoku edycji

Autor [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

W tej sekcji poznasz wygenerowane `Edit` metody i widoki akcji dla kontrolera filmu. Jednak najpierw zajmiemy się krótkim kierowaniem, aby Data wydania była lepsza. Otwórz plik *Models\Movie.cs* i Dodaj wyróżnione wiersze poniżej:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

Można również wprowadzić określoną kulturę dat w następujący sposób:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Będziemy ujawniać [Adnotacje](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) w następnym samouczku. Atrybut [Display](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) określa, co ma być wyświetlane dla nazwy pola (w tym przypadku "Data wydania" zamiast "ReleaseDate"). Atrybut [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) określa typ danych, w tym przypadku jest to data, więc informacje o czasie przechowywane w polu nie są wyświetlane. Atrybut [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) jest wymagany w przypadku usterki w przeglądarce Chrome, która niepoprawnie renderuje formaty dat.

Uruchom aplikację i przejdź do kontrolera `Movies`. Przytrzymaj wskaźnik myszy nad linkiem **edycji** , aby wyświetlić adres URL, do którego prowadzi połączenie.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

Link **edycji** został wygenerowany przez metodę `Html.ActionLink` w widoku *Views\Movies\Index.cshtml* :

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

Obiekt `Html` jest pomocnika, który jest uwidoczniony przy użyciu właściwości w klasie bazowej [System. Web. MVC. WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) . `ActionLink` metoda pomocnika ułatwia dynamiczne generowanie hiperłączy HTML, które łączą się z metodami akcji na kontrolerach. Pierwszym argumentem metody `ActionLink` jest tekst łącza do renderowania (na przykład `<a>Edit Me</a>`). Drugi argument to nazwa metody akcji do wywołania (w tym przypadku akcja `Edit`). Ostatnim argumentem jest [obiekt anonimowy](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) generujący dane trasy (w tym przypadku Identyfikator 4).

Wygenerowane łącze pokazane na poprzednim obrazie jest `http://localhost:1234/Movies/Edit/4`. Trasa domyślna (ustalona w *aplikacji\_Start\RouteConfig.cs*) przyjmuje `{controller}/{action}/{id}`wzorca adresu URL. W związku z tym ASP.NET tłumaczy `http://localhost:1234/Movies/Edit/4` na żądanie do `Edit` metody akcji kontrolera `Movies` z parametrem `ID` równy 4. Przejrzyj następujący kod z *aplikacji\_pliku Start\RouteConfig.cs* . Metoda [plik maproute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) służy do kierowania żądań HTTP do poprawnego kontrolera i metody akcji oraz do dostarczania opcjonalnego parametru ID. Metoda [plik maproute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) jest również używana przez [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) , takie jak `ActionLink` do generowania adresów URL na podstawie kontrolera, metody akcji i wszelkich danych tras.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

Parametry metody akcji można także przekazać za pomocą ciągu zapytania. Na przykład adres URL `http://localhost:1234/Movies/Edit?ID=3` przekazuje także parametr `ID` 3 do metody akcji `Edit` kontrolera `Movies`.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Otwórz kontroler `Movies`. Poniżej przedstawiono dwie `Edit` metod akcji.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Zwróć uwagę, że druga metoda działania `Edit` jest poprzedzona atrybutem `HttpPost`. Ten atrybut określa, że Przeciążenie metody `Edit` mogą być wywoływane tylko w przypadku żądań POST. Do pierwszej metody edycji można zastosować atrybut `HttpGet`, ale nie jest to konieczne, ponieważ jest to wartość domyślna. (Odwołujemy się do metod akcji, które są niejawnie przypisane do `HttpGet` atrybutu jako metody `HttpGet`). Atrybut [bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) jest innym ważnym mechanizmem zabezpieczeń, który zapobiega hakerom nad nadmiernym ogłaszaniem danych w modelu. Należy uwzględnić tylko właściwości w atrybucie bind, który ma zostać zmieniony. Informacje o przepełnieniu i atrybucie bind można znaleźć w artykule dotyczącym nadmiarowego [zaewidencjonowania zabezpieczeń](https://go.microsoft.com/fwlink/?LinkId=317598). W prostym modelu używanym w tym samouczku będziemy wiązać wszystkie dane w modelu. Atrybut [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) służy do zapobiegania fałszerstwu żądania i jest sparowany z `@Html.AntiForgeryToken()` w pliku widoku edycji (*Views\Movies\Edit.cshtml*), poniżej przedstawiono część poniżej:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()` generuje ukryty certyfikat ochrony przed fałszowaniem, który musi być zgodny w metodzie `Edit` kontrolera `Movies`. Więcej informacji na temat fałszerstwa żądań między witrynami (znanych również jako XSRF lub CSRF) można znaleźć w sekcji mój samouczek [XSRF/CSRF zapobiegania w MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

Metoda `HttpGet` `Edit` przyjmuje parametr identyfikatora filmu, wyszukuje film przy użyciu metody Entity Framework `Find` i zwraca wybrany film do widoku edycji. Jeśli nie można znaleźć filmu, zwracany jest [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) . Gdy system szkieletu utworzył widok edycji, zbadamy klasę `Movie` i utworzono kod w celu renderowania `<label>` i `<input>` elementów dla każdej właściwości klasy. W poniższym przykładzie przedstawiono widok edycji, który został wygenerowany przez system szkieletu programu Visual Studio:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Zauważ, w jaki sposób szablon widoku zawiera instrukcję `@model MvcMovie.Models.Movie` w górnej części pliku — określa, że widok oczekuje, że model dla szablonu ma być typu `Movie`.

Kod szkieletowy używa kilku *metod pomocniczych* do uproszczenia znaczników HTML. Pomocnik [`Html.LabelFor`](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) wyświetla nazwę pola (&quot;tytułu&quot;, &quot;ReleaseDate&quot;, &quot;gatunek&quot;lub &quot;cen&quot;). Pomocnik [`Html.EditorFor`](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) renderuje element HTML `<input>`. Pomocnik [`Html.ValidationMessageFor`](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) wyświetla wszystkie komunikaty weryfikacyjne skojarzone z tą właściwością.

Uruchom aplikację i przejdź do adresu URL */Movies* . Kliknij link **Edytuj** . Sprawdź Źródło strony w przeglądarce. KOD HTML dla elementu form jest przedstawiony poniżej.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

Elementy `<input>` znajdują się w elemencie HTML `<form>`, którego atrybut `action` jest ustawiony na wartość post na adres URL */Movies/Edit* . Dane formularza zostaną opublikowane na serwerze po kliknięciu przycisku **Zapisz** . W drugim wierszu jest wyświetlany ukryty token [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) generowany przez wywołanie `@Html.AntiForgeryToken()`.

## <a name="processing-the-post-request"></a>Przetwarzanie żądania POST

Na poniższej liście przedstawiono `HttpPost` wersji metody akcji `Edit`.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

Atrybut [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) sprawdza poprawność tokenu [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) wygenerowanego przez wywołanie `@Html.AntiForgeryToken()` w widoku.

[Spinacz modelu ASP.NET MVC](https://msdn.microsoft.com/library/dd410405.aspx) przyjmuje wartości ogłoszonych formularzy i tworzy obiekt `Movie`, który jest przesyłany jako parametr `movie`. `ModelState.IsValid` sprawdza, czy dane przesłane w formularzu mogą służyć do modyfikowania (edycji lub aktualizowania) `Movie` obiektu. Jeśli dane są prawidłowe, dane filmu są zapisywane do kolekcji `Movies` `db`(wystąpienie`MovieDBContext`). Nowe dane filmu są zapisywane w bazie danych przez wywołanie metody `SaveChanges` `MovieDBContext`. Po zapisaniu danych kod przekierowuje użytkownika do metody akcji `Index` klasy `MoviesController`, co spowoduje wyświetlenie kolekcji filmów, w tym właśnie wprowadzonych zmian.

Gdy tylko Walidacja po stronie klienta określi wartość pola jest nieprawidłowa, zostanie wyświetlony komunikat o błędzie. Jeśli język JavaScript jest wyłączony, sprawdzanie poprawności po stronie klienta jest wyłączone. Jednak serwer wykrywa ogłoszone wartości są nieprawidłowe, a wartości formularza są wyświetlane w komunikatach o błędach.

Weryfikacja została sprawdzona bardziej szczegółowo w dalszej części samouczka.

`Html.ValidationMessageFor` pomocników w szablonie widoku *Edit. cshtml* są zgodne z wyświetlaniem odpowiednich komunikatów o błędach.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Wszystkie metody `HttpGet` są zgodne z podobnym wzorcem. Uzyskują one obiekt filmu (lub listę obiektów w przypadku `Index`) i przekaże model do widoku. Metoda `Create` przekazuje pusty obiekt filmu do widoku tworzenia. Wszystkie metody, które tworzą, edytują, usuwają lub w inny sposób modyfikują dane, to w `HttpPost` Przeciążenie metody. Modyfikowanie danych w metodzie HTTP GET stanowi zagrożenie bezpieczeństwa, zgodnie z opisem w wpisie w blogu [ASP.NET #46 etykietki MVC — nie używaj linków usuwania, ponieważ tworzą one luki w zabezpieczeniach](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Modyfikowanie danych w metodzie GET narusza także najlepsze rozwiązania protokołu HTTP i wzorzec [rest](http://en.wikipedia.org/wiki/Representational_State_Transfer) architektury, który określa, że żądania GET nie powinny zmieniać stanu aplikacji. Innymi słowy wykonanie operacji GET powinno być operacją bezpieczną, która nie ma efektów ubocznych i nie modyfikuje utrwalonych danych.

## <a name="jquery-validation-for-non-english-locales"></a>Weryfikacja platformy jQuery dla ustawień regionalnych innych niż angielski

Jeśli używasz komputera z USA, możesz pominąć tę sekcję i przejść do następnego samouczka. Wersję z tego samouczka można pobrać [tutaj](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Aby zapoznać się z doskonałym samouczkiem dotyczącym aplikacji wielojęzycznych, zobacz [Nadeem ASP.NET MVC 5](http://afana.me/post/aspnet-mvc-internationalization.aspx).

> [!NOTE]
> Aby zapewnić obsługę walidacji jQuery dla ustawień regionalnych innych niż angielskie, które używają przecinka (&quot;,&quot;) dla punktu dziesiętnego i formatów daty innych niż angielski, należy dołączyć plik *globalizacjs. js* i określone *kultury/globalizacja pliku kultury. js* (z [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) i JavaScript, aby użyć `Globalize.parseFloat`. Można uzyskać weryfikację platformy jQuery w języku innym niż angielski z narzędzia NuGet. (Nie instaluj globalizacji, jeśli używasz ustawień regionalnych w języku angielskim).

1. W menu **Narzędzia** kliknij pozycję **Menedżer pakietów NuGet**, a następnie kliknij pozycję **Zarządzaj pakietami NuGet dla rozwiązania**.

    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. W okienku po lewej stronie wybierz pozycję <strong>Przeglądaj *.</strong>*  (Zobacz poniższy obraz).
3. W polu wejściowym wpisz * globalizacja * *.

    ![](examining-the-edit-methods-and-edit-view/_static/image6.png) wybierz `jQuery.Validation.Globalize`, wybierz `MvcMovie` i kliknij przycisk **Instaluj**. Plik *Scripts\jquery.globalize\globalize.js* zostanie dodany do projektu. Folder * Scripts\jquery.globalize\cultures\* będzie zawierać wiele plików języka JavaScript. Należy pamiętać, że instalacja tego pakietu może potrwać pięć minut.

   Poniższy kod przedstawia modyfikacje pliku Views\Movies\Edit.cshtml:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Aby uniknąć powtarzania tego kodu w każdym widoku edycji, można przenieść go do pliku układu. Aby zoptymalizować pobieranie skryptu, zobacz mój samouczek Tworzenie [i minifikacja](../../performance/bundling-and-minification.md).

Aby uzyskać więcej informacji, zobacz [ASP.NET MVC 3 — wielojęzyczne](http://afana.me/post/aspnet-mvc-internationalization.aspx) i [ASP.NET MVC 3 — część 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Jeśli nie możesz uzyskać walidacji działającej w ustawieniach regionalnych, możesz wymusić, aby komputer mógł korzystać z języka angielskiego w języku angielskim lub można wyłączyć język JavaScript w przeglądarce. Aby wymusić na komputerze używanie języka angielskiego w języku angielskim, można dodać element globalizacji do głównego pliku *Web. config* projektów. Poniższy kod przedstawia element globalizacji z kulturą ustawioną na Stany Zjednoczone w języku angielskim.

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a>W następnym samouczku zaimplementowano funkcję wyszukiwania.

> [!div class="step-by-step"]
> [Poprzednie](accessing-your-models-data-from-a-controller.md)
> [dalej](adding-search.md)

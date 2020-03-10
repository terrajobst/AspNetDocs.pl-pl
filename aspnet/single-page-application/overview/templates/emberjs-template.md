---
uid: single-page-application/overview/templates/emberjs-template
title: Szablon EmberJS | Microsoft Docs
author: xqiu
description: Szablon EmberJS
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1aefa46dd0841b1b06675409cc8a09f9a218d7ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578506"
---
# <a name="emberjs-template"></a>Szablon EmberJS

Autor [Xinyang Qiu](https://github.com/xqiu)

> Szablon EmberJS MVC jest pisany przez Nathana Totten, Thiago Santos i Xinyang Qiu.
> 
> [Pobierz szablon EmberJS MVC](https://go.microsoft.com/fwlink/?LinkId=282647)

Szablon SPA EmberJS został zaprojektowany, aby szybko rozpocząć tworzenie interaktywnych aplikacji sieci Web po stronie klienta przy użyciu EmberJS.

"Aplikacja jednostronicowa" (SPA) to ogólny termin aplikacji sieci Web, który ładuje pojedynczą stronę HTML, a następnie automatycznie aktualizuje stronę, zamiast ładować nowe strony. Po początkowym załadowaniu strony SPA komunikuje się z serwerem przez żądania AJAX.

![](emberjs-template/_static/image1.png)

Technologia AJAX nie ma nic nowego, ale dzisiaj istnieją struktury języka JavaScript, które ułatwiają tworzenie i konserwowanie dużej zaawansowanej aplikacji SPA. Ponadto kod HTML 5 i CSS3 ułatwiają tworzenie bogatych interfejsów użytkownika.

Szablon SPA EmberJS używa biblioteki JavaScript [wpływ](http://emberjs.com/) do obsługi aktualizacji stron z żądań AJAX. Wpływ. js używa powiązania danych do synchronizowania strony z najnowszymi danymi. Dzięki temu nie trzeba pisać żadnego kodu, który przegląda dane JSON i aktualizuje DOM. Zamiast tego należy umieścić atrybuty deklaracyjne w kodzie HTML, które informują wpływ. js, jak przedstawić dane.

Po stronie serwera szablon EmberJS jest niemal identyczny z [szablonem Spa KnockoutJS](../introduction/knockoutjs-template.md). Używa ASP.NET MVC do obsługi dokumentów HTML i ASP.NET interfejsu API sieci Web do obsługi żądań AJAX od klienta. Aby uzyskać więcej informacji na temat tych aspektów szablonu, zapoznaj się z dokumentacją [szablonu KnockoutJS](../introduction/knockoutjs-template.md) . W tym temacie omówiono różnice między szablonem odcinania i szablonem EmberJS.

## <a name="create-an-emberjs-spa-template-project"></a>Utwórz projekt szablonu SPA EmberJS

Pobierz i zainstaluj szablon, klikając przycisk Pobierz powyżej. Może być konieczne ponowne uruchomienie programu Visual Studio.

W okienku **Szablony** wybierz pozycję **zainstalowane szablony** i rozwiń węzeł **Wizualizacja C#**  . W **obszarze C#Wizualizacja** wybierz pozycję **Sieć Web**. Na liście szablonów projektu wybierz pozycję **aplikacja sieci Web MVC 4 ASP.NET**. Nazwij projekt, a następnie kliknij przycisk **OK**.

![](emberjs-template/_static/image2.png)

W kreatorze **nowego projektu** wybierz **projekt wpływ. js Spa**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Przegląd szablonu SPA EmberJS

Szablon EmberJS używa kombinacji jQuery, wpływ. js, kierownicy. js, aby utworzyć płynny, interaktywny interfejs użytkownika.

Wpływ. js to biblioteka języka JavaScript, która korzysta ze wzorca MVC po stronie klienta.

- *Szablon*, zapisany w języku tworzenia szablonów, zawiera opis interfejsu użytkownika aplikacji. W trybie wydania [kompilator kierownicy](https://github.com/Myslik/csharp-ember-handlebars) służy do łączenia i kompilowania szablonu kierownicy.
- *Model* przechowuje dane aplikacji, które pobiera z serwera (listy zadań do zrobienia i zadania do wykonania).
- *Kontroler* przechowuje stan aplikacji. Kontrolery często są obecne dane modelu do odpowiednich szablonów.
- *Widok umożliwia* przetłumaczenie zdarzeń pierwotnych z aplikacji i przekazanie ich do kontrolera.
- *Router* zarządza stanem aplikacji, utrzymując w synchronizacji adresy URL i szablony.

Ponadto biblioteka danych wpływ może służyć do synchronizowania obiektów JSON (uzyskanych z serwera za pośrednictwem interfejsu API RESTful) i modeli klienta.

Szablon EmberJS SPA organizuje skrypty w osiem warstw:

- WebAPI\_adapter. js, WebAPI\_serializator. js: rozszerza bibliotekę danych wpływ do pracy z interfejsem API sieci Web ASP.NET.
- Skrypty/pomocniks. js: definiuje nowych pomocników wpływ kierownicy.
- Skrypty/App. js: tworzy aplikację i konfiguruje kartę i Serializator.
- Skrypty/aplikacje/modele/\*. js: definiuje modele.
- Skrypty/aplikacja/widoki/\*. js: definiuje widoki.
- Skrypty/aplikacja/kontrolery/\*. js: definiuje kontrolery.
- Skrypty/aplikacje/trasy, skrypty/aplikacja/router. js: definiuje trasy.
- Szablony/\*. HBS: definiuje szablony kierownicy.

Przyjrzyjmy się kilku skryptom bardziej szczegółowym.

## <a name="models"></a>Modele

Modele są zdefiniowane w folderze skrypty/aplikacja/modele. Istnieją dwa pliki modelu: todoItem. js i todoList. js.

do **zrobienia. model. js** definiuje modele po stronie klienta (przeglądarki) dla list zadań do wykonania. Istnieją dwie klasy modelu: todoItem i todoList. W wpływ, modele są podklasami DS. Wzorów. Model może mieć właściwości z atrybutami:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

Modele mogą definiować relacje z innymi modelami:

[!code-css[Main](emberjs-template/samples/sample2.css)]

Modele mogą mieć obliczone właściwości, które są powiązane z innymi właściwościami:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

Modele mogą mieć funkcje obserwatorów, które są wywoływane, gdy obserwowana właściwość zostanie zmieniona:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Widoki

Widoki są zdefiniowane w folderze skrypty/aplikacje/widoki. Widok umożliwia przetłumaczenie zdarzeń z interfejsu użytkownika aplikacji. Procedura obsługi zdarzeń może wywoływać z powrotem do funkcji kontrolera lub bezpośrednio wywołać kontekst danych.

Na przykład poniższy kod pochodzi z widoku/TodoItemEditView. js. Definiuje obsługę zdarzeń dla wejściowego pola tekstowego.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Kontroler

Kontrolery są zdefiniowane w folderze skrypty/aplikacja/kontrolery. Aby przedstawić jeden model, należy zwiększyć `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Kontroler może również reprezentować kolekcję modeli, rozszerzając `Ember.ArrayController`. Na przykład TodoListController reprezentuje tablicę obiektów `todoList`. Kontroler sortuje według identyfikatora todoList, w kolejności malejącej:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Kontroler definiuje funkcję o nazwie `addTodoList`, która tworzy nowy todoList i dodaje ją do tablicy. Aby zobaczyć, jak ta funkcja jest wywoływana, Otwórz plik szablonu o nazwie todoListTemplate. html w folderze Templates. Poniższy kod szablonu wiąże przycisk z funkcją `addTodoList`:

[!code-html[Main](emberjs-template/samples/sample8.html)]

Kontroler zawiera również właściwość `error`, która zawiera komunikat o błędzie. Oto kod szablonu do wyświetlania komunikatu o błędzie (również w todoListTemplate. html):

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Trasy

Router. js definiuje trasy i domyślny szablon do wyświetlania, konfiguruje stan aplikacji i dopasowuje adresy URL do tras:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute. js ładuje dane dla TodoListRoute, zastępując funkcję setupController:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Wpływ używa konwencji nazewnictwa w celu dopasowania adresów URL, nazw tras, kontrolerów i szablonów. Aby uzyskać więcej informacji, zobacz [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) w dokumentacji EmberJS.

## <a name="templates"></a>Szablony

Folder szablonów zawiera cztery szablony:

- Application. HBS: domyślny szablon, który jest renderowany podczas uruchamiania aplikacji.
- about. HBS: szablon dla trasy "/about".
- index. HBS: szablon dotyczący trasy głównej "/".
- todoList. HBS: szablon dla trasy "/Todo".
- \_pasek nawigacyjny. HBS: szablon definiuje menu nawigacji.

Szablon aplikacji działa jak strona wzorcowa. Zawiera nagłówek, stopkę i "{{gniazdko}}", aby wstawić inne szablony w zależności od trasy. Aby uzyskać więcej informacji na temat szablonów aplikacji w programie wpływ, zobacz [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

Szablon "/todoList" zawiera dwa wyrażenia pętli. Pętla zewnętrzna jest `{{#each controller}}`, a pętla wewnątrz jest `{{#each todos}}`. Poniższy kod przedstawia wbudowany widok `Ember.Checkbox`, dostosowany `App.TodoItemEditView`i link z akcją `deleteTodo`.

[!code-html[Main](emberjs-template/samples/sample12.html)]

Klasa `HtmlHelperExtensions` zdefiniowana w obszarze controllers/HtmlHelperExtensions. cs definiuje funkcję pomocnika do buforowania i wstawiania plików szablonów, gdy **debugowanie** jest ustawione na **wartość true** w pliku Web. config. Ta funkcja jest wywoływana z pliku widoku ASP.NET MVC zdefiniowanego w widokach/Home/App. cshtml:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Wywoływana bez argumentów, funkcja renderuje wszystkie pliki szablonu w folderze Templates. Można również określić podfolder lub określony plik szablonu.

Gdy **debugowanie** ma **wartość false** w pliku Web. config, aplikacja zawiera element pakietu "~/bundles/templates". Ten element pakietu jest dodawany w BundleConfig.cs, przy użyciu biblioteki współpracującej z kompilatorem:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]

---
ms.openlocfilehash: 3be420696add54094f24424285a905e7e7963bad
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57073874"
---
# <a name="bootstraphttpgetbootstrapcom"></a>[Bootstrap](http://getbootstrap.com)

[![Slack](https://bootstrap-slack.herokuapp.com/badge.svg)](https://bootstrap-slack.herokuapp.com)
![wersji Bower](https://img.shields.io/bower/v/bootstrap.svg)
[![wersji npm](https://img.shields.io/npm/v/bootstrap.svg)](https://www.npmjs.com/package/bootstrap)
[![Stan kompilacji](https://img.shields.io/travis/twbs/bootstrap/master.svg)](https://travis-ci.org/twbs/bootstrap) 
 [ ![devDependency stan](https://img.shields.io/david/dev/twbs/bootstrap.svg)](https://david-dm.org/twbs/bootstrap#info=devDependencies)
[![NuGet](https://img.shields.io/nuget/v/bootstrap.svg)](https://www.nuget.org/packages/Bootstrap)
[![Selenium stan testu](https://saucelabs.com/browser-matrix/bootstrap.svg)](https://saucelabs.com/u/bootstrap)

Bootstrap to elegancki, intuicyjny i wydajny frontonu środowisko do programowania dla sieci web szybciej i łatwiej, utworzone przez [Otto znacznik](https://twitter.com/mdo) i [Jacob Thornton](https://twitter.com/fat)i konserwowane przez [podstawowe zespołu](https://github.com/orgs/twbs/people) dzięki obsłudze dużych oraz udziałem społeczności.

Aby rozpocząć, zapoznaj się z <http://getbootstrap.com>!


## <a name="table-of-contents"></a>Spis treści

* [Przewodnik Szybki start](#quick-start)
* [Usterki i sugestie funkcji](#bugs-and-feature-requests)
* [Dokumentacja](#documentation)
* [Współtworzenie](#contributing)
* [Community](#community)
* [Przechowywanie wersji](#versioning)
* [Dla twórców](#creators)
* [Prawa autorskie i licencji](#copyright-and-license)


## <a name="quick-start"></a>Przewodnik Szybki start

Dostępnych jest kilka opcji szybkiego startu:

* [Pobieranie najnowszej wersji](https://github.com/twbs/bootstrap/archive/v3.3.7.zip).
* Sklonuj repozytorium: `git clone https://github.com/twbs/bootstrap.git`.
* Instalowanie przy użyciu [Bower](http://bower.io): `bower install bootstrap`.
* Instalowanie przy użyciu [npm](https://www.npmjs.com): `npm install bootstrap@3`.
* Instalowanie przy użyciu [Meteor](https://www.meteor.com): `meteor add twbs:bootstrap`.
* Instalowanie przy użyciu [Composer](https://getcomposer.org): `composer require twbs/bootstrap`.

Odczyt [stronie wprowadzenie](http://getbootstrap.com/getting-started/) uzyskać informacji na temat zawartość framework, szablony i przykłady i inne.

### <a name="whats-included"></a>Zawartość pakietu

W ramach pliki do pobrania można znaleźć następujących katalogów i plików, logicznie grupowania typowych zasobów i jednocześnie zapewniając kompilowane i zminimalizowania odmiany. Zostanie wyświetlony podobny do poniższego:

```
bootstrap/
├── css/
│   ├── bootstrap.css
│   ├── bootstrap.css.map
│   ├── bootstrap.min.css
│   ├── bootstrap.min.css.map
│   ├── bootstrap-theme.css
│   ├── bootstrap-theme.css.map
│   ├── bootstrap-theme.min.css
│   └── bootstrap-theme.min.css.map
├── js/
│   ├── bootstrap.js
│   └── bootstrap.min.js
└── fonts/
    ├── glyphicons-halflings-regular.eot
    ├── glyphicons-halflings-regular.svg
    ├── glyphicons-halflings-regular.ttf
    ├── glyphicons-halflings-regular.woff
    └── glyphicons-halflings-regular.woff2
```

Firma Microsoft zapewnia skompilowanych CSS i JS (`bootstrap.*`), a także kompilowane i zminimalizowany CSS i JS (`bootstrap.min.*`). CSS [źródła mapy](https://developer.chrome.com/devtools/docs/css-preprocessors) (`bootstrap.*.map`) są dostępne do użycia z narzędzi deweloperskich w niektórych przeglądarkach. Czcionki z Glyphicons są dołączane, ponieważ jest opcjonalny motywie Bootstrap.


## <a name="bugs-and-feature-requests"></a>Usterki i sugestie funkcji

Masz usterkę lub wniosek dotyczący funkcji? Najpierw przeczytaj [wydaje wytyczne](https://github.com/twbs/bootstrap/blob/master/CONTRIBUTING.md#using-the-issue-tracker) i wyszukiwanie istniejących i zamkniętych problemów. Jeśli problem lub pomysł nie uwzględnia jeszcze [Otwórz nowy problem](https://github.com/twbs/bootstrap/issues/new).

Należy pamiętać, że **musi być przeznaczony dla żądania funkcji [Bootstrap v4](https://github.com/twbs/bootstrap/tree/v4-dev),** ponieważ ładowania początkowego v3 jest teraz w trybie konserwacji i jest zamknięte do nowych funkcji. Jest to, dzięki czemu możemy skoncentrować wysiłki na Bootstrap v4.


## <a name="documentation"></a>Dokumentacja

Dokumentację firmy bootstrap, zawarte w tym repozytorium w katalogu głównym został utworzony za pomocą [Jekyll](http://jekyllrb.com) i publicznie hostowanej na stronach GitHub w <http://getbootstrap.com>. Dokumenty mogą być także uruchamiane lokalnie.

### <a name="running-documentation-locally"></a>Dokumentacja usługi uruchomionej na komputerze lokalnym

1. Jeśli to konieczne, [zainstalować Jekyll](http://jekyllrb.com/docs/installation) oraz inne zależności języka Ruby przy użyciu `bundle install`.
   **Uwaga dla użytkowników Windows:** Odczyt [przewodniku nieoficjalny](http://jekyll-windows.juthilo.com/) można pobrać Jekyll działa bez problemów.
2. Z głównego `/bootstrap` katalogu, uruchom `bundle exec jekyll serve` w wierszu polecenia.
4. Otwórz `http://localhost:9001` w przeglądarce i voilà.

Dowiedz się więcej o używaniu Jekyll, zapoznając się jego [dokumentacji](http://jekyllrb.com/docs/home/).

### <a name="documentation-for-previous-releases"></a>Dokumentacja poprzednich wersji

Dokumentacja v2.3.2 został udostępniony w chwili obecnej w <http://getbootstrap.com/2.3.2/> podczas spamerzy przejście do Bootstrap 3.

[Poprzednie wersje](https://github.com/twbs/bootstrap/releases) i ich dokumentację są również dostępne do pobrania.


## <a name="contributing"></a>Współtworzenie

Przeczytaj nasze [współtworzenia wytycznych](https://github.com/twbs/bootstrap/blob/master/CONTRIBUTING.md). Zawarte są wskazówki dotyczące otwierania problemów, kodowania, standardów i uwagi dotyczące projektowania.

Ponadto, jeśli żądanie ściągnięcia zawiera poprawki JavaScript lub funkcji, musisz dołączyć [testów jednostkowych odpowiednich](https://github.com/twbs/bootstrap/tree/master/js/tests). Wszystkie HTML i CSS powinny być zgodne z [przewodnik kodu](https://github.com/mdo/code-guide), aktualizowane przez [Otto znacznik](https://github.com/mdo).

**Bootstrap w wersji 3 jest teraz zamknięte do nowych funkcji.** Stała się ona w trybie konserwacji, dzięki czemu możemy skoncentrować wysiłki na [Bootstrap v4](https://github.com/twbs/bootstrap/tree/v4-dev), przyszłość Framework. Żądania ściągnięcia, które dodania nowych funkcji (zamiast naprawiaj usterki) powinien dotyczyć [Bootstrap v4 ( `v4-dev` gałęzi git)](https://github.com/twbs/bootstrap/tree/v4-dev) zamiast tego.

Preferencje edytora są dostępne w [edytora konfiguracji](https://github.com/twbs/bootstrap/blob/master/.editorconfig) proste użytku we wspólnych edytorów tekstu. Dowiedz się więcej i Pobierz wtyczek w <http://editorconfig.org>.


## <a name="community"></a>Społeczność

Pobieranie aktualizacji na komputerach deweloperskich i porozmawiaj z maintainers projektu i członków społeczności firmy Bootstrap.

* Postępuj zgodnie z [ @getbootstrap w serwisie Twitter](https://twitter.com/getbootstrap).
* Przeczytaj i subskrybować [oficjalnym blogu Bootstrap](http://blog.getbootstrap.com).
* Dołącz do [oficjalne pokoju Slack](https://bootstrap-slack.herokuapp.com).
* Porozmawiaj z zespołu programów inicjujących w IRC. Na `irc.freenode.net` serwera w `##bootstrap` kanału.
* Implementacja pomocy można znaleźć w witrynie Stack Overflow (oznakowane [ `twitter-bootstrap-3` ](https://stackoverflow.com/questions/tagged/twitter-bootstrap-3)).
* Deweloperzy, należy użyć słowa kluczowego `bootstrap` w pakietach, które zmodyfikować lub dodać do funkcji Bootstrap podczas dystrybucji za pośrednictwem [npm](https://www.npmjs.com/browse/keyword/bootstrap) lub podobne mechanizmów dostawy maksymalny umożliwi to ich odnajdowanie.


## <a name="versioning"></a>Obsługa wersji

Przezroczystości na nasz cykl wydawania i fundusze zachować zgodność z poprzednimi wersjami, ładowania początkowego jest utrzymywany w [wytycznych Semantic Versioning](http://semver.org/). Czasami firma Microsoft przykręcenia w, ale firma Microsoft będzie stosować się do tych zasad, jeśli to możliwe.

Zobacz [wersji część naszego projektu GitHub](https://github.com/twbs/bootstrap/releases) dla changelogs dla każdej wersji Bootstrap. Zwolnij wpisy ogłoszenie na [Oficjalny blog Bootstrap](http://blog.getbootstrap.com) zawiera podsumowanie najbardziej warte zauważenia zmiany wprowadzone w każdej wersji.


## <a name="creators"></a>Dla twórców

**Oznacz Otto**

* <https://twitter.com/mdo>
* <https://github.com/mdo>

**Jacob Thornton**

* <https://twitter.com/fat>
* <https://github.com/fat>


## <a name="copyright-and-license"></a>Prawa autorskie i licencji

Kod i dokumentacji o prawach autorskich 2011-2016 Twitter, Inc. Kod wydawane na mocy [licencją MIT](https://github.com/twbs/bootstrap/blob/master/LICENSE). Dokumentacja wydawane na mocy [Creative Commons](https://github.com/twbs/bootstrap/blob/master/docs/LICENSE).

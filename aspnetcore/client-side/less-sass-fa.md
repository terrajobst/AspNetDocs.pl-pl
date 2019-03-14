---
title: Mniej, Sass i Font Awesome w programie ASP.NET Core
author: ardalis
description: Dowiedz się, jak używać mniej, Sass i Font Awesome w aplikacji platformy ASP.NET Core.
ms.author: tdykstra
ms.date: 10/14/2016
uid: client-side/less-sass-fa
ms.openlocfilehash: 2229c4e3b0238ff17c15e78f657b9acb10495c72
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57078335"
---
# <a name="less-sass-and-font-awesome-in-aspnet-core"></a>Mniej, Sass i Font Awesome w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/)

Użytkownicy aplikacji sieci web mieć zapewniał wysoką oczekiwania, jeśli chodzi o stylu i ogólne środowisko. Nowoczesnych aplikacji sieci web często korzystać z zaawansowanych narzędzi i platform do definiowania i zarządzanie ich wygląd i działanie w spójny sposób. Struktury, takich jak [Bootstrap](http://getbootstrap.com/) przejść długą drogę w definiowaniu wspólny zbiór — style i opcje układu dla witryn sieci web. Jednak większość witryn nietrywialnymi również korzystać z można skutecznie zdefiniować oraz Obsługa stylów i plików kaskadowych (CSS) arkuszy stylów, a także łatwy dostęp do ikony-image, które sprawić, że interfejs witryny bardziej intuicyjne. To tutaj języki i narzędzia, które obsługują [mniej](http://lesscss.org/) i [Sass](http://sass-lang.com/), i bibliotek, takich jak [Font Awesome](http://fontawesome.io/), są dostępne w.

## <a name="css-preprocessor-languages"></a>Języki preprocesora CSS

Języki, które są kompilowane do innych języków, aby ulepszyć środowisko pracy z językiem podstawowej są określane jako preprocesorami standardów. Istnieją dwa popularne preprocesorami standardów CSS: Mniej i rozszerzenie Sass.  Te preprocesorami standardów dodać funkcje do arkusza CSS, takie jak obsługa zmiennych i zagnieżdżone reguły, które poprawić łatwość dużych, złożonych arkuszy stylów. CSS jako język jest bardzo proste, bez pomocy technicznej, nawet w przypadku coś tak proste, jak zmienne, a jest to prawdopodobnie, że pliki CSS powtarzających się i przeglądarek. Dodawanie funkcji języka rzeczywiste programowania za pomocą preprocesorami standardów może pomóc zmniejszyć dublowania i zapewnienia lepszej organizacji reguł stylów. Program Visual Studio udostępnia wbudowaną obsługę zarówno Less i Sass, jak również rozszerzeń, które można jeszcze bardziej poprawić środowisko programistyczne, pracując z tych języków.

Jako prosty przykład jak preprocesorami standardów może zwiększyć czytelność i łatwość konserwacji informacje dotyczące stylu należy wziąć pod uwagę ten CSS:

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

Za pomocą mniejsze, to może zostać przepisane, aby wyeliminować wszystkich duplikatów, za pomocą *domieszki* (o nazwie tak, ponieważ umożliwia on "mieszać w" właściwości z jednej klasy lub zestawu reguł do innego):

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a>less

Przebiegi preprocesora mniej CSS przy użyciu środowiska Node.js. Aby zainstalować mniejsza, należy użyć Node Package Manager (npm) z wiersza polecenia (-g oznacza "global"):

```console
npm install -g less
```

Jeśli używasz programu Visual Studio, możesz rozpocząć pracę za pomocą Less, dodając jeden lub więcej mniej plików do projektu, a następnie konfigurując Gulp (lub Grunt), aby przetworzyć je w czasie kompilacji. Dodaj *style* folderu do projektu, a następnie dodaj nowy plik o nazwie Less *main.less* do tego folderu.

![Dodawanie pliku języka less](less-sass-fa/_static/add-less-file.png)

Po dodaniu, Twoja struktura folderów powinien wyglądać mniej więcej tak:

![Struktura folderów](less-sass-fa/_static/folder-structure.png)

Teraz możesz dodać niektóre podstawowe style do pliku, który będzie kompilowany na język CSS i wdrożony w folderze wwwroot przez Gulp.

Modyfikowanie *main.less* uwzględnienie następujących zawartość, która tworzy paletę kolorów prostego z pojedynczego podstawowego koloru.

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

`@base` a druga @-prefixed elementy są zmiennymi. Każdy z nich reprezentuje kolor. Z wyjątkiem `@base`, są ustawione przy użyciu funkcji kolorów: rozjaśnić ciemniejszy i pokrętła. Rozjaśnianie i ciemniejszy czy właściwie czego można oczekiwać; pokrętła ustawia odcień koloru przez liczbę stopni (około koło kolorów). Inteligentne zignorować zmiennych, które nie są używane, tak aby zademonstrować, jak działają te zmienne, należy wykonać je gdzieś jest mniej procesor. Klasy `.baseColor`, itp. zostaną przedstawione obliczone wartości poszczególnych zmiennych w pliku CSS, które są generowane.

### <a name="get-started"></a>Wprowadzenie

Tworzenie **plik konfiguracji programu npm** (*package.json*) w folderze projektu i edytuj go, aby odwoływać się do `gulp` i `gulp-less`:

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

Instalowanie zależności, albo w wierszu polecenia w folderze projektu lub w programie Visual Studio **Eksploratora rozwiązań** (**zależności > npm > Przywracanie pakietów**).

```console
npm install
```

![VS Przywracanie pakietów](less-sass-fa/_static/restore-packages.png)

W folderze projektu, należy utworzyć **Gulp pliku konfiguracyjnego** (*gulpfile.js*) do definiowania zautomatyzowanego procesu.  Dodaj zmienną w górnej części pliku do reprezentowania mniej, a mniej uruchomienie zadania:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Otwórz **Task Runner Explorer** (**Widok > inne Windows > Task Runner Explorer**). Wśród zadań, powinny zostać wyświetlone nowe zadanie o nazwie `less`. Może być konieczne odświeżenie okna.

Uruchom `less` zadania wyświetlone dane wyjściowe podobne do przedstawionego poniżej:

![mniej modułu uruchamiającego zadania](less-sass-fa/_static/less-task-runner.png)

*Wwwroot/css* folder zawiera teraz nowy plik *main.css*:

![css głównego tworzone](less-sass-fa/_static/main-css-created.png)

Otwórz *main.css* zobaczyć podobną do następującej:

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

Dodać prostą stronę HTML do *wwwroot* folder odwołania i *main.css* się palety kolorów w działaniu.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

Widać, że 180 stopni pokrętła na `@base` użyta do wyprodukowania `@background` spowodowało koło kolorów przeciwne kolor `@base`:

![przykład mniej testowania](less-sass-fa/_static/less-test-screenshot.png)

Mniej oferuje również obsługę zagnieżdżone reguły, a także zapytań zagnieżdżonej nośnika. Na przykład definiujące zagnieżdżone hierarchie, takich jak menu może spowodować pełne reguły CSS, takich jak te:

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

W idealnym wszystkich reguł stylów powiązane zostaną umieszczone razem w pliku CSS, ale w praktyce nie ma nic wymuszania tej reguły, z wyjątkiem Konwencji i prawdopodobnie komentarze bloku.

Definiowanie ma te same zasady przy użyciu mniej wygląda następująco:

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

Należy pamiętać, że w przypadku wszystkich elementów podrzędnych elementu `nav` znajdują się w swoim zakresie. Nie ma już wszystkie powtórzenia elementów nadrzędnych (`nav`, `li`, `a`), i liczba całkowita wiersza została obniżona również (chociaż niektóre jest wynikiem wprowadzanie wartości w tej samej linii w drugim przykładzie). Może być bardzo przydatne, organizacji, aby wyświetlić wszystkie reguły dla danego elementu interfejsu użytkownika w zakresie jawnie ograniczonego, w tym przypadku ustawione od pozostałej części pliku przez nawiasy klamrowe.

`&` Składnia jest mniej funkcji selektor & reprezentujący bieżący nadrzędny selektora. Tak, w ciągu {...} blok, `&` reprezentuje `a` znaczników i w związku z tym `&:link` jest odpowiednikiem `a:link`.

Zapytaniami multimediów, bardzo przydatne przy tworzeniu dynamiczne projekty mogą także uczestniczyć intensywnie powtórzeń i złożoność w kodzie CSS. Mniej umożliwia zapytania nośnika, aby być zagnieżdżony w klasach, dzięki czemu definicji całej klasy nie musi być powtarzany w ramach różnych najwyższego poziomu `@media` elementów. Na przykład Oto CSS menu dynamiczne:

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

Może to być lepiej określone w czasie krótszym jako:

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

Kolejną funkcją mniejszy, który już widzieliśmy jest obsługa operacje matematyczne, dzięki czemu atrybutów stylu, które mają zostać utworzone na podstawie wstępnie zdefiniowane zmienne. To sprawia, że aktualizacja style powiązane znacznie łatwiejsze, ponieważ zmienna podstawowy może być modyfikowany, a wszystkie wartości zależnego zmieniony automatycznie.

Pliki CSS, szczególnie w przypadku dużych witryn (a szczególnie w przypadku, gdy są używane zapytaniami multimediów), zwykle można pobrać dość duży wraz z upływem czasu wprowadzania pracę niewygodna. Mniejsze pliki mogą być definiowane osobno, następnie ściągane ze sobą przy użyciu `@import` dyrektywy. Mniej można również zaimportować poszczególne pliki CSS, jak również w razie potrzeby.

*Domieszki* zaakceptować parametrów i mniej obsługuje logikę warunkową w formie domieszki osłony, które zapewniają deklaratywną metodę, aby zdefiniować, kiedy niektórych domieszki zaczęły obowiązywać. Typowym zastosowaniem dla domieszki osłony jest aby dostosować kolory, w oparciu o sposób światła lub ciemny koloru źródłowego. Biorąc pod uwagę domieszki, który akceptuje parametr koloru, guard domieszki może służyć do modyfikowania domieszki na podstawie tego koloru:

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

Biorąc pod uwagę nasz bieżący `@base` wartość `#663333`, mniej skrypt generuje następujące arkusze CSS:

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

Mniej udostępnia szereg dodatkowych funkcji, ale to należy przedstawić możliwości to wstępne przetwarzanie języka.

## <a name="sass"></a>Sass

Sass jest podobny do niższej, świadczenie pomocy technicznej dla wielu tych samych funkcji, ale z nieco składnią. Stworzona przy użyciu języka Ruby, zamiast JavaScript i ma wymagania dotyczące różnych konfiguracji. Oryginalnym języku Sass został użyty przez Ciebie nawiasów klamrowych lub średników, ale zdefiniowany zakres przy użyciu biały znak i wcięcia. W wersji 3 Sass, wprowadzono nową składnię, **SCSS** ("Sassy CSS"). SCSS jest podobny do CSS, ponieważ ignoruje poziomów wcięć i odstępów i zamiast tego używa średnikami i nawiasów klamrowych.

Aby zainstalować Sass, zazwyczaj można będzie najpierw zainstaluj oprogramowanie Ruby (wstępnie zainstalowany w systemie macOS), a następnie uruchom:

```console
gem install sass
```

Jednak jeśli korzystasz z programu Visual Studio, możesz można wprowadzenie Sass w taki taki sam sposób jak w przypadku przy mniejszym. Otwórz *package.json* i Dodaj pakiet "gulp sass" `devDependencies`:

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

Następnie zmodyfikuj *gulpfile.js* pozwala dodać zmienną sass i zadania do kompilowania plików Sass i umieszcza wyniki w folderze wwwroot:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Teraz możesz dodać plik Sass *main2.scss* do *style* folderu w katalogu głównym projektu:

![Dodaj plik języka scss](less-sass-fa/_static/add-scss-file.png)

Otwórz *main2.scss* i Dodaj następujący kod:

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

Zapisz wszystkie pliki. Teraz, kiedy należy odświeżyć **Eksplorator modułu uruchamiającego zadania**, zostanie wyświetlony `sass` zadania. Uruchom go, a następnie sprawdź */wwwroot/css* folderu. Jest teraz *main2.css* pliku następującą zawartość:

```css
body {
    background-color: #CC0000;
}
```

Sass obsługuje zagnieżdżenia w taki, który był taki sam, że mniej nie, zapewniając korzyści podobnych. Pliki można podzielić przez funkcję i uwzględniony przy użyciu `@import` dyrektywy:

```sass
@import 'anotherfile';
```

Sass obsługuje także domieszki przy użyciu `@mixin` — słowo kluczowe, aby zdefiniować je i `@include` aby je uwzględnić, jak w poniższym przykładzie z [sass lang.com](http://sass-lang.com):

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

Oprócz domieszki Sass, również obsługuje dziedziczenia, dzięki czemu jedną klasę do innego rozszerzenia. Jest podobna do domieszki, ale wyniki w mniejszej ilości kodu CSS. Odbywa się przy użyciu `@extend` — słowo kluczowe. Aby wypróbować domieszki, Dodaj następujący kod do Twojego *main2.scss* pliku:

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Sprawdź dane wyjściowe w *main2.css* po uruchomieniu `sass` zadań w **Eksplorator modułu uruchamiającego zadania**:

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Należy zauważyć, że wszystkie typowe właściwości alertu domieszki powtarzają się w każdej klasie. Mixin — jak Dobra robota pomagania wyeliminować dublowanie w czasie programowania, ale nadal jest tworzenie CSS z dużą duplikatów, co większy niż niezbędne pliki CSS — potencjalny problem z wydajnością.

Teraz Zastąp alertu domieszki z `.alert` klasy i zmień wartość `@include` do `@extend` (zapamiętywanie rozszerzenie `.alert`, a nie `alert`):

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Ponownie uruchom Sass i zbadaj wynikowy arkusze CSS:

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Teraz właściwości są definiowane tylko jako dowolną liczbę razy, i lepiej jest generowany CSS.

Sass obejmuje również funkcje i operacje logikę warunkową, podobnie jak mniej. W rzeczywistości funkcje te dwa języki są bardzo podobne.

## <a name="less-or-sass"></a>Mniej lub Sass?

Nadal są nie zgodne, czy jest zazwyczaj lepiej jest mniejsze lub Sass, (lub nawet czy wolisz, oryginalnym Sass lub nowszej składni języka SCSS w ramach Sass). Prawdopodobnie jest najważniejsza decyzja **użyj jednej z tych narzędzi**, a nie po prostu programowania ręcznego pliki CSS. Po dokonaniu, decyzja, zarówno Less i Sass powinno się.

## <a name="font-awesome"></a>Font Awesome

Oprócz preprocesorami standardów CSS Font Awesome jest innym świetnym zasobem dla stylu nowoczesnych aplikacji sieci web. Font Awesome to pakiet narzędzi, który zawiera ponad 500 ikony wektorowe skalowalne, które mogą być swobodnie używane w aplikacji sieci web. Pierwotnie został zaprojektowany do pracy za pomocą narzędzi Bootstrap, ale go nie jest zależny na tej struktury lub żadnych bibliotek JavaScript.

Najprostszym sposobem, aby rozpocząć pracę z Font Awesome jest można dodać odwołania do niego przy użyciu lokalizacji publicznej dostarczania zawartości network (CDN):

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

Użytkownik może też dodać do projektu programu Visual Studio, dodając ją do "dependencies" w *bower.json*:

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

Po uzyskaniu odwołania do Font Awesome na stronie ikony można dodać do swojej aplikacji, stosując Font Awesome klasy, zazwyczaj prefiksem "fa-", do Twojego elementów śródwierszowych HTML (takie jak `<span>` lub `<i>`).  Na przykład można dodać ikony proste listy i menu przy użyciu kodu w następujący sposób:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

Pozwala to na utworzenie następujących w przeglądarce — należy pamiętać, ikony obok każdego elementu:

![Lista ikon](less-sass-fa/_static/list-icons-screenshot.png)

Możesz wyświetlić listę wszystkich dostępnych ikon:

http://fontawesome.io/icons/

## <a name="summary"></a>Podsumowanie

Nowoczesnych aplikacji sieci web wymaga coraz bardziej elastyczny, płynny projekty, które są czyste, intuicyjny i łatwo skorzystać przy użyciu różnych urządzeń. Zarządzanie złożonością arkuszy stylów CSS, wymagane, aby osiągnąć te cele najlepiej odbywa się przy użyciu notacji preprocesora mniej lub Sass. Ponadto zestaw narzędzi, takich jak Font Awesome szybkie dostarczanie dobrze znane ikony do menu nawigacji tekstowe i przyciski, poprawę ogólnej użytkownika środowisko aplikacji.

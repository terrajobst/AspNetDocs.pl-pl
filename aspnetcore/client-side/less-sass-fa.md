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
# <a name="less-sass-and-font-awesome-in-aspnet-core"></a><span data-ttu-id="5a29d-103">Mniej, Sass i Font Awesome w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5a29d-103">Less, Sass, and Font Awesome in ASP.NET Core</span></span>

<span data-ttu-id="5a29d-104">Przez [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="5a29d-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="5a29d-105">Użytkownicy aplikacji sieci web mieć zapewniał wysoką oczekiwania, jeśli chodzi o stylu i ogólne środowisko.</span><span class="sxs-lookup"><span data-stu-id="5a29d-105">Users of web applications have increasingly high expectations when it comes to style and overall experience.</span></span> <span data-ttu-id="5a29d-106">Nowoczesnych aplikacji sieci web często korzystać z zaawansowanych narzędzi i platform do definiowania i zarządzanie ich wygląd i działanie w spójny sposób.</span><span class="sxs-lookup"><span data-stu-id="5a29d-106">Modern web applications frequently leverage rich tools and frameworks for defining and managing their look and feel in a consistent manner.</span></span> <span data-ttu-id="5a29d-107">Struktury, takich jak [Bootstrap](http://getbootstrap.com/) przejść długą drogę w definiowaniu wspólny zbiór — style i opcje układu dla witryn sieci web.</span><span class="sxs-lookup"><span data-stu-id="5a29d-107">Frameworks like [Bootstrap](http://getbootstrap.com/) can go a long way toward defining a common set of styles and layout options for web sites.</span></span> <span data-ttu-id="5a29d-108">Jednak większość witryn nietrywialnymi również korzystać z można skutecznie zdefiniować oraz Obsługa stylów i plików kaskadowych (CSS) arkuszy stylów, a także łatwy dostęp do ikony-image, które sprawić, że interfejs witryny bardziej intuicyjne.</span><span class="sxs-lookup"><span data-stu-id="5a29d-108">However, most non-trivial sites also benefit from being able to effectively define and maintain styles and cascading style sheet (CSS) files, as well as having easy access to non-image icons that help make the site's interface more intuitive.</span></span> <span data-ttu-id="5a29d-109">To tutaj języki i narzędzia, które obsługują [mniej](http://lesscss.org/) i [Sass](http://sass-lang.com/), i bibliotek, takich jak [Font Awesome](http://fontawesome.io/), są dostępne w.</span><span class="sxs-lookup"><span data-stu-id="5a29d-109">That's where languages and tools that support [Less](http://lesscss.org/) and [Sass](http://sass-lang.com/), and libraries like [Font Awesome](http://fontawesome.io/), come in.</span></span>

## <a name="css-preprocessor-languages"></a><span data-ttu-id="5a29d-110">Języki preprocesora CSS</span><span class="sxs-lookup"><span data-stu-id="5a29d-110">CSS preprocessor languages</span></span>

<span data-ttu-id="5a29d-111">Języki, które są kompilowane do innych języków, aby ulepszyć środowisko pracy z językiem podstawowej są określane jako preprocesorami standardów.</span><span class="sxs-lookup"><span data-stu-id="5a29d-111">Languages that are compiled into other languages, in order to improve the experience of working with the underlying language, are referred to as preprocessors.</span></span> <span data-ttu-id="5a29d-112">Istnieją dwa popularne preprocesorami standardów CSS: Mniej i rozszerzenie Sass.</span><span class="sxs-lookup"><span data-stu-id="5a29d-112">There are two popular preprocessors for CSS: Less and Sass.</span></span>  <span data-ttu-id="5a29d-113">Te preprocesorami standardów dodać funkcje do arkusza CSS, takie jak obsługa zmiennych i zagnieżdżone reguły, które poprawić łatwość dużych, złożonych arkuszy stylów.</span><span class="sxs-lookup"><span data-stu-id="5a29d-113">These preprocessors add features to CSS, such as support for variables and nested rules, which improve the maintainability of large, complex stylesheets.</span></span> <span data-ttu-id="5a29d-114">CSS jako język jest bardzo proste, bez pomocy technicznej, nawet w przypadku coś tak proste, jak zmienne, a jest to prawdopodobnie, że pliki CSS powtarzających się i przeglądarek.</span><span class="sxs-lookup"><span data-stu-id="5a29d-114">CSS as a language is very basic, lacking support even for something as simple as variables, and this tends to make CSS files repetitive and bloated.</span></span> <span data-ttu-id="5a29d-115">Dodawanie funkcji języka rzeczywiste programowania za pomocą preprocesorami standardów może pomóc zmniejszyć dublowania i zapewnienia lepszej organizacji reguł stylów.</span><span class="sxs-lookup"><span data-stu-id="5a29d-115">Adding real programming language features via preprocessors can help reduce duplication and provide better organization of styling rules.</span></span> <span data-ttu-id="5a29d-116">Program Visual Studio udostępnia wbudowaną obsługę zarówno Less i Sass, jak również rozszerzeń, które można jeszcze bardziej poprawić środowisko programistyczne, pracując z tych języków.</span><span class="sxs-lookup"><span data-stu-id="5a29d-116">Visual Studio provides built-in support for both Less and Sass, as well as extensions that can further improve the development experience when working with these languages.</span></span>

<span data-ttu-id="5a29d-117">Jako prosty przykład jak preprocesorami standardów może zwiększyć czytelność i łatwość konserwacji informacje dotyczące stylu należy wziąć pod uwagę ten CSS:</span><span class="sxs-lookup"><span data-stu-id="5a29d-117">As a quick example of how preprocessors can improve readability and maintainability of style information, consider this CSS:</span></span>

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

<span data-ttu-id="5a29d-118">Za pomocą mniejsze, to może zostać przepisane, aby wyeliminować wszystkich duplikatów, za pomocą *domieszki* (o nazwie tak, ponieważ umożliwia on "mieszać w" właściwości z jednej klasy lub zestawu reguł do innego):</span><span class="sxs-lookup"><span data-stu-id="5a29d-118">Using Less, this can be rewritten to eliminate all of the duplication, using a *mixin* (so named because it allows you to "mix in" properties from one class or rule-set into another):</span></span>

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

## <a name="less"></a><span data-ttu-id="5a29d-119">less</span><span class="sxs-lookup"><span data-stu-id="5a29d-119">Less</span></span>

<span data-ttu-id="5a29d-120">Przebiegi preprocesora mniej CSS przy użyciu środowiska Node.js.</span><span class="sxs-lookup"><span data-stu-id="5a29d-120">The Less CSS preprocessor runs using Node.js.</span></span> <span data-ttu-id="5a29d-121">Aby zainstalować mniejsza, należy użyć Node Package Manager (npm) z wiersza polecenia (-g oznacza "global"):</span><span class="sxs-lookup"><span data-stu-id="5a29d-121">To install Less, use Node Package Manager (npm) from a command prompt (-g means "global"):</span></span>

```console
npm install -g less
```

<span data-ttu-id="5a29d-122">Jeśli używasz programu Visual Studio, możesz rozpocząć pracę za pomocą Less, dodając jeden lub więcej mniej plików do projektu, a następnie konfigurując Gulp (lub Grunt), aby przetworzyć je w czasie kompilacji.</span><span class="sxs-lookup"><span data-stu-id="5a29d-122">If you're using Visual Studio, you can get started with Less by adding one or more Less files to your project, and then configuring Gulp (or Grunt) to process them at compile-time.</span></span> <span data-ttu-id="5a29d-123">Dodaj *style* folderu do projektu, a następnie dodaj nowy plik o nazwie Less *main.less* do tego folderu.</span><span class="sxs-lookup"><span data-stu-id="5a29d-123">Add a *Styles* folder to your project, and then add a new Less file named *main.less* to this folder.</span></span>

![Dodawanie pliku języka less](less-sass-fa/_static/add-less-file.png)

<span data-ttu-id="5a29d-125">Po dodaniu, Twoja struktura folderów powinien wyglądać mniej więcej tak:</span><span class="sxs-lookup"><span data-stu-id="5a29d-125">Once added, your folder structure should look something like this:</span></span>

![Struktura folderów](less-sass-fa/_static/folder-structure.png)

<span data-ttu-id="5a29d-127">Teraz możesz dodać niektóre podstawowe style do pliku, który będzie kompilowany na język CSS i wdrożony w folderze wwwroot przez Gulp.</span><span class="sxs-lookup"><span data-stu-id="5a29d-127">Now you can add some basic styling to the file, which will be compiled into CSS and deployed to the wwwroot folder by Gulp.</span></span>

<span data-ttu-id="5a29d-128">Modyfikowanie *main.less* uwzględnienie następujących zawartość, która tworzy paletę kolorów prostego z pojedynczego podstawowego koloru.</span><span class="sxs-lookup"><span data-stu-id="5a29d-128">Modify *main.less* to include the following content, which creates a simple color palette from a single base color.</span></span>

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

<span data-ttu-id="5a29d-129">`@base` a druga @-prefixed elementy są zmiennymi.</span><span class="sxs-lookup"><span data-stu-id="5a29d-129">`@base` and the other @-prefixed items are variables.</span></span> <span data-ttu-id="5a29d-130">Każdy z nich reprezentuje kolor.</span><span class="sxs-lookup"><span data-stu-id="5a29d-130">Each of them represents a color.</span></span> <span data-ttu-id="5a29d-131">Z wyjątkiem `@base`, są ustawione przy użyciu funkcji kolorów: rozjaśnić ciemniejszy i pokrętła.</span><span class="sxs-lookup"><span data-stu-id="5a29d-131">Except for `@base`, they're set using color functions: lighten, darken, and spin.</span></span> <span data-ttu-id="5a29d-132">Rozjaśnianie i ciemniejszy czy właściwie czego można oczekiwać; pokrętła ustawia odcień koloru przez liczbę stopni (około koło kolorów).</span><span class="sxs-lookup"><span data-stu-id="5a29d-132">Lighten and darken do pretty much what you would expect; spin adjusts the hue of a color by a number of degrees (around the color wheel).</span></span> <span data-ttu-id="5a29d-133">Inteligentne zignorować zmiennych, które nie są używane, tak aby zademonstrować, jak działają te zmienne, należy wykonać je gdzieś jest mniej procesor.</span><span class="sxs-lookup"><span data-stu-id="5a29d-133">The Less processor is smart enough to ignore variables that aren't used, so to demonstrate how these variables work, we need to use them somewhere.</span></span> <span data-ttu-id="5a29d-134">Klasy `.baseColor`, itp. zostaną przedstawione obliczone wartości poszczególnych zmiennych w pliku CSS, które są generowane.</span><span class="sxs-lookup"><span data-stu-id="5a29d-134">The classes `.baseColor`, etc. will demonstrate the calculated values of each of the variables in the CSS file that's produced.</span></span>

### <a name="get-started"></a><span data-ttu-id="5a29d-135">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="5a29d-135">Get started</span></span>

<span data-ttu-id="5a29d-136">Tworzenie **plik konfiguracji programu npm** (*package.json*) w folderze projektu i edytuj go, aby odwoływać się do `gulp` i `gulp-less`:</span><span class="sxs-lookup"><span data-stu-id="5a29d-136">Create an **npm Configuration File** (*package.json*) in your project folder and edit it to reference `gulp` and `gulp-less`:</span></span>

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

<span data-ttu-id="5a29d-137">Instalowanie zależności, albo w wierszu polecenia w folderze projektu lub w programie Visual Studio **Eksploratora rozwiązań** (**zależności > npm > Przywracanie pakietów**).</span><span class="sxs-lookup"><span data-stu-id="5a29d-137">Install the dependencies either at a command prompt in your project folder, or in Visual Studio **Solution Explorer** (**Dependencies > npm > Restore packages**).</span></span>

```console
npm install
```

![VS Przywracanie pakietów](less-sass-fa/_static/restore-packages.png)

<span data-ttu-id="5a29d-139">W folderze projektu, należy utworzyć **Gulp pliku konfiguracyjnego** (*gulpfile.js*) do definiowania zautomatyzowanego procesu.</span><span class="sxs-lookup"><span data-stu-id="5a29d-139">In the project folder, create a **Gulp Configuration File** (*gulpfile.js*) to define the automated process.</span></span>  <span data-ttu-id="5a29d-140">Dodaj zmienną w górnej części pliku do reprezentowania mniej, a mniej uruchomienie zadania:</span><span class="sxs-lookup"><span data-stu-id="5a29d-140">Add a variable at the top of the file to represent Less, and a task to run Less:</span></span>

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

<span data-ttu-id="5a29d-141">Otwórz **Task Runner Explorer** (**Widok > inne Windows > Task Runner Explorer**).</span><span class="sxs-lookup"><span data-stu-id="5a29d-141">Open the **Task Runner Explorer** (**View > Other Windows > Task Runner Explorer**).</span></span> <span data-ttu-id="5a29d-142">Wśród zadań, powinny zostać wyświetlone nowe zadanie o nazwie `less`.</span><span class="sxs-lookup"><span data-stu-id="5a29d-142">Among the tasks, you should see a new task named `less`.</span></span> <span data-ttu-id="5a29d-143">Może być konieczne odświeżenie okna.</span><span class="sxs-lookup"><span data-stu-id="5a29d-143">You might have to refresh the window.</span></span>

<span data-ttu-id="5a29d-144">Uruchom `less` zadania wyświetlone dane wyjściowe podobne do przedstawionego poniżej:</span><span class="sxs-lookup"><span data-stu-id="5a29d-144">Run the `less` task, and you see output similar to what is shown here:</span></span>

![mniej modułu uruchamiającego zadania](less-sass-fa/_static/less-task-runner.png)

<span data-ttu-id="5a29d-146">*Wwwroot/css* folder zawiera teraz nowy plik *main.css*:</span><span class="sxs-lookup"><span data-stu-id="5a29d-146">The *wwwroot/css* folder now contains a new file, *main.css*:</span></span>

![css głównego tworzone](less-sass-fa/_static/main-css-created.png)

<span data-ttu-id="5a29d-148">Otwórz *main.css* zobaczyć podobną do następującej:</span><span class="sxs-lookup"><span data-stu-id="5a29d-148">Open *main.css* and you see something like the following:</span></span>

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

<span data-ttu-id="5a29d-149">Dodać prostą stronę HTML do *wwwroot* folder odwołania i *main.css* się palety kolorów w działaniu.</span><span class="sxs-lookup"><span data-stu-id="5a29d-149">Add a simple HTML page to the *wwwroot* folder, and reference *main.css* to see the color palette in action.</span></span>

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

<span data-ttu-id="5a29d-150">Widać, że 180 stopni pokrętła na `@base` użyta do wyprodukowania `@background` spowodowało koło kolorów przeciwne kolor `@base`:</span><span class="sxs-lookup"><span data-stu-id="5a29d-150">You can see that the 180 degree spin on `@base` used to produce `@background` resulted in the color wheel opposing color of `@base`:</span></span>

![przykład mniej testowania](less-sass-fa/_static/less-test-screenshot.png)

<span data-ttu-id="5a29d-152">Mniej oferuje również obsługę zagnieżdżone reguły, a także zapytań zagnieżdżonej nośnika.</span><span class="sxs-lookup"><span data-stu-id="5a29d-152">Less also provides support for nested rules, as well as nested media queries.</span></span> <span data-ttu-id="5a29d-153">Na przykład definiujące zagnieżdżone hierarchie, takich jak menu może spowodować pełne reguły CSS, takich jak te:</span><span class="sxs-lookup"><span data-stu-id="5a29d-153">For example, defining nested hierarchies like menus can result in verbose CSS rules like these:</span></span>

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

<span data-ttu-id="5a29d-154">W idealnym wszystkich reguł stylów powiązane zostaną umieszczone razem w pliku CSS, ale w praktyce nie ma nic wymuszania tej reguły, z wyjątkiem Konwencji i prawdopodobnie komentarze bloku.</span><span class="sxs-lookup"><span data-stu-id="5a29d-154">Ideally all of the related style rules will be placed together within the CSS file, but in practice there's nothing enforcing this rule except convention and perhaps block comments.</span></span>

<span data-ttu-id="5a29d-155">Definiowanie ma te same zasady przy użyciu mniej wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="5a29d-155">Defining these same rules using Less looks like this:</span></span>

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

<span data-ttu-id="5a29d-156">Należy pamiętać, że w przypadku wszystkich elementów podrzędnych elementu `nav` znajdują się w swoim zakresie.</span><span class="sxs-lookup"><span data-stu-id="5a29d-156">Note that in this case, all of the subordinate elements of `nav` are contained within its scope.</span></span> <span data-ttu-id="5a29d-157">Nie ma już wszystkie powtórzenia elementów nadrzędnych (`nav`, `li`, `a`), i liczba całkowita wiersza została obniżona również (chociaż niektóre jest wynikiem wprowadzanie wartości w tej samej linii w drugim przykładzie).</span><span class="sxs-lookup"><span data-stu-id="5a29d-157">There's no longer any repetition of parent elements (`nav`, `li`, `a`), and the total line count has dropped as well (though some of that's a result of putting values on the same lines in the second example).</span></span> <span data-ttu-id="5a29d-158">Może być bardzo przydatne, organizacji, aby wyświetlić wszystkie reguły dla danego elementu interfejsu użytkownika w zakresie jawnie ograniczonego, w tym przypadku ustawione od pozostałej części pliku przez nawiasy klamrowe.</span><span class="sxs-lookup"><span data-stu-id="5a29d-158">It can be very helpful, organizationally, to see all of the rules for a given UI element within an explicitly bounded scope, in this case set off from the rest of the file by curly braces.</span></span>

<span data-ttu-id="5a29d-159">`&` Składnia jest mniej funkcji selektor & reprezentujący bieżący nadrzędny selektora.</span><span class="sxs-lookup"><span data-stu-id="5a29d-159">The `&` syntax is a Less selector feature, with & representing the current selector parent.</span></span> <span data-ttu-id="5a29d-160">Tak, w ciągu {...}</span><span class="sxs-lookup"><span data-stu-id="5a29d-160">So, within the a {...}</span></span> <span data-ttu-id="5a29d-161">blok, `&` reprezentuje `a` znaczników i w związku z tym `&:link` jest odpowiednikiem `a:link`.</span><span class="sxs-lookup"><span data-stu-id="5a29d-161">block, `&` represents an `a` tag, and thus `&:link` is equivalent to `a:link`.</span></span>

<span data-ttu-id="5a29d-162">Zapytaniami multimediów, bardzo przydatne przy tworzeniu dynamiczne projekty mogą także uczestniczyć intensywnie powtórzeń i złożoność w kodzie CSS.</span><span class="sxs-lookup"><span data-stu-id="5a29d-162">Media queries, extremely useful in creating responsive designs, can also contribute heavily to repetition and complexity in CSS.</span></span> <span data-ttu-id="5a29d-163">Mniej umożliwia zapytania nośnika, aby być zagnieżdżony w klasach, dzięki czemu definicji całej klasy nie musi być powtarzany w ramach różnych najwyższego poziomu `@media` elementów.</span><span class="sxs-lookup"><span data-stu-id="5a29d-163">Less allows media queries to be nested within classes, so that the entire class definition doesn't need to be repeated within different top-level `@media` elements.</span></span> <span data-ttu-id="5a29d-164">Na przykład Oto CSS menu dynamiczne:</span><span class="sxs-lookup"><span data-stu-id="5a29d-164">For example, here is CSS for a responsive menu:</span></span>

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

<span data-ttu-id="5a29d-165">Może to być lepiej określone w czasie krótszym jako:</span><span class="sxs-lookup"><span data-stu-id="5a29d-165">This can be better defined in Less as:</span></span>

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

<span data-ttu-id="5a29d-166">Kolejną funkcją mniejszy, który już widzieliśmy jest obsługa operacje matematyczne, dzięki czemu atrybutów stylu, które mają zostać utworzone na podstawie wstępnie zdefiniowane zmienne.</span><span class="sxs-lookup"><span data-stu-id="5a29d-166">Another feature of Less that we have already seen is its support for mathematical operations, allowing style attributes to be constructed from pre-defined variables.</span></span> <span data-ttu-id="5a29d-167">To sprawia, że aktualizacja style powiązane znacznie łatwiejsze, ponieważ zmienna podstawowy może być modyfikowany, a wszystkie wartości zależnego zmieniony automatycznie.</span><span class="sxs-lookup"><span data-stu-id="5a29d-167">This makes updating related styles much easier, since the base variable can be modified and all dependent values change automatically.</span></span>

<span data-ttu-id="5a29d-168">Pliki CSS, szczególnie w przypadku dużych witryn (a szczególnie w przypadku, gdy są używane zapytaniami multimediów), zwykle można pobrać dość duży wraz z upływem czasu wprowadzania pracę niewygodna.</span><span class="sxs-lookup"><span data-stu-id="5a29d-168">CSS files, especially for large sites (and especially if media queries are being used), tend to get quite large over time, making working with them unwieldy.</span></span> <span data-ttu-id="5a29d-169">Mniejsze pliki mogą być definiowane osobno, następnie ściągane ze sobą przy użyciu `@import` dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="5a29d-169">Less files can be defined separately, then pulled together using `@import` directives.</span></span> <span data-ttu-id="5a29d-170">Mniej można również zaimportować poszczególne pliki CSS, jak również w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="5a29d-170">Less can also be used to import individual CSS files, as well, if desired.</span></span>

<span data-ttu-id="5a29d-171">*Domieszki* zaakceptować parametrów i mniej obsługuje logikę warunkową w formie domieszki osłony, które zapewniają deklaratywną metodę, aby zdefiniować, kiedy niektórych domieszki zaczęły obowiązywać.</span><span class="sxs-lookup"><span data-stu-id="5a29d-171">*Mixins* can accept parameters, and Less supports conditional logic in the form of mixin guards, which provide a declarative way to define when certain mixins take effect.</span></span> <span data-ttu-id="5a29d-172">Typowym zastosowaniem dla domieszki osłony jest aby dostosować kolory, w oparciu o sposób światła lub ciemny koloru źródłowego.</span><span class="sxs-lookup"><span data-stu-id="5a29d-172">A common use for mixin guards is to adjust colors based on how light or dark the source color is.</span></span> <span data-ttu-id="5a29d-173">Biorąc pod uwagę domieszki, który akceptuje parametr koloru, guard domieszki może służyć do modyfikowania domieszki na podstawie tego koloru:</span><span class="sxs-lookup"><span data-stu-id="5a29d-173">Given a mixin that accepts a parameter for color, a mixin guard can be used to modify the mixin based on that color:</span></span>

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

<span data-ttu-id="5a29d-174">Biorąc pod uwagę nasz bieżący `@base` wartość `#663333`, mniej skrypt generuje następujące arkusze CSS:</span><span class="sxs-lookup"><span data-stu-id="5a29d-174">Given our current `@base` value of `#663333`, this Less script will produce the following CSS:</span></span>

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

<span data-ttu-id="5a29d-175">Mniej udostępnia szereg dodatkowych funkcji, ale to należy przedstawić możliwości to wstępne przetwarzanie języka.</span><span class="sxs-lookup"><span data-stu-id="5a29d-175">Less provides a number of additional features, but this should give you some idea of the power of this preprocessing language.</span></span>

## <a name="sass"></a><span data-ttu-id="5a29d-176">Sass</span><span class="sxs-lookup"><span data-stu-id="5a29d-176">Sass</span></span>

<span data-ttu-id="5a29d-177">Sass jest podobny do niższej, świadczenie pomocy technicznej dla wielu tych samych funkcji, ale z nieco składnią.</span><span class="sxs-lookup"><span data-stu-id="5a29d-177">Sass is similar to Less, providing support for many of the same features, but with slightly different syntax.</span></span> <span data-ttu-id="5a29d-178">Stworzona przy użyciu języka Ruby, zamiast JavaScript i ma wymagania dotyczące różnych konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="5a29d-178">It's built using Ruby, rather than JavaScript, and so has different setup requirements.</span></span> <span data-ttu-id="5a29d-179">Oryginalnym języku Sass został użyty przez Ciebie nawiasów klamrowych lub średników, ale zdefiniowany zakres przy użyciu biały znak i wcięcia.</span><span class="sxs-lookup"><span data-stu-id="5a29d-179">The original Sass language didn't use curly braces or semicolons, but instead defined scope using white space and indentation.</span></span> <span data-ttu-id="5a29d-180">W wersji 3 Sass, wprowadzono nową składnię, **SCSS** ("Sassy CSS").</span><span class="sxs-lookup"><span data-stu-id="5a29d-180">In version 3 of Sass, a new syntax was introduced, **SCSS** ("Sassy CSS").</span></span> <span data-ttu-id="5a29d-181">SCSS jest podobny do CSS, ponieważ ignoruje poziomów wcięć i odstępów i zamiast tego używa średnikami i nawiasów klamrowych.</span><span class="sxs-lookup"><span data-stu-id="5a29d-181">SCSS is similar to CSS in that it ignores indentation levels and whitespace, and instead uses semicolons and curly braces.</span></span>

<span data-ttu-id="5a29d-182">Aby zainstalować Sass, zazwyczaj można będzie najpierw zainstaluj oprogramowanie Ruby (wstępnie zainstalowany w systemie macOS), a następnie uruchom:</span><span class="sxs-lookup"><span data-stu-id="5a29d-182">To install Sass, typically you would first install Ruby (pre-installed on macOS), and then run:</span></span>

```console
gem install sass
```

<span data-ttu-id="5a29d-183">Jednak jeśli korzystasz z programu Visual Studio, możesz można wprowadzenie Sass w taki taki sam sposób jak w przypadku przy mniejszym.</span><span class="sxs-lookup"><span data-stu-id="5a29d-183">However, if you're running Visual Studio, you can get started with Sass in much the same way as you would with Less.</span></span> <span data-ttu-id="5a29d-184">Otwórz *package.json* i Dodaj pakiet "gulp sass" `devDependencies`:</span><span class="sxs-lookup"><span data-stu-id="5a29d-184">Open *package.json* and add the "gulp-sass" package to `devDependencies`:</span></span>

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

<span data-ttu-id="5a29d-185">Następnie zmodyfikuj *gulpfile.js* pozwala dodać zmienną sass i zadania do kompilowania plików Sass i umieszcza wyniki w folderze wwwroot:</span><span class="sxs-lookup"><span data-stu-id="5a29d-185">Next, modify *gulpfile.js* to add a sass variable and a task to compile your Sass files and place the results in the wwwroot folder:</span></span>

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

<span data-ttu-id="5a29d-186">Teraz możesz dodać plik Sass *main2.scss* do *style* folderu w katalogu głównym projektu:</span><span class="sxs-lookup"><span data-stu-id="5a29d-186">Now you can add the Sass file *main2.scss* to the *Styles* folder in the root of the project:</span></span>

![Dodaj plik języka scss](less-sass-fa/_static/add-scss-file.png)

<span data-ttu-id="5a29d-188">Otwórz *main2.scss* i Dodaj następujący kod:</span><span class="sxs-lookup"><span data-stu-id="5a29d-188">Open *main2.scss* and add the following:</span></span>

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

<span data-ttu-id="5a29d-189">Zapisz wszystkie pliki.</span><span class="sxs-lookup"><span data-stu-id="5a29d-189">Save all of your files.</span></span> <span data-ttu-id="5a29d-190">Teraz, kiedy należy odświeżyć **Eksplorator modułu uruchamiającego zadania**, zostanie wyświetlony `sass` zadania.</span><span class="sxs-lookup"><span data-stu-id="5a29d-190">Now when you refresh **Task Runner Explorer**, you see a `sass` task.</span></span> <span data-ttu-id="5a29d-191">Uruchom go, a następnie sprawdź */wwwroot/css* folderu.</span><span class="sxs-lookup"><span data-stu-id="5a29d-191">Run it, and look in the */wwwroot/css* folder.</span></span> <span data-ttu-id="5a29d-192">Jest teraz *main2.css* pliku następującą zawartość:</span><span class="sxs-lookup"><span data-stu-id="5a29d-192">There's now a *main2.css* file, with these contents:</span></span>

```css
body {
    background-color: #CC0000;
}
```

<span data-ttu-id="5a29d-193">Sass obsługuje zagnieżdżenia w taki, który był taki sam, że mniej nie, zapewniając korzyści podobnych.</span><span class="sxs-lookup"><span data-stu-id="5a29d-193">Sass supports nesting in much the same was that Less does, providing similar benefits.</span></span> <span data-ttu-id="5a29d-194">Pliki można podzielić przez funkcję i uwzględniony przy użyciu `@import` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="5a29d-194">Files can be split up by function and included using the `@import` directive:</span></span>

```sass
@import 'anotherfile';
```

<span data-ttu-id="5a29d-195">Sass obsługuje także domieszki przy użyciu `@mixin` — słowo kluczowe, aby zdefiniować je i `@include` aby je uwzględnić, jak w poniższym przykładzie z [sass lang.com](http://sass-lang.com):</span><span class="sxs-lookup"><span data-stu-id="5a29d-195">Sass supports mixins as well, using the `@mixin` keyword to define them and `@include` to include them, as in this example from [sass-lang.com](http://sass-lang.com):</span></span>

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

<span data-ttu-id="5a29d-196">Oprócz domieszki Sass, również obsługuje dziedziczenia, dzięki czemu jedną klasę do innego rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="5a29d-196">In addition to mixins, Sass also supports the concept of inheritance, allowing one class to extend another.</span></span> <span data-ttu-id="5a29d-197">Jest podobna do domieszki, ale wyniki w mniejszej ilości kodu CSS.</span><span class="sxs-lookup"><span data-stu-id="5a29d-197">It's conceptually similar to a mixin, but results in less CSS code.</span></span> <span data-ttu-id="5a29d-198">Odbywa się przy użyciu `@extend` — słowo kluczowe.</span><span class="sxs-lookup"><span data-stu-id="5a29d-198">It's accomplished using the `@extend` keyword.</span></span> <span data-ttu-id="5a29d-199">Aby wypróbować domieszki, Dodaj następujący kod do Twojego *main2.scss* pliku:</span><span class="sxs-lookup"><span data-stu-id="5a29d-199">To try out mixins, add the following to your *main2.scss* file:</span></span>

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

<span data-ttu-id="5a29d-200">Sprawdź dane wyjściowe w *main2.css* po uruchomieniu `sass` zadań w **Eksplorator modułu uruchamiającego zadania**:</span><span class="sxs-lookup"><span data-stu-id="5a29d-200">Examine the output in *main2.css* after running the `sass` task in **Task Runner Explorer**:</span></span>

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

<span data-ttu-id="5a29d-201">Należy zauważyć, że wszystkie typowe właściwości alertu domieszki powtarzają się w każdej klasie.</span><span class="sxs-lookup"><span data-stu-id="5a29d-201">Notice that all of the common properties of the alert mixin are repeated in each class.</span></span> <span data-ttu-id="5a29d-202">Mixin — jak Dobra robota pomagania wyeliminować dublowanie w czasie programowania, ale nadal jest tworzenie CSS z dużą duplikatów, co większy niż niezbędne pliki CSS — potencjalny problem z wydajnością.</span><span class="sxs-lookup"><span data-stu-id="5a29d-202">The mixin did a good job of helping eliminate duplication at development time, but it's still creating CSS with a lot of duplication in it, resulting in larger than necessary CSS files - a potential performance issue.</span></span>

<span data-ttu-id="5a29d-203">Teraz Zastąp alertu domieszki z `.alert` klasy i zmień wartość `@include` do `@extend` (zapamiętywanie rozszerzenie `.alert`, a nie `alert`):</span><span class="sxs-lookup"><span data-stu-id="5a29d-203">Now replace the alert mixin with a `.alert` class, and change `@include` to `@extend` (remembering to extend `.alert`, not `alert`):</span></span>

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

<span data-ttu-id="5a29d-204">Ponownie uruchom Sass i zbadaj wynikowy arkusze CSS:</span><span class="sxs-lookup"><span data-stu-id="5a29d-204">Run Sass once more, and examine the resulting CSS:</span></span>

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

<span data-ttu-id="5a29d-205">Teraz właściwości są definiowane tylko jako dowolną liczbę razy, i lepiej jest generowany CSS.</span><span class="sxs-lookup"><span data-stu-id="5a29d-205">Now the properties are defined only as many times as needed, and better CSS is generated.</span></span>

<span data-ttu-id="5a29d-206">Sass obejmuje również funkcje i operacje logikę warunkową, podobnie jak mniej.</span><span class="sxs-lookup"><span data-stu-id="5a29d-206">Sass also includes functions and conditional logic operations, similar to Less.</span></span> <span data-ttu-id="5a29d-207">W rzeczywistości funkcje te dwa języki są bardzo podobne.</span><span class="sxs-lookup"><span data-stu-id="5a29d-207">In fact, the two languages' capabilities are very similar.</span></span>

## <a name="less-or-sass"></a><span data-ttu-id="5a29d-208">Mniej lub Sass?</span><span class="sxs-lookup"><span data-stu-id="5a29d-208">Less or Sass?</span></span>

<span data-ttu-id="5a29d-209">Nadal są nie zgodne, czy jest zazwyczaj lepiej jest mniejsze lub Sass, (lub nawet czy wolisz, oryginalnym Sass lub nowszej składni języka SCSS w ramach Sass).</span><span class="sxs-lookup"><span data-stu-id="5a29d-209">There's still no consensus as to whether it's generally better to use Less or Sass (or even whether to prefer the original Sass or the newer SCSS syntax within Sass).</span></span> <span data-ttu-id="5a29d-210">Prawdopodobnie jest najważniejsza decyzja **użyj jednej z tych narzędzi**, a nie po prostu programowania ręcznego pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="5a29d-210">Probably the most important decision is to **use one of these tools**, as opposed to just hand-coding your CSS files.</span></span> <span data-ttu-id="5a29d-211">Po dokonaniu, decyzja, zarówno Less i Sass powinno się.</span><span class="sxs-lookup"><span data-stu-id="5a29d-211">Once you've made that decision, both Less and Sass are good choices.</span></span>

## <a name="font-awesome"></a><span data-ttu-id="5a29d-212">Font Awesome</span><span class="sxs-lookup"><span data-stu-id="5a29d-212">Font Awesome</span></span>

<span data-ttu-id="5a29d-213">Oprócz preprocesorami standardów CSS Font Awesome jest innym świetnym zasobem dla stylu nowoczesnych aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="5a29d-213">In addition to CSS preprocessors, another great resource for styling modern web applications is Font Awesome.</span></span> <span data-ttu-id="5a29d-214">Font Awesome to pakiet narzędzi, który zawiera ponad 500 ikony wektorowe skalowalne, które mogą być swobodnie używane w aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="5a29d-214">Font Awesome is a toolkit that provides over 500 scalable vector icons that can be freely used in your web applications.</span></span> <span data-ttu-id="5a29d-215">Pierwotnie został zaprojektowany do pracy za pomocą narzędzi Bootstrap, ale go nie jest zależny na tej struktury lub żadnych bibliotek JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5a29d-215">It was originally designed to work with Bootstrap, but it has no dependency on that framework or on any JavaScript libraries.</span></span>

<span data-ttu-id="5a29d-216">Najprostszym sposobem, aby rozpocząć pracę z Font Awesome jest można dodać odwołania do niego przy użyciu lokalizacji publicznej dostarczania zawartości network (CDN):</span><span class="sxs-lookup"><span data-stu-id="5a29d-216">The easiest way to get started with Font Awesome is to add a reference to it, using its public content delivery network (CDN) location:</span></span>

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

<span data-ttu-id="5a29d-217">Użytkownik może też dodać do projektu programu Visual Studio, dodając ją do "dependencies" w *bower.json*:</span><span class="sxs-lookup"><span data-stu-id="5a29d-217">You can also add it to your Visual Studio project by adding it to the "dependencies" in *bower.json*:</span></span>

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

<span data-ttu-id="5a29d-218">Po uzyskaniu odwołania do Font Awesome na stronie ikony można dodać do swojej aplikacji, stosując Font Awesome klasy, zazwyczaj prefiksem "fa-", do Twojego elementów śródwierszowych HTML (takie jak `<span>` lub `<i>`).</span><span class="sxs-lookup"><span data-stu-id="5a29d-218">Once you have a reference to Font Awesome on a page, you can add icons to your application by applying Font Awesome classes, typically prefixed with "fa-", to your inline HTML elements (such as `<span>` or `<i>`).</span></span>  <span data-ttu-id="5a29d-219">Na przykład można dodać ikony proste listy i menu przy użyciu kodu w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="5a29d-219">For example, you can add icons to simple lists and menus using code like this:</span></span>

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

<span data-ttu-id="5a29d-220">Pozwala to na utworzenie następujących w przeglądarce — należy pamiętać, ikony obok każdego elementu:</span><span class="sxs-lookup"><span data-stu-id="5a29d-220">This produces the following in the browser - note the icon beside each item:</span></span>

![Lista ikon](less-sass-fa/_static/list-icons-screenshot.png)

<span data-ttu-id="5a29d-222">Możesz wyświetlić listę wszystkich dostępnych ikon:</span><span class="sxs-lookup"><span data-stu-id="5a29d-222">You can view a complete list of the available icons here:</span></span>

http://fontawesome.io/icons/

## <a name="summary"></a><span data-ttu-id="5a29d-223">Podsumowanie</span><span class="sxs-lookup"><span data-stu-id="5a29d-223">Summary</span></span>

<span data-ttu-id="5a29d-224">Nowoczesnych aplikacji sieci web wymaga coraz bardziej elastyczny, płynny projekty, które są czyste, intuicyjny i łatwo skorzystać przy użyciu różnych urządzeń.</span><span class="sxs-lookup"><span data-stu-id="5a29d-224">Modern web applications increasingly demand responsive, fluid designs that are clean, intuitive, and easy to use from a variety of devices.</span></span> <span data-ttu-id="5a29d-225">Zarządzanie złożonością arkuszy stylów CSS, wymagane, aby osiągnąć te cele najlepiej odbywa się przy użyciu notacji preprocesora mniej lub Sass.</span><span class="sxs-lookup"><span data-stu-id="5a29d-225">Managing the complexity of the CSS stylesheets required to achieve these goals is best done using a preprocessor like Less or Sass.</span></span> <span data-ttu-id="5a29d-226">Ponadto zestaw narzędzi, takich jak Font Awesome szybkie dostarczanie dobrze znane ikony do menu nawigacji tekstowe i przyciski, poprawę ogólnej użytkownika środowisko aplikacji.</span><span class="sxs-lookup"><span data-stu-id="5a29d-226">In addition, toolkits like Font Awesome quickly provide well-known icons to textual navigation menus and buttons, improving the overall user experience of your application.</span></span>

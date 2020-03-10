---
uid: whitepapers/request-validation
title: Żądanie weryfikacji — zapobieganie atakom skryptu | Microsoft Docs
author: rick-anderson
description: W tym dokumencie opisano funkcję walidacji żądania ASP.NET, gdzie Domyślnie aplikacja uniemożliwia przetwarzanie niekodowanej zawartości HTML...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 807cccd6fe1acdd6359b014387abd3878840d4cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640848"
---
# <a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="13b4a-103">Żądanie walidacji — zapobieganie atakom za pomocą skryptów</span><span class="sxs-lookup"><span data-stu-id="13b4a-103">Request Validation - Preventing Script Attacks</span></span>

> <span data-ttu-id="13b4a-104">W tym dokumencie opisano funkcję walidacji żądania ASP.NET, gdzie Domyślnie aplikacja nie może przetwarzać niekodowanej zawartości HTML przesłanej do serwera.</span><span class="sxs-lookup"><span data-stu-id="13b4a-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="13b4a-105">Tę funkcję sprawdzania poprawności żądania można wyłączyć, gdy aplikacja została zaprojektowana w celu bezpiecznego przetworzenia danych HTML.</span><span class="sxs-lookup"><span data-stu-id="13b4a-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="13b4a-106">Dotyczy ASP.NET 1,1 i ASP.NET 2,0.</span><span class="sxs-lookup"><span data-stu-id="13b4a-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>

<span data-ttu-id="13b4a-107">Zażądaj zweryfikowania funkcji ASP.NET od wersji 1,1, uniemożliwiając serwerowi akceptowanie zawartości zawierającej niezakodowany kod HTML.</span><span class="sxs-lookup"><span data-stu-id="13b4a-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="13b4a-108">Ta funkcja została zaprojektowana w taki sposób, aby zapobiec atakom polegającym na wstrzyknięciu skryptów, zgodnie z którymi kod skryptu klienta lub HTML może być nieświadomie przesłany do serwera, przechowywany i przedstawiony innym użytkownikom.</span><span class="sxs-lookup"><span data-stu-id="13b4a-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="13b4a-109">Zdecydowanie zalecamy, aby sprawdzać poprawność wszystkich danych wejściowych i kodu HTML w razie potrzeby.</span><span class="sxs-lookup"><span data-stu-id="13b4a-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="13b4a-110">Można na przykład utworzyć stronę sieci Web, która żąda adresu e-mail użytkownika, a następnie przechowywać ten adres e-mail w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="13b4a-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="13b4a-111">Jeśli użytkownik wprowadzi &lt;skrypt&gt;alertu ("Hello from Script")&lt;/SCRIPT&gt; zamiast poprawnego adresu e-mail, po wyświetleniu tych danych ten skrypt można wykonać, jeśli zawartość nie została prawidłowo zaszyfrowana.</span><span class="sxs-lookup"><span data-stu-id="13b4a-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="13b4a-112">Funkcja walidacji żądania ASP.NET zapobiega tym samym.</span><span class="sxs-lookup"><span data-stu-id="13b4a-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="13b4a-113">Dlaczego ta funkcja jest przydatna</span><span class="sxs-lookup"><span data-stu-id="13b4a-113">Why this feature is useful</span></span>

<span data-ttu-id="13b4a-114">W wielu lokacjach nie ma informacji, że są one otwarte dla prostych ataków na wstrzyknięcie skryptu.</span><span class="sxs-lookup"><span data-stu-id="13b4a-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="13b4a-115">Bez względu na to, czy celem tego ataku jest odtworzenie witryny przez wyświetlanie kodu HTML lub potencjalnie wykonanie skryptu klienta w celu przekierowania użytkownika do lokacji hakera, ataki przed iniekcją skryptów są problemem, który deweloperzy sieci Web muszą będą konkurować o.</span><span class="sxs-lookup"><span data-stu-id="13b4a-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="13b4a-116">Ataki z iniekcją skryptów to zainteresowania wszystkich deweloperów sieci Web, niezależnie od tego, czy korzystają z ASP.NET, ASP, czy innych technologii programistycznych dla sieci Web.</span><span class="sxs-lookup"><span data-stu-id="13b4a-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="13b4a-117">Funkcja walidacji żądań ASP.NET pozwala aktywnie zapobiegać atakom, nie zezwalając na przetworzenie niezakodowanej zawartości HTML na serwerze, chyba że deweloper zdecyduje się zezwolić na tę zawartość.</span><span class="sxs-lookup"><span data-stu-id="13b4a-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="13b4a-118">Oczekiwane: Strona błędu</span><span class="sxs-lookup"><span data-stu-id="13b4a-118">What to expect: Error Page</span></span>

<span data-ttu-id="13b4a-119">Poniższy zrzut ekranu przedstawia przykładowy kod ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="13b4a-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="13b4a-120">Uruchomienie tego kodu powoduje prostą stronę, która umożliwia wprowadzanie tekstu w polu tekstowym, a następnie kliknięcie tego przycisku i wyświetlenie tekstu w kontrolce etykieta:</span><span class="sxs-lookup"><span data-stu-id="13b4a-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="13b4a-121">Jednak były to skrypty JavaScript, takie jak `<script>alert("hello!")</script>`, które mają zostać wprowadzone i przesłane, możemy uzyskać wyjątek:</span><span class="sxs-lookup"><span data-stu-id="13b4a-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="13b4a-122">Komunikat o błędzie stwierdza, że wykryto wartość "potencjalnie niebezpieczne żądanie. formularz" i zawiera więcej szczegółów w opisie, jak dokładnie to, co się stało, i jak zmienić zachowanie.</span><span class="sxs-lookup"><span data-stu-id="13b4a-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="13b4a-123">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="13b4a-123">For example:</span></span>

<span data-ttu-id="13b4a-124">Podczas sprawdzania poprawności żądania wykryto potencjalnie niebezpieczną wartość wejściową klienta i przetwarzanie żądania zostało przerwane.</span><span class="sxs-lookup"><span data-stu-id="13b4a-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="13b4a-125">Ta wartość może wskazywać na próbę złamania zabezpieczeń aplikacji, na przykład atak skryptowy między lokacjami.</span><span class="sxs-lookup"><span data-stu-id="13b4a-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="13b4a-126">Sprawdzanie poprawności żądania można wyłączyć, ustawiając `validateRequest=false` w dyrektywie page lub w sekcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="13b4a-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="13b4a-127">Jednak zdecydowanie zaleca się, aby aplikacja jawnie sprawdzał wszystkie dane wejściowe w tym przypadku.</span><span class="sxs-lookup"><span data-stu-id="13b4a-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="13b4a-128">Wyłączanie weryfikacji żądań na stronie</span><span class="sxs-lookup"><span data-stu-id="13b4a-128">Disabling request validation on a page</span></span>

<span data-ttu-id="13b4a-129">Aby wyłączyć weryfikację żądań na stronie, należy ustawić atrybut `validateRequest` dyrektywy Page na `false`:</span><span class="sxs-lookup"><span data-stu-id="13b4a-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="13b4a-130">Po wyłączeniu walidacji żądania można przesłać zawartość na stronę; Projektant strony jest odpowiedzialny za zapewnienie, że zawartość jest prawidłowo zaszyfrowana lub przetworzona.</span><span class="sxs-lookup"><span data-stu-id="13b4a-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="13b4a-131">Wyłączanie weryfikacji żądań dla aplikacji</span><span class="sxs-lookup"><span data-stu-id="13b4a-131">Disabling request validation for your application</span></span>

<span data-ttu-id="13b4a-132">Aby wyłączyć weryfikację żądań dla aplikacji, należy zmodyfikować lub utworzyć plik Web. config dla aplikacji i ustawić atrybut validateRequest sekcji `<pages />`, aby `false`:</span><span class="sxs-lookup"><span data-stu-id="13b4a-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="13b4a-133">Jeśli chcesz wyłączyć weryfikację żądań dla wszystkich aplikacji na serwerze, możesz wprowadzić tę modyfikację do pliku Machine. config.</span><span class="sxs-lookup"><span data-stu-id="13b4a-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="13b4a-134">Gdy weryfikacja żądania jest wyłączona, zawartość może zostać przesłana do aplikacji; Deweloper aplikacji jest odpowiedzialny za zapewnienie, że zawartość jest prawidłowo zakodowana lub przetwarzana.</span><span class="sxs-lookup"><span data-stu-id="13b4a-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="13b4a-135">Poniższy kod został zmodyfikowany, aby wyłączyć weryfikację żądania:</span><span class="sxs-lookup"><span data-stu-id="13b4a-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="13b4a-136">Teraz, jeśli Poniższy kod JavaScript został wprowadzony do pola tekstowego `<script>alert("hello!")</script>` wynik:</span><span class="sxs-lookup"><span data-stu-id="13b4a-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="13b4a-137">Aby temu zapobiec, z wyłączeniem walidacji żądania, musimy zakodować zawartość HTML.</span><span class="sxs-lookup"><span data-stu-id="13b4a-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="13b4a-138">Jak kodować zawartość HTML</span><span class="sxs-lookup"><span data-stu-id="13b4a-138">How to HTML encode content</span></span>

<span data-ttu-id="13b4a-139">Jeśli wyłączono weryfikację żądań, dobrym sposobem jest kodowanie HTML zawartości, która będzie przechowywana do użytku w przyszłości.</span><span class="sxs-lookup"><span data-stu-id="13b4a-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="13b4a-140">Kodowanie HTML automatycznie zastępuje wszystkie "&lt;" lub "&gt;" (wraz z innymi innymi symbolami) z odpowiadającą im reprezentacją zakodowaną w kodzie HTML.</span><span class="sxs-lookup"><span data-stu-id="13b4a-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="13b4a-141">Na przykład element "&lt;" jest zastępowany przez element "&amp;lt;" i "&gt;" został zastąpiony przez "&amp;gt;".</span><span class="sxs-lookup"><span data-stu-id="13b4a-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="13b4a-142">Przeglądarki używają tych specjalnych kodów do wyświetlania "&lt;" lub "&gt;" w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="13b4a-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="13b4a-143">Zawartość można łatwo zakodować HTML na serwerze za pomocą interfejsu API `Server.HtmlEncode(string)`.</span><span class="sxs-lookup"><span data-stu-id="13b4a-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="13b4a-144">Zawartość można również łatwo zdekodować HTML, czyli przywrócić ją z powrotem do standardowego kodu HTML przy użyciu metody `Server.HtmlDecode(string)`.</span><span class="sxs-lookup"><span data-stu-id="13b4a-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="13b4a-145">Wyniki:</span><span class="sxs-lookup"><span data-stu-id="13b4a-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)

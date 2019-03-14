---
ms.openlocfilehash: 7eb0f9b556f0663a5208310d2d4864eca28f8632
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57075866"
---
# <a name="contributing-to-the-jquery-validation-plugin"></a>Współtworzenie jQuery wtyczki sprawdzania poprawności

## <a name="reporting-an-issue"></a>Zgłoszenie problemu

1. Upewnij się, że problem, którego jesteś adresowania zostanie odtworzony.
2. Użyj https://jsbin.com lub https://jsfiddle.net zapewnienie strony testowej.
3. Wskazują, jakie przeglądarki, problem może być odtworzony w. **Uwaga: Problemy z trybu programu Internet Explorer programu Access nie zostanie rozwiązany. Upewnij się, że testowanie w przeglądarce rzeczywistych!**
4. Jakie wersje dodatku typu plug-in jest problem do odtworzenia w. Czy jest do odtworzenia po aktualizacji do najnowszej wersji.

Problemy z dokumentacji również są śledzone w [jQuery weryfikacji](https://github.com/jquery-validation/jquery-validation/issues) narzędzie do śledzenia problemów.
Żądania ściągnięcia, aby poprawić dokumentację są Zapraszamy w [docs weryfikacji jQuery](https://github.com/jquery-validation/validation-content) repozytorium, chociaż.

**WAŻNA UWAGA DOTYCZĄCA WERYFIKACJI WIADOMOŚCI E-MAIL**. Począwszy od wersji 1.12.0 Ta wtyczka jest przy użyciu tego samego wyrażenia regularnego, [specyfikacji HTML5 sugeruje dla przeglądarek użyć](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). Firma Microsoft ich kierownictwem i używać tego samego sprawdzenia. Jeśli uważasz, że specyfikację jest nieprawidłowy, zgłoś go do nich. Jeśli masz inne wymagania, należy wziąć pod uwagę [niestandardowej metody](http://jqueryvalidation.org/jQuery.validator.addMethod/).
W przypadku, gdy wymagane jest dostosowanie weryfikacji wbudowane wzorce wyrażeń regularnych, prosimy [zgodnie z dokumentacją](http://jqueryvalidation.org/jQuery.validator.methods/).

## <a name="contributing-code"></a>Współtworzenia

Dziękujemy za udział w projektach! Poniżej przedstawiono kilka wskazówek, aby ułatwić Twojego wkładu, Pobierz teraz.

1. Upewnij się, że problem, którego jesteś adresowania zostanie odtworzony. Użyj jsbin.com lub jsfiddle.net, aby dostarczyć strony testowej.
2. Postępuj zgodnie z [przewodnik stylistyczny jQuery](http://contribute.jquery.com/style-guides/js)
3. Dodaj lub zaktualizuj testy jednostkowe oraz swoje poprawki. Uruchom testy jednostkowe w co najmniej jednej przeglądarki (patrz poniżej).
4. Uruchom `grunt` (zobacz poniżej), aby sprawdzić Zaznaczanie błędów i kilka innych problemów.
5. Opisano zmiany w wiadomości zatwierdzenia i odwoływać się--ticket, następująco: "Pokazy: Naprawiono delegata usterki pokaz dynamiczne sumy. Poprawki #51 ". W przypadku dodawania nowego pliku w lokalizacji, należy użyć podobny do poniższego: "Lokalizacja: Lokalizacja dodano Chorwacki (HR)"

## <a name="build-setup"></a>Tworzenie Instalatora

1. Zainstaluj [NodeJS](http://nodejs.org).
2. Zainstaluj Grunt interfejsu wiersza polecenia do instalacji uruchamiając `npm install -g grunt-cli`. Więcej informacji jest dostępnych na stronie internetowej http://gruntjs.com/getting-started.
3. Zainstaluj zależności rozwiązania NPM, uruchamiając `npm install`.
4. Teraz można wywołać kompilacji, uruchamiając `grunt`.

## <a name="creating-a-new-additional-method"></a>Tworzenie nowej metody dodatkowe

Jeśli został napisany niestandardowych metod, które możesz współtworzyć dodatkowe-methods.js:

1. Tworzenie gałęzi
2. Dodaj metodę jako nowy plik w `src/additional`
3. (Opcjonalnie) Dodawanie tłumaczenia `src/localization`
4. Wyślij żądanie pobierania do głównej gałęzi.

## <a name="unit-tests"></a>Testy jednostkowe

Aby uruchomić testy jednostkowe, po prostu otwórz `test/index.html` w przeglądarce. Upewnij się, że uruchomieniu `npm install` przed więc wszystkie wymagane zależności są dostępne.
Uruchom przy użyciu jednej przeglądarki podczas tworzenia poprawki, a następnie uruchamiać inne przed zatwierdzeniem. Zazwyczaj najnowsza Chrome, Firefox, Safari i Opera i kilka właściwości.

## <a name="documentation"></a>Dokumentacja

Zgłoś problemy z dokumentacji [jQuery weryfikacji](https://github.com/jquery-validation/jquery-validation/issues) narzędzie do śledzenia problemów.
W przypadku żądania ściągnięcia implementuje lub zmiany publicznego interfejsu API byłoby plus zapewni żądanie ściągnięcia względem [docs weryfikacji jQuery](https://github.com/jquery-validation/validation-content) repozytorium.

## <a name="linting"></a>Zaznaczanie błędów

Aby uruchomić JSHint i inne narzędzia, należy użyć `grunt`.

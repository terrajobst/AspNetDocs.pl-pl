---
ms.openlocfilehash: 170d5ec71b0789bc1efb43fbdd4e24eab36a8cc4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57073439"
---
<a name="--"></a>--
==================

## <a name="core"></a>Core
  * Usuń nieużywane removeAttrs — metoda
  * Zastąp wyrażenie regularne dla adresu url, metoda
  * Usuwanie nieprawidłowych parametrów adresu url w .ajax $, zastąpiona przez $.extend
  * Poprawnie obsługiwać przycisk Zatwierdź anulowanie zagnieżdżonych
  * Usuń wcięcie
  * Refaktoryzuj attributeRules i dataRules udostępnianie noramlizer
  * Metoda dataRules można przekonwertować wartości na liczbę liczba danych wejściowych
  * Zaktualizuj metodę adres url umożliwiający protokołu względnych adresów URL
  * Usuń przestarzałe symbolu zastępczego .format $
  * Użyj jQuery 1.7 lub nowszy, włączenia/wyłączenia, Dodaj destroy — metoda
  * Zgodność programu IE 8 zmieniony .indexOf na $.inArray
  * Rzutowanie atrybuty wartości NaN do niezdefiniowanego Opera Mini
  * Zatrzymaj wartość przycinania w wymaganej metody
  * Użycie: wyłączone selektora, aby dopasować elementy wyłączone
  * Wyklucz niektórych klawiszy, aby uniknąć ponownego sprawdzania poprawności nazwy pola
  * Nie wyszukuj całego modelu DOM, dla opcji/pola wyboru elementów
  * Throw lepsze błędów dla metody zły reguły
  * Naprawiono błąd sprawdzania poprawności numeru
  * Usuń odwołanie do specyfikacji whatwg
  * Nieprawidłowy element fokus podczas sprawdzania poprawności niestandardowy zestaw danych wejściowych
  * Resetuj element style, gdy za pomocą metod niestandardowego wyróżnienia
  * Znak dolara ucieczki w identyfikator błędu
  * Przywróć "tylko do odczytu Ignoruj także pola wyłączone."
  * Łącze Aktualizuj dostępne w komentarzu dla algorytmu Luhna

## <a name="additionals"></a>Additionals
  * Aktualizuj dateITA adres strefy czasowej problemu
  * Usuń metodę rozszerzenia, aby tylko okres — metoda
  * Poprawka zaakceptować metodę, aby dopasować tylko okres
  * Zaktualizuj metodę czasu umożliwia godzinę pojedyncza cyfra
  * Zły test wytrzymałości notEqualTo metody
  * Dodaj metodę notEqualTo
  * Użyj jQuery poprawne odwołania za pośrednictwem `$`
  * Usuń wyboru bezcelowe wyrażeń regularnych w metodzie iban
  * Numer CPF brazylijski

## <a name="localization"></a>Lokalizacja
  * Aktualizuj messages_tr.js
  * Update messages_sr_lat.js
  * Dodawanie Perú hiszpański (ES PE)
  * Dodawanie Gruziński (ქართული, ge)
  * Naprawiono błąd pisowni w tłumaczeniu Kataloński
  * Poprawa tłumaczenia fiński (fi)
  * Dodawanie ustawień regionalnych Ormiański (hy_AM)
  * Rozszerzanie tłumaczenia włoski (on) przy użyciu metody waluty
  * Dodawanie ustawień regionalnych bn_BD
  * Aktualizowanie ustawień regionalnych zh
  * Usuń kropką na końcu włoski wiadomości

<a name="1131--2014-10-14"></a>1.13.1 / 2014-10-14
==================

## <a name="core"></a>Core
  * Zezwalaj na 0 jako wartość dla autoCreateRanges
  * Zastosuj Ignoruj ustawienie, aby wszystkie elementy validationTargetFor
  * Nie przycinaj wartość minimalna/maksymalna/rangelength metody
  * Znak ucieczki identyfikator i nazwa, zanim użyjesz go jako selektora w errorsFor
  * Jawne Tworzenie domyślnych opcji focusCleanup
  * Poprawka niepoprawne regexp describedby dopasowywania
  * Ignoruj tylko do odczytu, a także pola wyłączone
  * Poprawić anulowania zapewnianego element id, przechowywania identyfikator o zmienionym znaczeniu w describedby
  * Użyj wartości zwracanej przez submitHandler aby umożliwić lub uniemożliwić przesyłania formularza

## <a name="additionals"></a>Additionals
  * Dodaj metodę postalcodeBR
  * Naprawiono metody wzorca, gdy jest ciągiem


<a name="1130--2014-07-01"></a>1.13.0 / 2014-07-01
==================

## <a name="all"></a>Wszystkie
* Dodaj wtyczkę UMD otoki

## <a name="core"></a>Core
* Przestrzeganie — błąd aria-describedby i puste błędy ukryte
* Poprawa dateISO RegExp
* Dodano opcji/pole wyboru, aby delegować Zdarzenie kliknięcia
* Użyj aria describedby-label elementów
* Rejestrowanie focusin, focusout i keyup również w opcji/pola wyboru
* Napraw normalizacji rangelength wartości atrybutu
* Zaktualizuj metodę elementValue radzenia sobie z typu = "number" pól
* Użyj charAt — zamiast notacji tablicę ciągów, do obsługi IE8(?)

## <a name="localization"></a>Lokalizacja
* Napraw sk tłumaczenia rangelength — metoda
* Dodaj metody fiński
* Naprawiono komunikat sprawdzania poprawności GL
* Naprawiono ES numer komunikatu weryfikacji — metoda
* Dodano Galicyjski (GL)
* Naprawiono francuska wiadomości dla metod, min i max

## <a name="additionals"></a>Additionals
* Dodaj metodę statesUS
* Napraw dateITA metody radzenia sobie z błędu czasu letniego
* Dodaj metodę perski daty
* Dodaj metodę postalCodeCA
* Dodaj metodę postalcodeIT

<a name="1120--2014-04-01"></a>1.12.0 / 2014-04-01
==================

* Dodaj testy ARIA ([3d5658e](https://github.com/jzaefferer/jquery-validation/commit/3d5658e9e4825fab27198c256beed86f0bd12577))
* Dodaj es AR lokalizacji wiadomości. ([7b30beb](https://github.com/jzaefferer/jquery-validation/commit/7b30beb8ebd218c38a55d26a63d529e16035c7a2))
* Dodaj brakujące kropki "es" i "es_AR" wiadomości. ([a2a653c](https://github.com/jzaefferer/jquery-validation/commit/a2a653cb68926ca034b4b09d742d275db934d040))
* Dodano lokalizacji indonezyjski (ID) ([1d348bd](https://github.com/jzaefferer/jquery-validation/commit/1d348bdcb65807c71da8d0bfc13a97663631cd3a))
* Dodane przy użyciu interfejsu Sieciowego, NIE i hiszpańskiego CIF weryfikacji liczby dokumentów ([#830](https://github.com/jzaefferer/jquery-validation/issues/830), [317c20f](https://github.com/jzaefferer/jquery-validation/commit/317c20fa9bb772770bb9b70d46c7081d7cfc6545))
* Dodano bieżącego formularza do kontekstu zdalne żądanie ajax ([0a18ae6](https://github.com/jzaefferer/jquery-validation/commit/0a18ae65b9b6d877e3d15650a5c2617a9d2b11d5))
* Additionals: Zaktualizuj metodę IBAN przycinania, spacji końcowych ([#970](https://github.com/jzaefferer/jquery-validation/issues/970), [347b04a](https://github.com/jzaefferer/jquery-validation/commit/347b04a7d4e798227405246a5de3fc57451d52e1))
* Metoda kod identyfikacyjny: Poprawa wyrażeń regularnych, {1} zawsze jest nadmiarowe. Zamyka gh 744 ([5cad6b4](https://github.com/jzaefferer/jquery-validation/commit/5cad6b493575e8a9a82470d17e0900c881130873))
* Program bower: Dodawanie pliku Bower.json dla rejestracji pakietu ([e86ccb0](https://github.com/jzaefferer/jquery-validation/commit/e86ccb06e301613172d472cf15dd4011ff71b398))
* Zmienia Dolar odwołania do "jQuery" dla zgodności z jQuery.noConflict. Zamyka gh-754 ([2049afe](https://github.com/jzaefferer/jquery-validation/commit/2049afe46c1be7b3b89b1d9f0690f5bebf4fbf68))
* podstawowe: Dodaj do wpisu listy błędów pole "method" ([89a15c7](https://github.com/jzaefferer/jquery-validation/commit/89a15c7a4b17fa2caaf4ff817f09b04c094c3884))
* podstawowe: Dodano obsługę dla ogólnych komunikatów za pomocą atrybutu danych msg ([5bebaa5](https://github.com/jzaefferer/jquery-validation/commit/5bebaa5c55c73f457c0e0181ec4e3b0c409e2a9d))
* podstawowe: Zezwalaj na atrybutów, które mają mieć wartość zero (na przykład min = "0") ([#854](https://github.com/jzaefferer/jquery-validation/issues/854), [9dc0d1d](https://github.com/jzaefferer/jquery-validation/commit/9dc0d1dd946b2c6178991fb16df0223c76162579))
* podstawowe: Wyłącz przestarzałe $.format ([#755](https://github.com/jzaefferer/jquery-validation/issues/755), [bf3b350](https://github.com/jzaefferer/jquery-validation/commit/bf3b3509140ea8ab5d83d3ec58fd9f1d7822efc5))
* podstawowe: Obsługa wielu klas błąd naprawić ([c1f0baf](https://github.com/jzaefferer/jquery-validation/commit/c1f0baf36c21ca175bbc05fb9345e5b44b094821))
* podstawowe: Ignoruj zdarzenia w przypadku elementów ignorowanych ([#700](https://github.com/jzaefferer/jquery-validation/issues/700), [a864211](https://github.com/jzaefferer/jquery-validation/commit/a86421131ea69786ee9e0d23a68a54a7658ccdbf))
* podstawowe: Poprawa elementValue — metoda ([6c041ed](https://github.com/jzaefferer/jquery-validation/commit/6c041edd21af1425d12d06cdd1e6e32a78263e82))
* podstawowe: Zapewnić prawidłowe element() uchwyt zignorowane elementy. ([3f464a8](https://github.com/jzaefferer/jquery-validation/commit/3f464a8da49dbb0e4881ada04165668e4a63cecb))
* podstawowe: Przełącz dataRules do specyfikacji styl W3C HTML5 ([460fd22](https://github.com/jzaefferer/jquery-validation/commit/460fd22b6c84a74c825ce1fa860c0a9da20b56bb))
* podstawowe: Wyzwalanie sukces na opcjonalne, ale ma inne moduły pomyślne ([#851](https://github.com/jzaefferer/jquery-validation/issues/851), [f93e1de](https://github.com/jzaefferer/jquery-validation/commit/f93e1deb48ec8b3a8a54e946a37db2de42d3aa2a))
* podstawowe: Użyj zwykłego elementu zamiast bez zawijania element ponownie ([03cd4c9](https://github.com/jzaefferer/jquery-validation/commit/03cd4c93069674db5415a0bf174a5870da47e5d2))
* Podstawowe: Upewnij się, zdalne jest wykonywany w ostatniej ([#711](https://github.com/jzaefferer/jquery-validation/issues/711), [ad91b6f](https://github.com/jzaefferer/jquery-validation/commit/ad91b6f388b7fdfb03b74e78554cbab9fd8fca6f))
* Pokaz: Użyj opcji poprawne pokaz wieloczęściową. ([#1025](https://github.com/jzaefferer/jquery-validation/issues/1025), [070edc7](https://github.com/jzaefferer/jquery-validation/commit/070edc7be4de564cb74cfa9ee4e3f40b6b70b76f))
* Napraw $/ jQuery użycie w dodatkowych metod. Poprawki #839 ([#839](https://github.com/jzaefferer/jquery-validation/issues/839), [59bc899](https://github.com/jzaefferer/jquery-validation/commit/59bc899e4586255a4251903712e813c21d25b3e1))
* Udoskonalaj tłumaczenia chińskim ([1a0bfe3](https://github.com/jzaefferer/jquery-validation/commit/1a0bfe32b16f8912ddb57388882aa880fab04ffe))
* Wstępne wdrożenie wymaganych ARIA ([bf3cfb2](https://github.com/jzaefferer/jquery-validation/commit/bf3cfb234ede2891d3f7e19df02894797dd7ba5e))
* Lokalizacja: zmiana Zaakceptuj rozszerzenia. Poprawki #771, zamyka gh 793. ([#771](https://github.com/jzaefferer/jquery-validation/issues/771), [12edec6](https://github.com/jzaefferer/jquery-validation/commit/12edec66eb30dc7e86756222d455d49b34016f65))
* Komunikaty: Dodawanie lokalizacji islandzkim ([dc88575](https://github.com/jzaefferer/jquery-validation/commit/dc885753c8872044b0eaa1713cecd94c19d4c73d))
* Komunikaty: Dodaj brakujące kropki "bg", "fr" i "sr" komunikatów. ([adbc636](https://github.com/jzaefferer/jquery-validation/commit/adbc6361c377bf6b74c35df9782479b1115fbad7))
* Komunikaty: Create messages_sr_lat.js ([f2f9007](https://github.com/jzaefferer/jquery-validation/commit/f2f90076518014d98495c2a9afb9a35d45d184e6))
* Komunikaty: Utwórz messages_tj.js ([de830b3](https://github.com/jzaefferer/jquery-validation/commit/de830b3fd8689a7384656c17565ee92c2878d8a5))
* Komunikaty: Naprawić sr_lat tłumaczenia, Dodaj brakujące miejsce ([880ba1c](https://github.com/jzaefferer/jquery-validation/commit/880ba1ca545903a41d8c5332fc4038a7e9a580bd))
* Komunikaty: Aktualizacji messages_sr.js, napraw Brak miejsca ([10313f4](https://github.com/jzaefferer/jquery-validation/commit/10313f418c18ea75f385248468c2d3600f136cfb))
* Metody: Dodaj kolejną metodę waluty ([1a981b4](https://github.com/jzaefferer/jquery-validation/commit/1a981b440346620964c87ebdd0fa03246348390e))
* Metody: Dodawanie inteligentnych cudzysłowów do usuwania znaków interpunkcyjnych stripHTML firmy ([aa0d624](https://github.com/jzaefferer/jquery-validation/commit/aa0d6241c3ea04663edc1e45ed6e6134630bdd2f))
* Metody: Napraw metody dateITA unikanie błędów czasu letniego ([279b932](https://github.com/jzaefferer/jquery-validation/commit/279b932c1267b7238e6652880b7846ba3bbd2084))
* Metody: Zlokalizowane metody chilijskie kultury (es-CL) ([cf36b93](https://github.com/jzaefferer/jquery-validation/commit/cf36b933499e435196d951401221d533a4811810))
* Metody: Zaktualizuj adres e-mail, aby użyć wyrażeń regularnych języka HTML5, Usuń metodę e-mail 2 ([#828](https://github.com/jzaefferer/jquery-validation/issues/828), [dd162ae](https://github.com/jzaefferer/jquery-validation/commit/dd162ae360639f73edd2dcf7a256710b2f5a4e64))
* Metoda wzorca: Ograniczniki, należy usunąć, ponieważ implementacje HTML5 nie zawierają tych albo. ([37992c1](https://github.com/jzaefferer/jquery-validation/commit/37992c1c9e2e0be8b315ccccc2acb74863439d3e))
* Ograniczanie karty kredytowej modułu sprawdzania poprawności, aby uwzględnić sprawdzania długości. Zamyka gh 772 ([f5f47c5](https://github.com/jzaefferer/jquery-validation/commit/f5f47c5c661da5b0c0c6d59d169e82230928a804))
* Aktualizacja messages_ko.js — zamyka gh 715 ([5da3085](https://github.com/jzaefferer/jquery-validation/commit/5da3085ff02e0e6ecc955a8bfc3bb9a8d220581b))
* Zaktualizuj messages_pt_BR.js. Zamyka gh 782 ([4bf813b](https://github.com/jzaefferer/jquery-validation/commit/4bf813b751ce34fac3c04eaa2e80f75da3461124))
* Aktualizuj phonesUK i mobileUK do akceptowania nowych prefiksy. Zamyka gh 750 ([d447b41](https://github.com/jzaefferer/jquery-validation/commit/d447b41b830dee984be21d8281ec7b87a852001d))
* Sprawdź, kody pocztowe dziewięciu cyfr. Zamyka gh 726 ([165005d](https://github.com/jzaefferer/jquery-validation/commit/165005d4b5780e22d13d13189d107940c622a76f))
* phoneUS: Dodaj wykluczenia N11. Zamyka gh 861 ([519bbc6](https://github.com/jzaefferer/jquery-validation/commit/519bbc656bcb26e8aae5166d7b2e000014e0d12a))
* resetForm należy wyczyścić wszystkie wartości aria nieprawidłowy ([4f8a631](https://github.com/jzaefferer/jquery-validation/commit/4f8a631cbe84f496ec66260ada52db2aa0bb3733))
* Valid(): Sprawdź wszystkie elementy. Poprawki #791 - valid() sprawdza tylko pierwszy element (nieprawidłowy) ([#791](https://github.com/jzaefferer/jquery-validation/issues/791), [6f26803](https://github.com/jzaefferer/jquery-validation/commit/6f268031afaf4e155424ee74dd11f6c47fbb8553))

<a name="1111--2013-03-22"></a>1.11.1 / 2013-03-22
==================

  * Przywróć także konwertowanie parametry metody zakresu na liczby. Zamyka gh 702
  * Zastąp większość użytkowania PHP mockjax obsługi. Należy również wyczyścić pokaz, przeprowadź aktualizację do nowszej wtyczki maskowane danych wejściowych. Zachowaj pokaz captcha w języku PHP. Poprawki #662
  * Usunąć wbudowanego kodu wyróżnienie mleka pokaz. Wyświetl źródło działa prawidłowo.
  * Napraw pokaz dynamiczne sumy, odstępów przycinania z szablonu zawartości przed przekazaniem do konstruktora jQuery
  * Napraw weryfikacji minimalnej/maksymalnej. Zamyka gh 666. Poprawki #648
  * Stała "wiadomości", pojawi się zgodnie z zasadą i powodują, że wyjątek po zaktualizowaniu za pośrednictwem rules("add"). Zamyka gh 670, rozwiązuje #624
  * Dodaj lokalizację koreański (ko). Zamyka gh 671
  * Udoskonalone metody kod pocztowy Zjednoczone Królestwo, umożliwiające filtrowanie więcej postcodes nieprawidłowy. Zamyka #682
  * Zaktualizuj messages_sv.js. Zamyka #683
  * Zmień grunt łącze do witryny sieci Web projektu. Zamyka #684
  * Przenieś metody zdalnej w dół listy, po których uruchomi ostatniego, wszystkie inne metody stosowane do pola. Poprawki #679
  * Opis plugin.json aktualizacji, powinien zawierać słowo "Zweryfikuj"
  * Rozwiązywanie błędów
  * Usuń moduł ładujący jQuery, użyj ścieżki samego siebie. Pokazy zagnieżdżonych poprawki.
  * Aktualizuj grunt contrib qunit do realizacji PhantomJS 1.8 podczas instalacji za pośrednictwem modułu węzła "phantomjs"
  * Należy valid() zwracać wartość logiczną, a nie 0 lub 1. Poprawki #109 - valid() nie zwraca wartość logiczną

<a name="1110--2013-02-04"></a>1.11.0 / 2013-02-04
==================

  * Usuń czyszczenie jako liczby `min`, `max` i `range` reguły. Rozwiązuje #455. Zamyka gh 528.
  * Aktualizuj istniejące etykiety — poprawki #430 zamyka gh-436
  * Napraw $. validator.format, aby uniknąć interpolacji grupy, gdy co najmniej zastępuje IE8/9 - bash z dopasowaniem. Poprawki #614
  * Napraw mimetype wyrażeń regularnych
  * Dodaj manifestu wtyczki i aktualizowanie nagłówków można po prostu licencją MIT, usunąć niepotrzebne podwójnego licencjonowania (np. jQuery).
  * Hebrajski wiadomości: Usunięto kropki na końcu zdania - gh-568 poprawki
  * Francuski tłumaczenia require_from_group weryfikacji. Rozwiązuje gh 573.
  * Zezwala na być tablicą lub ciągiem — poprawki 479 # grup
  * Usunięto miejsca do magazynowania z wieloma typami MIME
  * Usuń niektóre Data sprawdzania poprawności, błędy składniowe Javascript.
  * Usuń obsługę wtyczki metadanych, Zamień msg zasada danych i danych — (dodano w 907467e8) właściwości.
  * Dodano sftp jako prawidłowy wzorzec adresu url
  * Dodaj Malajski (Mój) lokalizacji
  * Aktualizuj localization/messages_hu.js
  * Usuń focusin/focusout kód Pro. Poprawki 542 # - włączenia interfers jquery.validate ze zdarzeniami focusin i focusout w programem IE9 lub nowszym
  * Lokalizacja: Naprawiono błąd pisowni w tłumaczeniu fiński
  * Usuń wersję demonstracyjną RTM, aby pokazać ikona elementów nieprawidłowych podczas przechodzenia z prawidłową wstecz do nieprawidłowego
  * Naprawiono przedwczesne zwrotu w funkcji zdalnego, który uniemożliwił wywołanie ajax z odbywa się w przypadku, gdy dane wejściowe wprowadzono zbyt szybko. Gwarantuje, że zawsze zdalnej weryfikacji sprawdza poprawność najnowsze wartości.
  * Cofnij poprawkę #244. Rozwiązuje 521 # - E-mail weryfikacji uruchamiany natychmiast po tekst w polu.

<a name="1100--2012-09-07"></a>1.10.0 / 2012-09-07
===================

  * Poprawiono francuskie ciągi dla nowhitespace, phoneUS, phoneUK i mobileUK na podstawie opinii społeczności.
  * Zmień nazwy plików dla language_REGION zgodnie ze standardowego ISO_3166-1 (http://en.wikipedia.org/wiki/ISO_3166-1)dla Tajwanu tha język jest chiński (zh) i region jest Tajwan (TW)
  * Optymalizuj wzorców wyrażeń regularnych, szczególnie w przypadku numerów telefonów Zjednoczone Królestwo.
  * Dodawanie nazwy języka dla każdego pliku, Zmień nazwę kod języka zgodnie ze standard ISO 639 dla estoński, gruziński, ukraiński i chiński (http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)
  * Dodano Kuna lokalizacji (godziny)
  * Edytowano istniejące tłumaczenia francuskim i francuskim tłumaczeń dodatkowe metody zostały dodane.
  * Scalone zmiany do określania niestandardowych komunikatów o błędach w atrybutach danych
  * Zaktualizowane UK Mobile phone numer wyrażenie regularne dla nowych numerów. Poprawki #154
  * Dodaj element do wywołania sukcesu z testem. Poprawki #60
  * Aparat wyrażeń regularnych stały czas dodatkowe metody. Poprawki #131
  * resetForm teraz usuwa stare previousValue dla elementów formularza. Poprawki #312
  * Dodano pole wyboru testu require_from_group i require_from_group zmienione, aby użyć elementValue. Poprawki #359
  * Stała dataFilter problemów odpowiedzi jQuery 1.5.2+. Poprawki #405
  * Dodano jQuery mobilnej wersji demonstracyjnej. Poprawki #249
  * Deoptimize findByName pod kątem poprawności. Poprawki 82 # - $. validator.prototype.findByName przerywa w IE7
  * Dodano obsługę kod pocztowy Stanów Zjednoczonych i testowania. Poprawki #90
  * Zmienione lastElement do lastActive w keyup, Pomiń sprawdzanie poprawności na karcie lub pusty element. Poprawki #244
  * Liczba usuniętych obcięcie z stripHtml. Poprawki #2
  * Stała Nieprawidłowa liczba można prawidłowe zdalnej weryfikacji. Poprawki #286
  * Dodaj łącze do file_input indeks wersji demonstracyjnej
  * Przenieść stary accept — metoda rozszerzenia dodatkową metodę, dodano nowe akceptować metodę, aby obsługiwać filtrowanie mimetype standardową przeglądarką. #287 poprawki i zastępuje #369
  * Wyłącza rozmycia zdarzenie, kiedy onfocusout jest ustawiona na wartość false. Dodano test.
  * Rozwiązano problem z wartości dla pola wyboru i przyciski opcji. Poprawki #363
  * Dodano test rangeWords i stałym wyrażeniem regularnym i granice w metodzie. Poprawki #308
  * Stałe TinyMCE pokazów i dodano łącze do strony pokaz. Poprawki #382
  * Komunikat zmiany lokalizacji dla minimalnej/maksymalnej. Poprawki #273
  * Dodano pseudo selektora dla typów wejściowych tekstu rozwiązać problem z domyślnego pustego typu atrybutu. Dodano testy, a niektóre testu znaczników. Poprawki #217
  * Naprawiono delegata usterki pokaz dynamiczne sumy. Poprawki #51
  * Poprawka niepoprawny komunikat alfanumeryczne modułu sprawdzania poprawności
  * Usunięto niepoprawne false sprawdzanie wymaganego atrybutu
  * Napraw wymaganego atrybutu dla przeglądarek innych html5. Poprawki #301
  * Dodano metody "require_from_group" i "skip_or_fill_minimum"
  * Kod iso poprawne na użytek szwedzki
  * Użyj HTML5 doctype pliki zaktualizowane pokaz HTML
  * Rozwiązano problem z wyrażeniem regularnym dla miejsc dziesiętnych, bez zer wiodących. Dodano nowe metody testu. Poprawki #41
  * Wprowadzenie metody elementValue, która normalizuje wyłącznie wartości będące ciągami (don't touch tablicy wartości wybór wielokrotny). Poprawki #116
  * Obsługa przesyłania dodawane dynamicznie przycisków i zaktualizowanych przypadków testowych. Używa validateDelegate. Kod z żądania Ściągnięcia #9
  * Napraw nieprawidłowe podwójnego cudzysłowu w świetlnymi testu
  * Usuń metodę maxWords, aby uwzględnić górną granicę, wyklucza je. Poprawki #284
  * Błąd stałej gramatyki komunikatu weryfikacji niemieckiego zakresu. Poprawki #315
  * Naprawiono obsługi wielu nazw klas errorClass opcji. Przetestuj Lyncha Max. Poprawki #280
  * Napraw jQuery.format użycia, powinno być $. validator.format. Poprawki #329
  * Metody "all" numery telefonów Zjednoczone Królestwo + postcodes Zjednoczone Królestwo
  * Metoda wzorca: Konwertuj ciąg param RegExp. Poprawki wystawiać #223
  * Błąd gramatyki w Niemczech lokalizacja pliku
  * Dodano estoński lokalizacji dla wiadomości
  * Poprawa obsługi na pokaz themerollered etykietki narzędzia
  * Dodaj typ = "text" jako danych wejściowych pola bez atrybutu typu do qSA, zapoznaj się z
  * Aktualizacja pokazu themerollered na potrzeby etykietkę narzędzia Pokaż błędy jako nakładki.
  * Aktualizacja themerollered wersję demonstracyjną, aby używać najnowszych interfejs użytkownika jQuery (wraz z nowszej wersji jQuery). Przenieś kod około, aby przyspieszyć ładowanie strony.
  * Naprawiono min komunikat o błędzie w języku japońskim uszkodzone.
  * Zaktualizuj wtyczki formularza do najnowszej wersji. Zwiększ pokaz ajaxSubmit.
  * Upuść metody dateDE i numberDE z classRuleSettings pozostałość z tych metod zlokalizowane przenoszenia
  * Przekazywanie przesyłania zdarzeń do wywołania zwrotnego submitHandler
  * Naprawiono #219 - rozwiązywanie valid() elementów z wywołania zwrotnego zależności lub zależność — wyrażenie.
  * Poprawa kompilacji, aby usunąć katalog dist, aby upewnić się, że tylko bieżącej wersji pobiera pliki z rozszerzeniem zip

<a name="190"></a>1.9.0
---
* Dodano lokalizacji Baskijski (UE)
* Dodano lokalizacji Słoweński (SL)
* Rozwiązano problem #127 - fiński tłumaczenia ma jeden: zamiast z;
* Stały rosyjski lokalizacji, problem ze składnią pomocnicza
* Dodano obsługę typów wejściowych HTML5, poprawki #97
* Ulepszona obsługa języków HTML5, przez ustawienie atrybutu novalidate w formularzu i odczytywania tego typu atrybutu.
* Naprawiono showLabel() usunięcie wszystkich klas z elementu błędu. Usuń tylko settings.validClass. Rozwiązuje #151.
* Dodać "pattern" do dodatkowe metody do sprawdzania poprawności dowolnego wyrażenia regularne.
* Metoda ulepszone poczty e-mail nie zezwalać na kropki na końcu (prawidłowe RFC, ale niechciane tutaj). Poprawki #143
* Stałe tłumaczenia szwedzkim i norweskim, stało się przełączony min/max komunikatów. Poprawki #181
* Naprawiono #184 - resetForm: powinien lastElement nie ustawiono
* Naprawiono 71 # - poprawić istniejącą metodę czasu i Dodaj metodę time12h dla formatu godziny am/pm 12 godz.
* Naprawiono #177 - napraw sprawdzania poprawności, radio pojedynczego lub zaznacz pole wyboru, wprowadź
* Naprawiono #189 -: ukryte elementy teraz są ignorowane przez domyślne
* Naprawiono 194 # - wymagane, ponieważ atrybut nie powiedzie się, jeśli jQuery > = wersji 1.6 — użyj .prop zamiast .attr
* Naprawiono 47 #, #39, #32 - numery dozwolonych kart kredytowych zawiera spacje, a także kreski (miejsca do magazynowania są często wprowadzane przez użytkowników).

<a name="181"></a>1.8.1
---
* Dodano lokalizacji tajski (ty), poprawki #85
* Dodano lokalizacji wietnamski (VI), dziękuję Ngoc
* Rozwiązano problem #78. Błąd/prawidłowe stylów mają zastosowanie do wszystkich przycisków opcji w tej samej grupy dla wymaganej weryfikacji.
* Nie używaj form.elements jako that'sn t są obsługiwane w wersji 1.6 jQuery dłużej. Jego buggy jako hell mimo to (IE6-8: form.elements === formularza).

<a name="180"></a>1.8.0
---
* Ulepszone (lokalizacja) — Holandia http://plugins.jquery.com/node/14120)
* Dodano lokalizacji Gruziński (GE), dziękuję Avtandil Kikabidze
* Dodano lokalizacji Serbski (SR), dziękuję Aleksandar Milovac
* Dodano protokołów ipv4 i ipv6 dla dodatkowych metod Dziękuję Natal Ngétal
* Dodano lokalizacji japoński (Japonia), dziękuję Bryan Meyerovich
* Dodano lokalizacji Kataloński (CA), dziękuję Xavier de Pedro
* Naprawiono Brak instrukcji var w pętlach for w
* Poprawka do zdalnej weryfikacji, w którym nieprawidłowo sformatowany komunikat stało się uporządkowane (https://github.com/jzaefferer/jquery-validation/issues/11)
* Poprawki błędów na potrzeby utrzymywania zgodności z jQuery 1.5.1, przy zachowaniu zgodności z poprzednimi wersjami

<a name="17"></a>1.7
---
* Dodano lokalizacji litewski (Litwa)
* (Lokalizacja) dodano Grecki (EL)http://plugins.jquery.com/node/12319)
* Dodano (lokalizacja) — Łotewski (Łotwa)http://plugins.jquery.com/node/12349)
* (Lokalizacja) dodano hebrajski (HE)http://plugins.jquery.com/node/12039)
* (Lokalizacja) Naprawiono hiszpański (ES)http://plugins.jquery.com/node/12696)
* Pokaz themerolled interfejsu użytkownika jQuery dodano
* Usunięto cmxform.js
* Naprawiono cztery Brak (średnikami http://plugins.jquery.com/node/12639)
* Zmieniono nazwę metody telefonu w dodatkowych methods.js do phoneUS
* Dodano phoneUK i mobileUK metody (dodatkowe methods.js http://plugins.jquery.com/node/12359)
* Głębokie rozszerzyć opcje, aby uniknąć modyfikowania wiele form, używając metody reguł (pojedynczy element http://plugins.jquery.com/node/12411)
* Poprawki błędów na potrzeby utrzymywania zgodności z jQuery 1.4.2, przy zachowaniu zgodności z poprzednimi wersjami

<a name="16"></a>1.6
---
* Dodano arabski (AR), portugalski (PTPT), perski (FA), fiński (FI) i lokalizacja bułgarski (Brazylia)
* Zaktualizowano lokalizację szwedzki (Szwecja) (niektóre brakujące iso znaków html)
* Naprawiono $. validator.addMethod poprawnie obsłużyć pusty ciąg, a niezdefiniowane argumentu komunikat
* Naprawiono dwóch przypadkowym zmienne globalne
* Ulepszone minimalna/maksymalna/rangeWords (w dodatkowych methods.js), aby usunąć html przed liczenia; dobre, gdy zliczania wyrazów w edytorze richtext
* Dodane zlokalizowany metody DE, NL i PT, usuwanie metody dateDE i numberDE (messages_de.js i methods_de.js za pomocą metod liczb i dat zamiast tego użyj)
* Stały zdalnego formularza przesyłania synchronizacji, pochwałę do Matas Petrikas
* Ulepszone interaktywne wybierz opcję sprawdzania poprawności, teraz również weryfikowanie po kliknięciu (za pośrednictwem opcji lub wybierz kolejno pozycje niespójne przeglądarkami); nie działa w przeglądarce Safari, które nie wyzwalają Zdarzenie kliknięcia w ogóle na wybranych elementów; poprawki http://plugins.jquery.com/node/11520
* Zaktualizowany do najnowszej wtyczki formularza (2,36), naprawianie http://plugins.jquery.com/node/11487
* Powiąż z Rozmycie zdarzenie equalTo docelowym w celu ponownego zweryfikowania podczas przeznaczone na platformę zmiany i poprawki http://plugins.jquery.com/node/11450
* Uproszczone wybierz opcję sprawdzania poprawności, delegowanie do metody val() jQuery firmy można uzyskać wartość wybierz; należy naprawić http://plugins.jquery.com/node/11239
* Naprawiono domyślną wiadomość dla cyfr (http://plugins.jquery.com/node/9853)
* Rozwiązano problem z pamięci podręcznej komunikatu zdalnego (http://plugins.jquery.com/node/11029 i http://plugins.jquery.com/node/9351)
* Naprawiono Brak średnika w (dodatkowe methods.js http://plugins.jquery.com/node/9233)
* Dodano automatyczne wykrywanie parametrów podstawiania w wiadomości, usuwając konieczność, aby zapewnić format funkcji)http://plugins.jquery.com/node/11195)
* Rozwiązano problem z: wypełnione /: puste nieco spowodowane (Wierzą http://plugins.jquery.com/node/11144)
* Dodać metodę do liczby całkowitej do (dodatkowe methods.js http://plugins.jquery.com/node/9612)
* Metoda errorsFor stały, gdzie dla atrybutu zawiera znaki, których użycie ważność wewnątrz (selektor http://plugins.jquery.com/node/9611)

<a name="155"></a>1.5.5
---
* Poprawka http://plugins.jquery.com/node/8659
* Naprawiono przecinkiem w messages_cs.js

<a name="154"></a>1.5.4
---
* (Usterki) Fixed zdalnego — metoda http://plugins.jquery.com/node/8658)

<a name="153"></a>1.5.3
---
* Naprawiono usterki związane z opcją otoki, gdzie wszystkich elementów nadrzędnych, pasujących do opcji otoki gdzie zaznaczone ()http://plugins.jquery.com/node/7624)
* Zaktualizowano wieloczęściowej demonstrację, aby używać najnowszych właściwości accordion interfejsu użytkownika jQuery
* Dodaje metody dateNL i czas do additionalMethods.js
* Lokalizacja Kazachstan (kk) w art i dodano chiński tradycyjny, chiński (Tajwan, tw)
* Przenieść jQuery.format (dawniej String.format) do jQuery.validator.format, jQuery.format jest przestarzały i zostanie usunięte w wersji 1.6 (zobacz http://code.google.com/p/jquery-utils/issues/detail?id=15 szczegóły)
* Wyczyszczone messages_pl.js i messages_ptbr.js (nadal zdefiniowane komunikaty dla Maksymalna/minimalna/rangeValue, które zostały usunięte w 1.4)
* Usunięto wadliwy operatora logicznego w prawidłowej wtyczka metody dla wielu elementów; teraz wszystkie elementy muszą być prawidłowe dla (atrybut typu wartość logiczna true wyniku http://plugins.jquery.com/node/8481)
* Rozszerzenie $. validator.addMethod: Niezdefiniowany trzeci — argument komunikatu nie zastępować istniejące (wiadomości http://plugins.jquery.com/node/8443)
* Ulepszenie submitHandler opcji: W przypadku użycia, jest wstawiane do formularza przed wywołaniem submitHandler kliknij zdarzeń na przesłanie przyciski są przechwytywane i przycisk przesyłania, a usuwane później; przechowuje (nieuszkodzone przyciski przesyłania http://plugins.jquery.com/node/7183#comment-3585)
* Dodano opcję validClass, domyślna "ważne", który dodaje tej klasy z prawidłowymi elementami po weryfikacji (http://dev.jquery.com/ticket/2205)
* Metoda dodano creditcardtypes additionalMethods.js, w tym testy (za pośrednictwem http://dev.jquery.com/ticket/3635)
* Ulepszone metody zdalnej do obsługi wiadomości serverside jako ciąg lub wartość PRAWDA prawidłowe lub wartość false dla nieprawidłowy (komunikat zdefiniowany po stronie klienta przy użyciu http://dev.jquery.com/ticket/3807)
* Ulepszone zaakceptować metodę, aby również akceptowania stylów Drupal rozdzielaną przecinkami listę wartości (http://plugins.jquery.com/node/8580)

<a name="152"></a>1.5.2
---
* Naprawiono wiadomości w dodatkowych methods.js maxWords, minWords i rangeWords obejmujący wywołanie $.format
* Stała wartość przekazywana do metody, aby wykluczyć z powrotu karetki (\r), jest takie same jak val() firmy jQuery
* Dodano lokalizacji słowacki (sk)
* Dodano pokaz dla integracji z kartami interfejsu użytkownika jQuery
* Dodano przykład wybiera grupowania na znaki tabulacji pokaz (Zobacz drugi karty, pola Data urodzenia)

<a name="151"></a>1.5.1
---
* Zaktualizowano marketo wersję demonstracyjną, aby użyć opcji invalidHandler zamiast zdarzeń formularz nieprawidłowe powiązania
* Dodano przykład integracji edytora tinymce.
* Dodano lokalizacji Ukraiński (ua)
* Sprawdzanie poprawności o stałej długości, aby pracować z wartością przycięty (Regresja, z której usunięto ogólne przycinania przed sprawdzania poprawności w wersji 1.5)
* Różne drobnych poprawek dla zachowania zgodności z 1.2.6 i 1.3

<a name="15"></a>1,5
---
* Ulepszone pokazu podstawowe sprawdzanie poprawności pole Potwierdź hasło po zmianie hasła
* Naprawiono podstawowe sprawdzanie poprawności do przekazania nieodciętej wartości wejściowej jako pierwszy parametr do metody sprawdzania poprawności, zmieniona wymagane; przerywa istniejącą metodę niestandardowych, które korzystają z przycinanie
* Dodano norweski (no), włoski (on), węgierski (hu) i lokalizacja rumuński (ro)
* Naprawiono #3195: Dwa usterkom w lokalizacji szwedzki
* Naprawiono 3503 #: Rules("add"), aby zaakceptować komunikaty o właściwości rozszerzone: umożliwia określenie dodawać niestandardowe komunikaty do elementu za pomocą reguł ("add", {wiadomości: {wymagane: "Required! " } });
* Naprawiono 3356 #: Regresja z #2908 przy użyciu opcji meta
* Naprawiono 3370 #: Dodano ignoreTitle opcję, Pomiń odczytywać wiadomości od atrybutu title pomaga uniknąć problemów z paska narzędzi Google; Wartość domyślna to false dla zgodności
* Naprawiono 3516 #: Zdarzenie nieprawidłowe formularza wyzwalacza, nawet wtedy, gdy chodzi o weryfikację zdalną
* Dodano opcję invalidHandler jako skrót do powiązania ("nieprawidłowy form", function() {})
* Rozwiązano problem z przeglądarki Safari dla obciążenia wskaźnika w ajaxSubmit integracji demo (najpierw dołączyć do treści, a następnie ukryć)
* Dodano test sprawdzania poprawności creditcard i ulepszona domyślna komunikatu
* Ulepszone zdalnej weryfikacji, akceptując sposoby .ajax $ jako parametr przekazywania (ciągu adresu url lub opcji, w tym właściwość adresu url oraz wszystkie inne obsługuje ten .ajax $)

<a name="14"></a>1.4
---
* Stała #2931 i weryfikowanie elementów w kolejności dokumentu i Ignoruj typ = dane wejściowe obrazu
* Naprawiono użycie zmiennych $ i jQuery, teraz w pełni zgodny z wszystkie odmiany funkcji noConflict użycia
* Zaimplementowane #2908, włączania niestandardowych komunikatów za pomocą klasy ala metadanych = "{wymagane: true, wiadomości: {wymagana:"wymaganego pola"}}", dodano pokaz/custom wiadomości metadanych — demo.html
* Usunąć przestarzałe metody minValue (min), maxValue (maksymalna), rangeValue (rangevalue), minLength (minlength), maxLength (maxlength) rangeLength (rangelength)
* Naprawiono Regresje #2215: Wywołanie unhighlight tylko dla bieżącego elementów, nie wszystko
* Zaimplementowano #2989, włączanie obrazu przycisk, aby anulować sprawdzania poprawności
* Rozwiązano problem, których dotyczy niepoprawnego sprawdzania poprawności względem maxlength = 0
* Dodano lokalizacji czeski (cs)
* Resetuj validator.submitted na validator.resetForm(), włączenie pełnego resetowania, gdy jest to konieczne
* Naprawiono 3035 #, pomijanie falsy wszystkie atrybuty podczas odczytywania reguł (0, Niezdefiniowany, pusty ciąg), usunąć część maxlength obejście (0)
* Dodano niderlandzki (nl) lokalizacji (#3201)

<a name="13"></a>1.3
---
* Naprawiono zdarzenia formularza nieprawidłowy teraz tylko wyzwalane, gdy formularz jest nieprawidłowy
* Dodano hiszpański (es), rosyjski (ru), Portugalski brazylijski (ptbr), turecki (tr) i lokalizacja Polski (pl)
* Dodano removeAttrs dodatek plug-in, aby ułatwić dodawanie i usuwanie wiele atrybutów
* Dodano opcję grup, aby wyświetlić jeden komunikat o wiele elementów, za pośrednictwem grupy: {arbitraryGroupName: "Nazwapola2 Nazwapola1 [, fieldNameN"}
* Ulepszone rules() do dodawania i usuwania reguł (statyczne): reguły ("add", "metoda1 [, methodN]" / {method1:param [, method_n:param]}) i zasad ("Usuń" ["metoda1 [, method_n]")
* Rozszerzone reguły opcja przyjmuje rozdzielonych spacjami ciąg listę metod, np. {datę urodzenia: "required daty"}
* Naprawiono sprawdzanie poprawności grupy pole wyboru przy użyciu wbudowanych reguł: Tak długo, jak zasady są określone na pierwszy element, grupa jest teraz prawidłowo sprawdzania poprawności po kliknięciu
* Naprawiono 2473 #, ignorując wszystkie reguły z parametrem jawne false atrybut typu wartość logiczna, np. wymagane: false jest taka sama jak nie został podany wymagany na wszystkich (go został obsłużony zgodnie z wymaganiami: true do tej pory)
* Naprawiono 2424 #, za pomocą zmodyfikowanego poprawki z #2473: Metody, zwracając niezgodnością zależności nie przestawaj inne reguły z oceniane już; jednak Powodzenie nie jest stosowana dla pól opcjonalnych
* Stały adres url i wiadomości e-mail sprawdzania poprawności, aby nie używać wartości przycięty
* Naprawiono creditcard weryfikacji do akceptowania tylko cyfry i łączniki ("asdf" nie jest liczbą creditcard prawidłowe)
* Zezwalaj na dane wejściowe i przycisk elementy przyciski "Anuluj" (za pośrednictwem klasy = "Anuluj")
* Naprawiono #2215: Naprawiono komunikat o błędzie do wywołania unhighlight jako część pokazywaniu i ukrywaniu wiadomości, nie ma więcej efektów wizualnych po stronie podczas sprawdzania elementu i validator.checkForm wyodrębnione do sprawdzania poprawności formularz bez sideeffects interfejsu użytkownika
* Rewrote selektory niestandardowe (: pole pozostanie puste,: wypełnieniu: niezaznaczone) przy użyciu funkcji w celu zachowania zgodności z powietrza

<a name="121"></a>1.2.1
-----

* Wtyczka delegata powiązane z weryfikacji wtyczki — zawsze wymaga mimo to
* Ulepszone zdalnej weryfikacji, aby dołączyć składniki Report Part z wtyczki ajaxQueue właściwe synchronizacji (nie dodatkowe wtyczki konieczne)
* Naprawiono stopRequest, aby zapobiec pendingRequest < 0
* Właściwość jQuery.validator.autoCreateRanges dodano wartość domyślna to false, Włącz, aby przekonwertować minimalny/maksymalny zakres i minlength/maxlength rangelength; to zasadniczo naprawia problem wprowadzone przez automatyczne tworzenie zakresów w 1.2
* Naprawiono opcjonalny metody nie Wyróżnij nic w ogóle Jeśli to pole jest puste, który jest nie wyzwolenia sukces
* Zezwalaj na wartość false/null wyróżnienia/unhighlight opcje zamiast wymuszania wielokrotnego wykonania-nothing — wywołanie zwrotne, nawet wtedy, gdy niczego nie trzeba być wyróżniony
* Wywołanie validate() stałej bez elementów zaznaczone, zwracanie niezdefiniowane zamiast zgłoszenie błędu.
* Ulepszone pokaz, zastępując metadanych klasy i atrybuty do określania reguł
* Usunięto błąd stosowania nie niestandardowy komunikat do zdalnej weryfikacji
* Zmodyfikowane poczty e-mail i adres url sprawdzania poprawności, aby wymagać etykietę domeny i górnej etykiety
* Stały adres url i wiadomości e-mail sprawdzania poprawności, aby wymagać domeny najwyższego poziomu (faktycznie będą musieli etykietę domeny); w wersji 1.2 (domeny najwyższego poziomu jest opcjonalny) zostanie przeniesiony do uzupełnienia url2 oraz e-mail 2
* Pokaz stałej sumy dynamiczne w IE6 7 dni w tygodniu i ulepszone Tworzenie szablonów przy użyciu textarea do przechowywania szablonu wielowierszowy i Interpolacja ciągów
* Przykład użycia elementu form dodano logowania za pomocą linku "Hasło konta E-mail", który sprawia, że pole hasła jest opcjonalne
* Rozszerzone sumy dynamiczne pokaz z przykładem pojedynczej wiadomości dla dwóch pól

<a name="12"></a>1.2
---

* Dodano przykład weryfikacji AJAX captcha (na podstawie http://psyrens.com/captcha/)
* Dodano Pamiętaj mleka demo (Dziękuję RTM zespół uprawnienia!)
* Dodano pokaz usługi marketo (Glen Lipka Dziękujemy!)
* Dodano obsługę technologii ajax krzyżowa Weryfikacja, zobacz metodę "zdalnego"; serverside zwraca JSON, wartość true dla elementów prawidłową wartość false lub ciąg związanych z nieprawidłowymi, ciąg jest używany jako wiadomości
* Dodano wyróżnianie i unhighlight opcje, domyślnie włącza/wyłącza errorClass w elemencie, umożliwia wyróżnianie niestandardowe
* Dodano valid() wtyczki metody dla łatwe programowe kontroli formularze i pola, bez konieczności użycia interfejsu API modułu sprawdzania poprawności
* Dodano rules() wtyczki metodę, aby odczytywać i zapisywać reguły dla elementu (obecnie tylko do odczytu)
* Zamieniono wyrażeń regularnych w przypadku metody wiadomości e-mail dzięki udział Scott Gonzales, zobacz http://projects.scottsplayground.com/email_address_validation/
* Modyfikować danych zdarzeń architektury polegać wyłącznie na delegowanie, zarówno usprawnienie wydajności i łatwości użycia dla deweloperów (wymaga jquery.delegate.js)
* Przeniesiono dokumentacji z wbudowaną, aby http://docs.jquery.com/Plugins/Validation — w tym interaktywne przykłady dla wszystkich metod
* Usunięto validator.refresh() sprawdzania poprawności jest teraz całkowicie dynamiczne
* Zmieniono nazwę minValue min, maxValue do max i rangeValue do zakresu, wycofano poprzedniej nazwy (do usunięcia w 1.3)
* Zmieniono nazwę minLength minlength, maxLength maxlength i rangeLength do rangelength, wycofano poprzedniej nazwy (do usunięcia w 1.3)
* Dodano funkcja min i max do i należeć do zakresu i minlength i maxlength do rangelength
* Dodano obsługę parametry dynamiczne reguły, dzięki czemu można na przykład określić funkcję jako parametr. Element minlength o nazwie podczas weryfikowania elementu
* Zezwalaj, aby określić wartość null lub pusty ciąg jako komunikat do wyświetlenia, nic nie (zobacz pokaz usługi marketo)
* Przebudowa reguły: Teraz obsługuje kombinacja opcję reguły, metadane, klas (nowy) i atrybuty (nowy), zobacz rules(), aby uzyskać szczegółowe informacje

<a name="112"></a>1.1.2
---

* Zamieniono wyrażenie regularne dla metody adresu URL, dzięki udział Scott Gonzales, zobacz http://projects.scottsplayground.com/iri/
* Ulepszone e-mail metodę, aby lepiej obsługiwać znaki unicode
* Naprawiono błąd kontener, aby ukryć, gdy wszystkie elementy są prawidłowe, a nie tylko na przesyłania formularza
* Naprawiono String.format do jQuery.format (przeniesienie do przestrzeni nazw jQuery)
* Naprawiono zaakceptować metodę, aby zaakceptować zarówno wielkich i małych rozszerzeń
* Naprawiono validate() wtyczki metodę, aby utworzyć tylko jedno wystąpienie modułu sprawdzania poprawności dla danego formularza i zawsze zwracają jednego wystąpienia (pozwala uniknąć powiązania zdarzeń wiele razy)
* Zmienić tryb debugowania konsoli dziennika za "error" na poziomie "ostrzeżenie"

<a name="111"></a>1.1.1
-----

* Naprawiono XHTML nieprawidłowe, uniemożliwianie tworzenia etykiety błąd w programie Internet Explorer od jQuery 1.1.4
* Stałe i ulepszone String.format: Wyszukiwanie globalne i Zamień, lepszą obsługę tablica argumentów
* Obsługa stały przycisk anulowania na potrzeby przechowywania stanu zamiast element formularza weryfikacji obiektu
* Rozwiązano nazwy selektorów do obsługi nazwy "złożona", np. nawiasy kwadratowe ("list[]") zawierający
* Dodano przycisku i wyłączone elementy do wykluczenia z weryfikacji
* Przenieść element obsługi zdarzeń można odświeżyć, aby można było dodać procedury obsługi do nowych elementów
* Stały adres e-mail sprawdzania poprawności, aby zezwolić na długo domen najwyższego poziomu poziomu (np. ".travel")
* Przeniesione showErrors() z valid() do form()
* Dodano validator.size(): zwraca liczbę bieżących błędów
* Wywołaj submitHandler z modułu sprawdzania poprawności jako zasięg w celu ułatwienia dostępu jego metod, np. Aby znaleźć błąd etykiet przy użyciu errorsFor(Element)
* Zgodne z jQuery 1.1.x i 1.2.x

<a name="11"></a>1.1
---

* Dodano sprawdzanie poprawności Rozmycie keyup, a następnie kliknij (dla pola wyboru i radiobutton). Zastępuje opcję zdarzeń.
* ResetForm stałej
* Naprawiono demo niestandardowe — metody

<a name="10"></a>1.0
---

* Ulepszone numer numberDE metody i sprawdź, czy są prawidłowe liczby dziesiętne z ogranicznikami
* Tylko te elementy, które mają reguły są sprawdzane (w przeciwnym razie sukces — opcja jest stosowana do wszystkich elementów)
* Metoda numeracji creditcard dodano (dzięki Brian Klug)
* Dodano opcji Ignoruj, np. Ignoruj: "[@type= ukryte]", przy użyciu to wyrażenie, aby wykluczyć elementy do sprawdzania poprawności. Wartość domyślna: Brak, jednak przesyłanie i resetowania przyciski są zawsze ignorowane
* Silnie ulepszone funkcje — jako — liczba komunikatów, zapewniając elastyczne pomocnika String.format
* Zaakceptuj funkcji jako komunikaty, zapewniając środowiska uruchomieniowego niestandardowe komunikaty o
* Naprawiono wyłączenie elementów bez reguł z successList
* Naprawiono custom metoda demo zastąpione wiadomości zawierające liczbę błędów alertu
* Naprawiono formularza przesyłania zapobiegania przy użyciu submitHandler
* Całkowicie usunięte zależność od identyfikatory elementów, jednak są nadal używane (jeśli istnieją) do połączyć błąd etykiet danych wejściowych. Osiągnąć przy użyciu tablicy {nazwa, wiadomości, element} zamiast obiekt z parami id: komunikat dla wewnętrznego errorList.
* Dodano obsługę określania proste zasady jako zwykłe ciągi, np. "required" jest odpowiednikiem {wymagane: true}
* Dodano funkcję: Dodaj errorClass do elementu nadrzędnego nieprawidłowe pole s, dzięki czemu jest łatwy do nadawania stylu kontenera etykiety/pola lub etykiety w polu.
* Dodano funkcję: focusCleanup — Jeśli włączone, usuwa errorClass z nieprawidłowe elementy i ukrywa wszystkie błędy wiadomości, zawsze wtedy, gdy element ma fokus.
* Dodano opcję Powodzenie, aby wyświetlać pola została pomyślnie zweryfikowana.
* Rozwiązano wybierz Opera — problem (unikanie kolizja atrybut)
* Naprawiono problemy z koncentrujących się ukryte elementy w programie Internet Explorer
* Dodano funkcję, aby pominąć weryfikację przyciski do przesyłania danych za pomocą klasy "Anuluj"
* Naprawiono potencjalne problemy z paska narzędzi Google przez preferowanie wtyczki opcji komunikatów za pośrednictwem atrybutu title
* submitHandler jest wywoływana tylko wtedy, gdy rzeczywistego przesyłanie zdarzeń został obsłużony, validator.form() zwraca wartość false, tylko w przypadku nieprawidłowy formularzy
* Nieprawidłowe elementy teraz koncentrujące się tylko na przesyłanie lub za pośrednictwem validator.focusInvalid(), unikając wszystkie problemy z fokus na Rozmycie
* IE6 błąd kontener układu problem został rozwiązany.
* Dostosowywanie błąd elementu za pomocą opcji errorElement
* Dodano validator.refresh(), aby znaleźć nowe dane wejściowe w postaci
* Dodano zaakceptować metody sprawdzania poprawności, sprawdzanie rozszerzeń plików
* Funkcja zależna ulepszone przez dodanie dwóch wyrażeń niestandardowych: ": pusty" Aby wybrać elementy o wartości pustej i: wypełnione, aby wybrać elementy o wartości, zarówno z wyłączeniem białe znaki
* Metoda resetForm() należy dodać do modułu sprawdzania poprawności: Resetuje każdy element formularza (przy użyciu wtyczki formularza, jeśli jest dostępny), usuwa klas na nieprawidłowe elementy i ukrywa wszystkie komunikaty o błędach
* Naprawiono dokumenty dotyczące validator.showErrors()
* Naprawiono błąd tworzenia etykiety zawsze używaj html() zamiast text(), dzięki czemu dowolnego elementu HTML przekazanej jako komunikaty
* Usunięto błąd tworzenia etykiety do użycia klasy określonego błędu
* Funkcja dodano zależności: Wymaga metoda akceptuje zarówno ciąg (jQuery wyrażenia), jak i funkcji jako argument
* Ulepszone intensywnie, Dostosowywanie wyświetlania komunikat o błędzie: Użyj zwykłych wiadomości i Pokaż/Ukryj dodatkowy kontener; Całkowitego zastąpienia wyświetlania komunikatu przy użyciu własnego mechanizmu (będąc możliwość delegowania domyślny program obsługi; Dostosowywanie, umieszczając wygenerowanego etykiet (zamiast domyślnego poniżej elementu)
* Usunięto dwóch głównych usterek w programie IE (błąd kontenery) i Opera (metadane)
* Zmodyfikowane metody sprawdzania poprawności, aby zaakceptować puste pola jako prawidłowe (wyjątek: kurs, wymagane, a także metody equalTo)
* Zmieniono nazwę "min" do "minLength", "max" do "maxLength", "length" do "rangeLength"
* Dodano "minLength", "maxValue" i "rangeValue"
* Ujednolicony interfejs API obsługi różnych zdarzeń. Domyślnie, Prześlij, może zostać wyłączony. Jeśli określono dowolne zdarzenie, który jest stosowany do każdego elementu (zamiast całego formularza). Łączenie keyup weryfikacji przy użyciu przesłać sprawdzania poprawności jest teraz bardzo proste — Konfiguracja
* Dodano obsługę dla jednego komunikat na rule podczas definiowania komunikaty za pośrednictwem ustawień wtyczki
* Dodano obsługę do opakowania metadanych w pewien element nadrzędny. Parametr jest przydatne, gdy metadane są używane zbyt dla innych wtyczek.
* Wycofanej testy i pokazy: Mniej plików lepsze pokazy
* Ulepszone dokumentacji: Więcej przykładów dla metod referencyjnej teksty wyjaśniających pewne podstawowe informacje

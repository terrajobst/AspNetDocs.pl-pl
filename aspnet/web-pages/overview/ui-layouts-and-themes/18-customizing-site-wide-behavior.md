---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Dostosowywanie zachowania całej witryny dla witryn ASP.NET Web Pages (Razor) | Microsoft Docs
author: Rick-Anderson
description: W tym rozdziale wyjaśniono, jak wprowadzać ustawienia do całej witryny sieci Web lub całego folderu, a nie tylko na stronie.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: f05e05f725d9209bce283ce18659ae5efe4de2ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634625"
---
# <a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Dostosowywanie zachowania całej witryny dla witryn ASP.NET Web Pages (Razor)

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak utworzyć ustawienia po stronie witryny dla stron w witrynie internetowej ASP.NET Web Pages (Razor).
> 
> Zawartość:
> 
> - Jak uruchomić kod, który umożliwia ustawianie wartości (wartości globalne lub ustawienia pomocnika) dla wszystkich stron w witrynie.
> - Jak uruchomić kod, który umożliwia ustawianie wartości dla wszystkich stron w folderze.
> - Jak uruchomić kod przed i po załadowaniu strony.
> - Jak wysłać błędy do centralnej strony błędu.
> - Jak dodać uwierzytelnianie do wszystkich stron w folderze.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - ASP.NET strony sieci Web (Razor) 2
> - WebMatrix 3
> - Biblioteka pomocników sieci Web ASP.NET (pakiet NuGet)
>   
> 
> Ten samouczek działa również z ASP.NET Web Pages 3 i Visual Studio 2013 (lub Visual Studio Express 2013 dla sieci Web), z tą różnicą, że nie można użyć biblioteki ASP.NET Web Pomocniks.

<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Dodawanie kodu startowego witryny internetowej dla stron sieci Web ASP.NET

W przypadku większości kodu, który można napisać na stronach sieci Web ASP.NET, pojedyncza strona może zawierać cały kod, który jest wymagany dla tej strony. Na przykład jeśli strona wyśle wiadomość e-mail, można umieścić cały kod dla tej operacji na jednej stronie. Może to obejmować kod umożliwiający zainicjowanie ustawień wysyłania wiadomości e-mail (czyli dla serwera SMTP) i wysłania wiadomości e-mail.

Jednak w niektórych sytuacjach można uruchomić jakiś kod przed uruchomieniem dowolnej strony w witrynie. Jest to przydatne w przypadku ustawiania wartości, które mogą być używane w dowolnym miejscu w lokacji (nazywanej *wartościami globalnymi*). Na przykład niektórzy pomocnicy wymagają podania wartości, takich jak ustawienia poczty e-mail lub klucze kont. Te ustawienia mogą być przydatne w wartościach globalnych.

Można to zrobić, tworząc stronę o nazwie *\_AppStart. cshtml* w folderze głównym witryny. Jeśli ta strona istnieje, zostanie uruchomiona po raz pierwszy. W związku z tym jest dobrym miejscem, aby uruchomić kod, aby ustawić wartości globalne. (Ponieważ *\_AppStart. cshtml* ma prefiks podkreślenia, ASP.NET nie wyśle strony do przeglądarki, nawet jeśli użytkownicy zażądają go bezpośrednio).

Na poniższym diagramie pokazano, jak działa strona *\_AppStart. cshtml* . Gdy żądanie pojawia się na stronie, a jeśli jest to pierwsze żądanie dla każdej strony w witrynie, ASP.NET najpierw sprawdza, czy istnieje strona *\_AppStart. cshtml* . W takim przypadku każdy kod na stronie *\_AppStart. cshtml* zostanie uruchomiony, a następnie zostanie uruchomiona żądana strona.

![Image](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Ustawianie wartości globalnych dla witryny sieci Web

1. W folderze głównym witryny internetowej WebMatrix Utwórz plik o nazwie *\_AppStart. cshtml*. Plik musi znajdować się w katalogu głównym witryny.
2. Zastąp istniejącą zawartość następującym: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Ten kod przechowuje wartość w słowniku `AppState`, która jest automatycznie dostępna dla wszystkich stron w witrynie. Zauważ, że plik *\_AppStart. cshtml* nie zawiera żadnych znaczników. Na stronie zostanie uruchomiony kod, a następnie zostanie przekierowany na stronę, której pierwotnie zażądano.

    > [!NOTE]
    > Należy zachować ostrożność podczas umieszczania kodu w pliku *\_AppStart. cshtml* . Jeśli wystąpią błędy w kodzie w pliku *\_AppStart. cshtml* , witryna internetowa nie zostanie uruchomiona.
3. W folderze głównym Utwórz nową stronę o nazwie *nazwa_aplikacji. cshtml*.
4. Zastąp domyślne znaczniki i kod następującymi: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Ten kod wyodrębnia wartość z obiektu `AppState`, który został ustawiony na stronie *\_AppStart. cshtml* .
5. Uruchom stronę *nazwa_aplikacji. cshtml* w przeglądarce. (Upewnij się, że strona została wybrana w obszarze roboczym **pliki** przed jej uruchomieniem). Na stronie zostanie wyświetlona wartość globalna. 

    ![Image](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Ustawianie wartości dla pomocników

Dobrym zastosowaniem pliku *\_AppStart. cshtml* jest ustawienie wartości dla pomocników używanych w danej lokacji, które muszą zostać zainicjowane. Typowe przykłady to ustawienia poczty e-mail pomocnika `WebMail` oraz klucze prywatne i publiczne dla pomocnika `ReCaptcha`. W takich przypadkach można ustawić wartości jeden raz w *\_AppStart. cshtml* , a następnie są one już ustawione dla wszystkich stron w witrynie.

Ta procedura pokazuje, jak ustawić `WebMail` ustawienia globalne. (Aby uzyskać więcej informacji na temat korzystania z pomocnika `WebMail`, zobacz [Dodawanie poczty e-mail do witryny ASP.NET Web Pages](../getting-started/11-adding-email-to-your-web-site.md).)

1. Dodaj bibliotekę pomocników sieci Web ASP.NET do witryny internetowej zgodnie z opisem w temacie [Instalowanie pomocników w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), jeśli jeszcze nie została dodana.
2. Jeśli nie masz jeszcze pliku *\_AppStart. cshtml* , w folderze głównym witryny sieci Web Utwórz plik o nazwie *\_AppStart. cshtml*.
3. Dodaj następujące ustawienia `WebMail` do pliku *\_AppStart. cshtml* : 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Zmodyfikuj następujące ustawienia związane z pocztą e-mail w kodzie:

   - Ustaw `your-SMTP-host` na nazwę serwera SMTP, do którego masz dostęp.
   - Ustaw `your-user-name-here` na nazwę użytkownika dla konta serwera SMTP.
   - Ustaw `your-account-password` na hasło dla konta serwera SMTP.
   - Ustaw `your-email-address-here` na własny adres e-mail. Jest to adres e-mail, z którego wiadomość jest wysyłana. (Niektórzy dostawcy poczty e-mail nie umożliwiają określania innego adresu `From` i będą używać nazwy użytkownika jako adresu `From`).

     Aby uzyskać więcej informacji na temat ustawień SMTP, zobacz [Konfigurowanie ustawień poczty e-mail](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) w artykule [wysyłanie wiadomości e-mail z witryny ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) i [problemy z wysyłaniem poczty e-mail](https://go.microsoft.com/fwlink/?LinkId=253001#email) w [przewodniku rozwiązywania problemów z ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).
4. Zapisz plik *\_AppStart. cshtml* i zamknij go.
5. W folderze głównym witryny sieci Web Utwórz nową stronę o nazwie *TestEmail. cshtml*.
6. Zastąp istniejącą zawartość następującym: 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. Uruchom stronę *TestEmail. cshtml* w przeglądarce.
8. Wypełnij pola, aby wysłać do siebie wiadomość e-mail, a następnie kliknij przycisk **Wyślij**.
9. Sprawdź pocztę e-mail, aby upewnić się, że wiadomość została wykorzystana.

Ważna część tego przykładu polega na tym, że ustawienia, które zwykle nie są zmieniane — takie jak nazwa serwera SMTP i poświadczenia poczty e-mail — są ustawiane w pliku *\_AppStart. cshtml* . W ten sposób nie trzeba ich ponownie ustawiać na każdej stronie, na której jest wysyłana wiadomość e-mail. (Chociaż z jakiegoś powodu trzeba zmienić te ustawienia, można ustawić je pojedynczo na stronie). Na stronie można ustawić tylko te wartości, które zwykle zmieniają się za każdym razem, jak odbiorca i treść wiadomości e-mail.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Uruchamianie kodu przed i po plikach w folderze

Podobnie jak można użyć *\_AppStart. cshtml* , aby napisać kod przed uruchomieniem stron w lokacji, można napisać kod, który jest uruchamiany przed (i po) każdej stronie w określonym folderze. Jest to przydatne w przypadku elementów, takich jak Ustawianie tej samej strony układu dla wszystkich stron w folderze, lub sprawdzanie, czy użytkownik jest zalogowany przed uruchomieniem strony w folderze.

W przypadku stron w określonych folderach można utworzyć kod w pliku o nazwie *\_PageStart. cshtml*. Na poniższym diagramie pokazano, jak działa strona *\_PageStart. cshtml* . Gdy żądanie pojawia się na stronie, ASP.NET najpierw sprawdza, czy na stronie *\_AppStart. cshtml* i uruchamia, czy. Następnie ASP.NET sprawdza, czy istnieje strona *\_PageStart. cshtml* , i jeśli tak, uruchamia. Następnie uruchamia żądaną stronę.

Na stronie *\_PageStart. cshtml* można określić, gdzie podczas przetwarzania żądana strona ma być uruchamiana przez uwzględnienie metody `RunPage`. Pozwala to uruchomić kod przed uruchomieniem żądanej strony, a następnie ponownie po nim. Jeśli nie dołączysz `RunPage`, zostanie uruchomiony cały kod *\_PageStart. cshtml* , a następnie żądana strona zostanie uruchomiona automatycznie.

![Image](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET umożliwia tworzenie hierarchii plików *\_PageStart. cshtml* . Plik *\_PageStart. cshtml* można umieścić w katalogu głównym lokacji i w dowolnym podfolderze. Po zażądaniu strony zostanie uruchomiony plik *\_PageStart. cshtml* na najwyższego poziomu (najbliżej katalogu głównego witryny), a następnie plik *\_. cshtml* w następnym podfolderze i tak dalej, aż żądanie osiągnie folder zawierający żądaną stronę. Po uruchomieniu wszystkich odpowiednich plików *\_PageStart. cshtml* zostanie uruchomiona żądana strona.

Na przykład może istnieć następująca kombinacja plików *\_PageStart. cshtml* i pliku *default. cshtml* :

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Po uruchomieniu programu */MyFolder/default.cshtml*zostaną wyświetlone następujące elementy:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Uruchamianie kodu inicjującego dla wszystkich stron w folderze

Dobrym zastosowaniem plików *\_PageStart. cshtml* jest zainicjowanie tej samej strony układu dla wszystkich plików w jednym folderze.

1. W folderze głównym Utwórz nowy folder o nazwie *InitPages*.
2. W folderze *InitPages* witryny sieci Web Utwórz plik o nazwie *\_PageStart. cshtml* i Zastąp domyślne znaczniki i kod poniższymi: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. W katalogu głównym witryny sieci Web Utwórz folder o nazwie *Shared*.
4. W folderze *udostępnionym* Utwórz plik o nazwie *\_Layout1. cshtml* i Zastąp domyślne znaczniki i kod następującymi: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. W folderze *InitPages* Utwórz plik o nazwie *Content1. cshtml* i Zastąp istniejącą zawartość następującym: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. W folderze *InitPages* Utwórz inny plik o nazwie *Content2. cshtml* i Zastąp znacznik domyślny następującymi: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Uruchom *Content1. cshtml* w przeglądarce. 

    ![Image](18-customizing-site-wide-behavior/_static/image4.jpg)

    Po uruchomieniu strony *Content1. cshtml* ustawiany jest plik *\_PageStart. cshtml* `Layout`, a także ustawia `PageData["MyBackground"]` na kolor. W *Content1. cshtml*stosowane są układ i kolor.
8. Wyświetlanie *Content2. cshtml* w przeglądarce. 

    Układ jest taki sam, ponieważ obie strony używają tej samej strony układu i koloru jak zainicjowane w *\_PageStart. cshtml*.

## <a name="using-_pagestartcshtml-to-handle-errors"></a>Używanie \_PageStart. cshtml do obsługi błędów

Innym dobrym zastosowaniem pliku *\_PageStart. cshtml* jest utworzenie sposobu obsługi błędów programowania (wyjątków), które mogą wystąpić na dowolnej stronie *. cshtml* w folderze. Ten przykład pokazuje, jak to zrobić.

1. W folderze głównym Utwórz folder o nazwie *InitCatch*.
2. W folderze *InitCatch* witryny sieci Web Utwórz plik o nazwie *\_PageStart. cshtml* i Zastąp istniejący znacznik i kod poniższymi: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    W tym kodzie spróbuj uruchomić żądaną stronę jawnie, wywołując metodę `RunPage` wewnątrz bloku `try`. Jeśli na żądana stronie wystąpią jakieś błędy programowania, kod wewnątrz bloku `catch` zostanie uruchomiony. W takim przypadku kod przekierowuje do strony (*Error. cshtml*) i przekazuje nazwę pliku, który napotkał błąd w ramach adresu URL. (Wkrótce utworzysz stronę).
3. W folderze *InitCatch* witryny sieci Web Utwórz plik o nazwie *Exception. cshtml* i Zastąp istniejący znacznik i kod poniższymi: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Na potrzeby tego przykładu czynności wykonywane na tej stronie są świadomie związane z tworzeniem błędu, próbując otworzyć plik bazy danych, który nie istnieje.
4. W folderze głównym Utwórz plik o nazwie *Error. cshtml* i Zastąp istniejący znacznik i kod następującym kodem: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    Na tej stronie wyrażenie `@Request["source"]` Pobiera wartość spoza adresu URL i wyświetla ją.
5. Na pasku narzędzi kliknij przycisk **Zapisz**.
6. Uruchom w przeglądarce *wyjątek. cshtml* . 

    ![Image](18-customizing-site-wide-behavior/_static/image5.jpg)

    Ponieważ wystąpił błąd w *Exception. cshtml*, *\_PageStart. cshtml* przekieruje do pliku *Error. cshtml* , który wyświetla komunikat.

    Aby uzyskać więcej informacji o wyjątkach, zobacz [wprowadzenie do ASP.NET stron sieci Web przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-_pagestartcshtml-to-restrict-folder-access"></a>Używanie \_PageStart. cshtml w celu ograniczenia dostępu do folderu

Można również użyć pliku *\_PageStart. cshtml* , aby ograniczyć dostęp do wszystkich plików w folderze.

1. W programie WebMatrix Utwórz nową witrynę sieci Web przy użyciu opcji **lokacja z szablonu** .
2. Z dostępnych szablonów wybierz pozycję **Witryna początkowa**.
3. W folderze głównym Utwórz folder o nazwie *AuthenticatedContent*.
4. W folderze *AuthenticatedContent* Utwórz plik o nazwie *\_PageStart. cshtml* i Zastąp istniejący znacznik i kod poniższymi: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    Kod zostanie uruchomiony przez uniemożliwienie buforowania wszystkich plików w folderze. (Jest to wymagane w przypadku scenariuszy, takich jak komputery publiczne, w przypadku których nie chcesz, aby jeden z buforowanych stron był dostępny dla następnego użytkownika). Następnie kod określa, czy użytkownik zalogował się do lokacji, zanim będzie mógł wyświetlić wszystkie strony w folderze. Jeśli użytkownik nie jest zalogowany, kod przekieruje się do strony logowania. Strona logowania może zwrócić użytkownika na stronę, której pierwotnie żądano, jeśli dołączysz wartość ciągu zapytania o nazwie `ReturnUrl`.
5. Utwórz nową stronę w folderze *AuthenticatedContent* o nazwie *Page. cshtml*.
6. Zastąp znaczniki domyślne następującymi:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Uruchom *stronę Page. cshtml* w przeglądarce. Kod przekierowuje użytkownika do strony logowania. Musisz zarejestrować się przed zalogowaniem się. Po zarejestrowaniu i zalogowaniu można przejść do strony i wyświetlić jej zawartość.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe materiały

[Wprowadzenie do programowania stron sieci Web ASP.NET przy użyciu składni Razor](https://go.microsoft.com/fwlink/?LinkID=251587)

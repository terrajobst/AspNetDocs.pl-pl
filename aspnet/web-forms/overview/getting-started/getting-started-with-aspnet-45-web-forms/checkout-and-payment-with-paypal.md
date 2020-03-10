---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Wyewidencjonowywanie i płatność przy użyciu systemu PayPal | Microsoft Docs
author: Erikre
description: Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 62d00a86c6c5845fb894896df65002c7086d039f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78565409"
---
# <a name="checkout-and-payment-with-paypal"></a>Finalizacja zakupu i płatność w systemie PayPal

Autor [Erik Reitan](https://github.com/Erikre)

[Pobierz program Wingtip zabawki (C#)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013 dla sieci Web. Projekt Visual Studio 2013 [z C# kodem źródłowym](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny do tej serii samouczków.

W tym samouczku opisano sposób modyfikowania przykładowej aplikacji Wingtip zabawki w celu uwzględnienia autoryzacji użytkownika, rejestracji i płatności w systemie PayPal. Tylko użytkownicy, którzy są zalogowani, będą mieli autoryzację do kupowania produktów. Wbudowana funkcja rejestrowania formularzy sieci Web w programie ASP.NET 4,5 zawiera już wiele potrzebnych informacji. Dodasz funkcję wyewidencjonowania w systemie PayPal Express. W tym samouczku korzystasz ze środowiska testowania dla deweloperów w systemie PayPal, więc nie zostaną przesłane żadne rzeczywiste środki. Na końcu samouczka przetestujesz aplikację, wybierając produkty do dodania do koszyka, klikając przycisk Wyewidencjonuj i przekazując dane do witryny internetowej testowania w systemie PayPal. W witrynie internetowej testowania w systemie PayPal Potwierdź swoje informacje dotyczące wysyłki i płatności, a następnie powrócisz do lokalnej aplikacji Wingtip zabawki, aby potwierdzić i zakończyć zakup.

Istnieje kilka doświadczonych procesorów płatniczych innych firm, które są wyspecjalizowane w kupowaniu online, które zwiększają skalowalność i bezpieczeństwo. Deweloperzy ASP.NET powinni wziąć pod uwagę zalety korzystania z rozwiązania płatniczego innej firmy przed wdrożeniem rozwiązania do zakupów i kupowania.

> [!NOTE] 
> 
> Przykładowa aplikacja Wingtip zabawki została zaprojektowana w celu pokazywania określonych koncepcji i funkcji ASP.NET dostępnych dla deweloperów sieci Web ASP.NET. Ta przykładowa aplikacja nie została zoptymalizowana ze względu na skalowalność i bezpieczeństwo.

## <a name="what-youll-learn"></a>Zawartość:

- Jak ograniczyć dostęp do określonych stron w folderze.
- Jak utworzyć znany koszyk z anonimowego koszyka zakupów.
- Jak włączyć protokół SSL dla projektu.
- Jak dodać dostawcę OAuth do projektu.
- Korzystanie z systemu PayPal do kupowania produktów przy użyciu środowiska testowania systemu PayPal.
- Jak wyświetlić szczegóły z systemu PayPal w kontrolce **DetailsView** .
- Jak zaktualizować bazę danych aplikacji Wingtip zabawki z informacjami uzyskanymi w systemie PayPal.

## <a name="adding-order-tracking"></a>Dodawanie śledzenia kolejności

W tym samouczku utworzysz dwie nowe klasy służące do śledzenia danych z kolejności, w której utworzono użytkownika. Klasy będą śledzić dane dotyczące wysyłki, łącznego zakupu i potwierdzenia płatności.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Dodawanie klas Order i OrderDetail model

Wcześniej w tej serii samouczków został zdefiniowany schemat dla kategorii, produktów i elementów koszyka zakupów, tworząc klasy `Category`, `Product`i `CartItem` w folderze *modele* . Teraz dodasz dwie nowe klasy w celu zdefiniowania schematu dla zamówienia produktu i szczegółów zamówienia.

1. W folderze **modele** Dodaj nową klasę o nazwie *Order.cs*.   
   Nowy plik klasy zostanie wyświetlony w edytorze.
2. Zastąp kod domyślny następującymi:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Dodaj klasę *OrderDetail.cs* do folderu *models* .
4. Zastąp domyślny kod następującym kodem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

Klasy `Order` i `OrderDetail` zawierają schemat służący do definiowania informacji o zamówieniach używanych do kupowania i wysyłania.

Ponadto konieczne będzie zaktualizowanie klasy kontekstu bazy danych, która zarządza klasami jednostek i zapewnia dostęp do danych w bazie danych. W tym celu należy dodać nowo utworzoną i `OrderDetail` klasy modelu do klasy `ProductContext`.

1. W **Eksplorator rozwiązań**Znajdź i otwórz plik *ProductContext.cs* .
2. Dodaj wyróżniony kod do pliku *ProductContext.cs* , jak pokazano poniżej:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Jak wspomniano wcześniej w tej serii samouczków, kod w pliku *ProductContext.cs* dodaje przestrzeń nazw `System.Data.Entity`, dzięki czemu masz dostęp do wszystkich podstawowych funkcji Entity Framework. Ta funkcja obejmuje możliwość wykonywania zapytań, wstawiania, aktualizowania i usuwania danych przez pracę z silnie określonymi obiektami. Powyższy kod w klasie `ProductContext` dodaje Entity Framework dostęp do nowo dodanych klas `Order` i `OrderDetail`.

## <a name="adding-checkout-access"></a>Dodawanie dostępu do wyewidencjonowania

Przykładowa aplikacja Wingtip zabawki umożliwia anonimowym użytkownikom przeglądanie i dodawanie produktów do koszyka. Jeśli jednak użytkownicy anonimowi zdecydują się zakupić produkty dodane do koszyka, muszą zalogować się do witryny. Po zalogowaniu użytkownicy mogą uzyskiwać dostęp do stron z ograniczeniami aplikacji sieci Web, które obsługują proces wyewidencjonowania i zakupu. Te strony z ograniczeniami są zawarte w folderze *wyewidencjonowywanie* aplikacji.

### <a name="add-a-checkout-folder-and-pages"></a>Dodawanie folderu wyewidencjonowywania i stron

Teraz utworzysz folder *wyewidencjonowywania* i znajdujące się w nim strony, które klient zobaczy podczas procesu wyewidencjonowywania. Te strony zostaną zaktualizowane w dalszej części tego samouczka.

1. Kliknij prawym przyciskiem myszy nazwę projektu (**zabawki Wingtip**) w **Eksplorator rozwiązań** a następnie wybierz pozycję **Dodaj nowy folder**. 

    ![Wyewidencjonowywanie i płatność przy użyciu systemu PayPal — nowy folder](checkout-and-payment-with-paypal/_static/image1.png)
2. Nazwij nowy folder *wyewidencjonowywanie*.
3. Kliknij prawym przyciskiem myszy folder *wyewidencjonowania* , a następnie wybierz pozycję **Dodaj**-&gt;**nowy element**. 

    ![Wyewidencjonowanie i płatność przy użyciu systemu PayPal — nowy element](checkout-and-payment-with-paypal/_static/image2.png)
4. Zostanie wyświetlone okno dialogowe **Dodaj nowy element** .
5. Wybierz grupę szablonów **sieci Web** **Visual C#**  -&gt; po lewej stronie. Następnie w środkowym okienku wybierz pozycję **formularz sieci Web ze stroną wzorcową**i nadaj jej nazwę *CheckoutStart. aspx*. 

    ![Wyewidencjonowywanie i płatność przy użyciu systemu PayPal — okno dialogowe Dodawanie nowego elementu](checkout-and-payment-with-paypal/_static/image3.png)
6. Tak jak wcześniej, wybierz plik *site. Master* jako stronę wzorcową.
7. Dodaj następujące dodatkowe strony do folderu *wyewidencjonowywania* , wykonując te same kroki:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Dodaj plik Web. config

Dodając nowy plik *Web. config* do folderu *wyewidencjonowywania* , będziesz mieć możliwość ograniczenia dostępu do wszystkich stron znajdujących się w folderze.

1. Kliknij prawym przyciskiem myszy folder *wyewidencjonowywanie* i wybierz pozycję **Dodaj** -&gt; **nowy element**.  
   Zostanie wyświetlone okno dialogowe **Dodaj nowy element** .
2. Wybierz grupę szablonów **sieci Web** **Visual C#**  -&gt; po lewej stronie. Następnie w środkowym okienku wybierz pozycję **plik konfiguracji sieci Web**, zaakceptuj domyślną nazwę pliku *Web. config*, a następnie wybierz pozycję **Dodaj**.
3. Zastąp istniejącą zawartość XML w pliku *Web. config* następującym:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Zapisz plik *Web. config* .

Plik *Web. config* określa, że wszyscy nieznani użytkownicy aplikacji sieci Web muszą mieć odmowę dostępu do stron znajdujących się w folderze *wyewidencjonowywanie* . Jeśli jednak użytkownik zarejestrował konto i jest zalogowany, będzie to znany użytkownik i będzie miał dostęp do stron w folderze *wyewidencjonowywanie* .

Należy pamiętać, że konfiguracja ASP.NET jest zgodna z hierarchią, gdzie każdy plik *Web. config* stosuje ustawienia konfiguracji do folderu, w którym znajduje się w, i do wszystkich katalogów podrzędnych znajdujących się poniżej.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Włącz protokół SSL dla projektu

 SSL (SSL) to protokół zdefiniowany w celu umożliwienia bezpieczniejszej komunikacji serwerów sieci Web i klientów sieci Web przy użyciu szyfrowania. Gdy protokół SSL nie jest używany, dane wysyłane między klientem a serwerem są otwierane w celu wykrywania pakietów przez dowolną osobę mającą fizyczny dostęp do sieci. Ponadto kilka typowych schematów uwierzytelniania nie jest zabezpieczonych za pośrednictwem zwykłego protokołu HTTP. W szczególności uwierzytelnianie podstawowe i uwierzytelnianie formularzy wysyłają nieszyfrowane poświadczenia. Aby zapewnić bezpieczeństwo, te schematy uwierzytelniania muszą używać protokołu SSL. 

1. W **Eksplorator rozwiązań**kliknij projekt **WingtipToys** , a następnie naciśnij klawisz **F4** , aby wyświetlić okno **Właściwości** .
2. Zmień **włączony protokół SSL** na `true`.
3. Skopiuj **adres URL protokołu SSL** , aby można było go później użyć.   
 Adres URL protokołu SSL będzie `https://localhost:44300/`, chyba że wcześniej utworzono witryny sieci Web SSL (jak pokazano poniżej).   
    ![Właściwości projektu](checkout-and-payment-with-paypal/_static/image4.png)
4. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy projekt **WingtipToys** , a następnie kliknij pozycję **Właściwości**.
5. Na lewej karcie kliknij pozycję **Sieć Web**.
6. Zmień **adres URL projektu** tak, aby korzystał z zapisanego wcześniej **adresu URL protokołu SSL** .   
    ![właściwości sieci Web projektu](checkout-and-payment-with-paypal/_static/image5.png)
7. Zapisz stronę, naciskając **klawisze CTRL + S**.
8. Naciśnij klawisze **Ctrl+F5**, aby uruchomić aplikację. Program Visual Studio wyświetli opcję umożliwiającą uniknięcie ostrzeżeń protokołu SSL.
9. Kliknij przycisk **tak** , aby zaufać CERTYFIKATOWI protokołu SSL IIS Express i kontynuować.   
    ![IIS Express szczegóły certyfikatu protokołu SSL](checkout-and-payment-with-paypal/_static/image6.png)  
 Zostanie wyświetlone ostrzeżenie o zabezpieczeniach.
10. Kliknij przycisk **tak** , aby zainstalować certyfikat na hoście lokalnym.   
    ![okno dialogowe ostrzeżenia o zabezpieczeniach](checkout-and-payment-with-paypal/_static/image7.png)  
 Zostanie wyświetlone okno przeglądarki.

Teraz można łatwo testować aplikację sieci Web lokalnie przy użyciu protokołu SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Dodawanie dostawcy OAuth 2,0

Formularze sieci Web ASP.NET udostępniają ulepszone opcje członkostwa i uwierzytelniania. Te ulepszenia obejmują protokół OAuth. OAuth to otwarty protokół, który umożliwia bezpieczną autoryzację w prostej i standardowej metodzie z aplikacji sieci Web, mobilnych i klasycznych. Szablon formularzy sieci Web ASP.NET używa protokołu OAuth w celu udostępnienia usług Facebook, Twitter, Google i Microsoft jako dostawców uwierzytelniania. Mimo że w tym samouczku jest używany tylko firma Google jako dostawca uwierzytelniania, można łatwo zmodyfikować kod, aby użyć dowolnego dostawcy. Kroki implementowania innych dostawców są bardzo podobne do kroków opisanych w tym samouczku.

Oprócz uwierzytelniania, samouczek również będzie używać ról do implementowania autoryzacji. Tylko Ci użytkownicy dodawani do roli `canEdit` będą mogli zmieniać dane (tworzyć, edytować lub usuwać kontakty).

> [!NOTE] 
> 
> Aplikacje Windows Live akceptują tylko Live URL dla działającej witryny sieci Web, więc nie można używać adresu URL lokalnego witryny sieci Web do testowania logowania.

Poniższe kroki umożliwią dodanie dostawcy uwierzytelniania Google.

1. Otwórz plik *aplikacji\_Start\Startup.auth.cs* .
2. Usuń znaki komentarza z metody `app.UseGoogleAuthentication()`, aby Metoda była wyświetlana w następujący sposób: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Przejdź do [konsoli deweloperów firmy Google](https://console.developers.google.com/). Konieczne będzie również zalogowanie się przy użyciu konta e-mail dla deweloperów firmy Google (gmail.com). Jeśli nie masz konta Google, wybierz link **Utwórz konto** .   
   Następnie zobaczysz **konsolę deweloperów firmy Google**.   
    ![konsoli deweloperów rozwiązań firmy Google](checkout-and-payment-with-paypal/_static/image8.png)
4. Kliknij przycisk **Utwórz projekt** i wprowadź nazwę projektu i identyfikator (można użyć wartości domyślnych). Następnie kliknij **pole wyboru umowa** i przycisk **Utwórz** .  

    ![Google — nowy projekt](checkout-and-payment-with-paypal/_static/image9.png)

   W ciągu kilku sekund zostanie utworzony nowy projekt, a w przeglądarce zostanie wyświetlona strona nowe projekty.
5. Na karcie po lewej stronie kliknij pozycję **interfejsy api &amp; uwierzytelnianie**, a następnie kliknij pozycję **poświadczenia**.
6. Kliknij przycisk **Utwórz nowy identyfikator klienta** w obszarze **OAuth**.   
   Zostanie wyświetlone okno dialogowe **Tworzenie identyfikatora klienta** .   
    ![identyfikator klienta Google — Tworzenie](checkout-and-payment-with-paypal/_static/image10.png)
7. W oknie dialogowym **Tworzenie identyfikatora klienta** Zachowaj domyślną **aplikację sieci Web** dla typu aplikacji.
8. Ustaw **autoryzowane źródła kodu JavaScript** na adres URL protokołu SSL użyty wcześniej w tym samouczku (`https://localhost:44300/`, chyba że utworzono inne projekty SSL).   
   Ten adres URL jest źródłem dla aplikacji. Dla tego przykładu zostanie wprowadzony tylko adres URL testu localhost. Można jednak wprowadzić wiele adresów URL do konta lokalnego i produkcyjnego.
9. Ustaw **Autoryzowany identyfikator URI przekierowania** na następujący: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Ta wartość jest identyfikatorem URI, który ASP.NET użytkowników OAuth do komunikacji z serwerem Google OAuth. Zapamiętaj adres URL protokołu SSL użyty powyżej (`https://localhost:44300/`, chyba że utworzono inne projekty SSL).
10. Kliknij przycisk **Utwórz identyfikator klienta** .
11. W menu po lewej stronie w konsoli deweloperów firmy Google kliknij pozycję menu **ekran zgody** , a następnie ustaw adres e-mail i nazwę produktu. Po zakończeniu formularza kliknij pozycję **Zapisz**.
12. Kliknij element menu **interfejsy API** , przewiń w dół i kliknij przycisk **zniżki** obok usługi **Google + interfejs API**.   
    Zaakceptowanie tej opcji spowoduje włączenie interfejsu API Google +.
13. Należy również zaktualizować pakiet NuGet **Microsoft. Owin** do wersji 3.0.0.   
    W menu **Narzędzia** wybierz pozycję **Menedżer pakietów NuGet** , a następnie wybierz pozycję **Zarządzaj pakietami NuGet dla rozwiązania**.  
    W oknie **Zarządzanie pakietami NuGet** Znajdź i zaktualizuj pakiet **Microsoft. Owin** do wersji 3.0.0.
14. W programie Visual Studio zaktualizuj metodę `UseGoogleAuthentication` strony *Startup.auth.cs* , kopiując i wklejając **Identyfikator klienta** i **klucz tajny klienta** do metody. Wartości **Identyfikator klienta** i **klucz tajny klienta** podane poniżej są przykładowe i nie będą działały. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Naciśnij **kombinację klawiszy CTRL + F5** , aby skompilować i uruchomić aplikację. Kliknij link **Zaloguj** .
16. W obszarze **Użyj innej usługi do zalogowania**się kliknij pozycję **Google**.  
    ![Zaloguj się](checkout-and-payment-with-paypal/_static/image11.png)
17. Jeśli musisz wprowadzić swoje poświadczenia, nastąpi przekierowanie do witryny Google, w której będą wprowadzane poświadczenia.  
    ![Google — logowanie](checkout-and-payment-with-paypal/_static/image12.png)
18. Po wprowadzeniu poświadczeń zostanie wyświetlony monit o przyznanie uprawnień aplikacji sieci Web, która została właśnie utworzona.  
    ![Domyślne konto usługi projektu](checkout-and-payment-with-paypal/_static/image13.png)
19. Kliknij przycisk **Akceptuj**. Nastąpi przekierowanie z powrotem do strony **rejestracji** w aplikacji **WingtipToys** , w której można zarejestrować swoje konto Google.  
    ![zarejestrować się przy użyciu konta Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Istnieje możliwość zmiany nazwy rejestracji lokalnej poczty e-mail używanej na potrzeby konta usługi Gmail, ale zazwyczaj chcesz zachować domyślny alias poczty e-mail (czyli ten, który został użyty do uwierzytelniania). Kliknij pozycję **Zaloguj się** , jak pokazano powyżej.

### <a name="modifying-login-functionality"></a>Modyfikowanie funkcji logowania

Jak wspomniano wcześniej w tej serii samouczków, większość funkcji rejestracji użytkowników została zawarta domyślnie w szablonie ASP.NET Web Forms. Teraz zmodyfikujesz domyślne strony *login. aspx* i *register. aspx* , aby wywołać metodę `MigrateCart`. Metoda `MigrateCart` kojarzy nowo zalogowanego użytkownika z anonimowym koszykiem zakupów. Dzięki skojarzeniu użytkownika i koszyka Wingtip zabawki Przykładowa aplikacja może zachować koszyk użytkownika między wizytami.

1. W **Eksplorator rozwiązań**Znajdź i Otwórz folder *konta* .
2. Zmodyfikuj stronę z kodem o nazwie *login.aspx.cs* , aby obejmowała kod wyróżniony kolorem żółtym, tak aby pojawił się w następujący sposób:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Zapisz plik *login.aspx.cs* .

Na razie możesz zignorować ostrzeżenie, że nie ma definicji metody `MigrateCart`. Ten samouczek zostanie dodany później.

Plik związany z kodem *login.aspx.cs* obsługuje metodę logowania. Sprawdzając stronę login. aspx, zobaczysz, że ta strona zawiera przycisk Zaloguj się, który po kliknięciu wyzwala procedurę obsługi `LogIn` w kodzie.

Gdy wywoływana jest metoda `Login` na *login.aspx.cs* , zostanie utworzone nowe wystąpienie koszyka zakupów o nazwie `usersShoppingCart`. Identyfikator koszyka (GUID) jest pobierany i ustawiany na zmienną `cartId`. Następnie zostanie wywołana metoda `MigrateCart`, przekazanie zarówno `cartId`, jak i nazwy zalogowanego użytkownika do tej metody. Po przeprowadzeniu migracji koszyka identyfikator GUID używany do identyfikacji koszyka anonimowego jest zastępowany nazwą użytkownika.

Oprócz modyfikacji pliku związanego z kodem *login.aspx.cs* w celu przeprowadzenia migracji koszyka, gdy użytkownik się zaloguje, należy również zmodyfikować *plik związany z kodem register.aspx.cs* w celu przeprowadzenia migracji koszyka, gdy użytkownik tworzy nowe konto i loguje się do niego.

1. W folderze *konto* Otwórz plik związany z kodem o nazwie *register.aspx.cs*.
2. Zmodyfikuj plik związany z kodem, dołączając kod żółty, tak aby pojawił się w następujący sposób:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Zapisz plik *register.aspx.cs* . Po ponownym uruchomieniu zignoruj ostrzeżenie o metodzie `MigrateCart`.

Zwróć uwagę, że kod użyty w obsłudze zdarzeń `CreateUser_Click` jest bardzo podobny do kodu użytego w `LogIn` metodzie. Gdy użytkownik rejestruje lub loguje się do witryny, zostanie wykonane wywołanie metody `MigrateCart`.

## <a name="migrating-the-shopping-cart"></a>Migrowanie koszyka

Teraz, gdy masz zaktualizowany proces logowania i rejestracji, możesz dodać kod w celu przeprowadzenia migracji koszyka przy użyciu metody `MigrateCart`.

1. W **Eksplorator rozwiązań**Znajdź folder *Logic* i Otwórz plik klasy *ShoppingCartActions.cs* .
2. Dodaj kod wyróżniony żółtym do istniejącego kodu w pliku *ShoppingCartActions.cs* , tak aby kod w pliku *ShoppingCartActions.cs* pojawiał się w następujący sposób:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

Metoda `MigrateCart` używa istniejącej cartId, aby znaleźć koszyk dla użytkownika. Następnie kod przechodzi przez wszystkie elementy koszyka zakupów i zastępuje właściwość `CartId` (określoną przez schemat `CartItem`) nazwą zalogowanego użytkownika.

### <a name="updating-the-database-connection"></a>Aktualizowanie połączenia z bazą danych

Jeśli korzystasz z tego samouczka przy użyciu **wstępnie skompilowanej** przykładowej aplikacji Wingtip zabawki, musisz ponownie utworzyć domyślną bazę danych członkostwa. Modyfikując domyślne parametry połączenia, baza danych członkostwa zostanie utworzona przy następnym uruchomieniu aplikacji.

1. Otwórz plik *Web. config* w katalogu głównym projektu.
2. Zaktualizuj domyślne parametry połączenia tak, aby były wyświetlane w następujący sposób:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Integrowanie systemu PayPal

System PayPal jest platformą rozliczeniową opartą na sieci Web, która akceptuje płatności od handlowców online. W tym samouczku wyjaśniono, jak zintegrować funkcje ekspresowe wyewidencjonowania w systemie PayPal z Twoją aplikacją. Ekspresowe wyewidencjonowanie umożliwia klientom korzystanie z systemu PayPal do płacenia za elementy, które zostały dodane do koszyka zakupów.

### <a name="create-paypal-test-accounts"></a>Tworzenie kont testów w systemie PayPal

Aby użyć środowiska testowania w systemie PayPal, należy utworzyć i zweryfikować konto testowe dewelopera. Do utworzenia konta testowego nabywcy i konta testowego sprzedawcy zostanie użyte konto test dla deweloperów. Poświadczenia konta testowego dla deweloperów umożliwiają również dostęp do środowiska testowego w programie Wingtip.

1. W przeglądarce przejdź do witryny testowania dla deweloperów systemu PayPal:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Jeśli nie masz konta dewelopera w systemie PayPal, Utwórz nowe konto, klikając pozycję **zarejestruj się**i postępując zgodnie z krokami rejestracji. Jeśli masz istniejące konto dewelopera w systemie PayPal, zaloguj się, klikając przycisk **Zaloguj**. Do przetestowania przykładowej aplikacji Wingtip zabawki w dalszej części tego samouczka potrzebne jest konto dewelopera systemu PayPal.
3. Jeśli masz już konto dla deweloperów systemu PayPal, może być konieczne zweryfikowanie konta dewelopera systemu PayPal w systemie PayPal. Aby zweryfikować swoje konto, wykonaj czynności wysyłane przez system PayPal na konto e-mail. Po zweryfikowaniu konta dewelopera w systemie PayPal Zaloguj się ponownie do witryny testowania dla deweloperów w systemie PayPal.
4. Po zalogowaniu się do witryny dewelopera systemu PayPal przy użyciu konta dewelopera systemu PayPal musisz utworzyć konto testowe kupującego w systemie PayPal, jeśli jeszcze go nie masz. Aby utworzyć konto testowe dla kupującego, w witrynie PayPal kliknij kartę **aplikacje** , a następnie kliknij pozycję **konta piaskownicy**.   
 Zostanie wyświetlona strona **konta testów piaskownicy** .   

    > [!NOTE] 
    > 
    > Witryna dewelopera systemu PayPal już udostępnia konto testowe sprzedawcy.

    ![Wyewidencjonowywanie i płatność dzięki kontom testowym w systemie PayPal](checkout-and-payment-with-paypal/_static/image15.png)
5. Na stronie konta testów piaskownicy kliknij pozycję **Utwórz konto**.
6. Na stronie **Tworzenie konta testowego** wybierz adres e-mail konta testu kupującego i wybrane hasło.   

    > [!NOTE] 
    > 
    > Do przetestowania przykładowej aplikacji Wingtip zabawki na końcu tego samouczka będą potrzebne adresy e-mail i hasło kupującego.

    ![Wyewidencjonowywanie i płatność dzięki kontom testowym w systemie PayPal](checkout-and-payment-with-paypal/_static/image16.png)
7. Utwórz konto testowe dla kupującego, klikając przycisk **Utwórz konto** .  
 Zostanie wyświetlona strona **konta testów piaskownicy** . 

    ![Wyewidencjonowywanie i płatność przy użyciu systemu PayPal — konta PayPal](checkout-and-payment-with-paypal/_static/image17.png)
8. Na stronie **konta testów piaskownicy** kliknij konto z **ułatwieniami** do obsługi poczty e-mail.  
    Zostaną wyświetlone opcje **profilu** i **powiadomień** .
9. Wybierz opcję **profilu** , a następnie kliknij pozycję **poświadczenia interfejsu API** , aby wyświetlić poświadczenia interfejsu API dla konta testowego handlowca.
10. Skopiuj poświadczenia interfejsu API testowania do Notatnika.

Do środowiska testowania w systemie PayPal będą potrzebne wyświetlane poświadczenia klasycznego interfejsu API (username, Password i Signature). Poświadczenia zostaną dodane w następnym kroku.

### <a name="add-paypal-class-and-api-credentials"></a>Dodawanie klasy i poświadczeń interfejsu API systemu PayPal

Większość kodu PayPal zostanie umieszczona w jednej klasie. Ta klasa zawiera metody używane do komunikacji z usługą PayPal. Ponadto do tej klasy należy dodać poświadczenia systemu PayPal.

1. W przykładowej aplikacji Wingtip zabawki w programie Visual Studio kliknij prawym przyciskiem myszy folder **logiki** , a następnie wybierz pozycję **Dodaj** -&gt; **nowy element**.   
   Zostanie wyświetlone okno dialogowe **Dodaj nowy element** .
2. W **obszarze C# Wizualizacja** z **zainstalowanego** okienka po lewej stronie wybierz pozycję **kod**.
3. W środkowym okienku wybierz pozycję **Klasa**. Nadaj tej nowej klasie nazwę **PayPalFunctions.cs**.
4. Kliknij pozycję **Add** (Dodaj).  
   Nowy plik klasy zostanie wyświetlony w edytorze.
5. Zastąp domyślny kod następującym kodem:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Dodaj poświadczenia interfejsu API handlowca (username, Password i Signature), które zostały wcześniej wyświetlone w tym samouczku, aby umożliwić wywoływanie funkcji w środowisku testowania w systemie PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> W tej przykładowej aplikacji dodawane są tylko poświadczenia do C# pliku (. cs). Jednak w zaimplementowanym rozwiązaniu należy rozważyć szyfrowanie poświadczeń w pliku konfiguracji.

Klasa NVPAPICaller zawiera większość funkcji systemu PayPal. Kod w klasie zawiera metody, które są konieczne do dokonania zakupu testowego w środowisku testowania systemu PayPal. Następujące trzy funkcje systemu PayPal są używane do dokonywania zakupów:

- Funkcja `SetExpressCheckout`
- Funkcja `GetExpressCheckoutDetails`
- Funkcja `DoExpressCheckoutPayment`

Metoda `ShortcutExpressCheckout` zbiera informacje o zakupie testów oraz szczegóły produktu z koszyka zakupów i wywołuje funkcję usługi `SetExpressCheckout` w systemie PayPal. Metoda `GetCheckoutDetails` potwierdza szczegóły zakupu i wywołuje funkcję usługi `GetExpressCheckoutDetails` w systemie PayPal przed dokonaniem zakupu testu. Metoda `DoCheckoutPayment` wykonuje zakup testu w środowisku testowym, wywołując funkcję usługi `DoExpressCheckoutPayment` w systemie PayPal. Pozostały kod obsługuje metody i procesy w systemie PayPal, takie jak ciągi kodowania, dekodowania ciągów, przetwarzanie tablic i Określanie poświadczeń.

> [!NOTE] 
> 
> System PayPal pozwala dołączać opcjonalne szczegóły zakupu w oparciu o [specyfikację interfejsu API systemu PayPal](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Rozszerzając kod w przykładowej aplikacji Wingtip zabawki, można uwzględnić szczegóły lokalizacji, opisy produktów, podatek, numer usługi klienta, a także wiele innych pól opcjonalnych.

Zwróć uwagę, że adresy URL Return i Cancel określone w metodzie **ShortcutExpressCheckout** używają numeru portu.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Gdy Visual Web Developer uruchamia projekt sieci Web przy użyciu protokołu SSL, zazwyczaj port 44300 jest używany dla serwera sieci Web. Jak pokazano powyżej, numer portu to 44300. Po uruchomieniu aplikacji zobaczysz inny numer portu. Numer portu musi być poprawnie ustawiony w kodzie, aby można było pomyślnie uruchomić przykładową aplikację Wingtip zabawki na końcu tego samouczka. W następnej sekcji tego samouczka wyjaśniono, jak pobrać numer portu hosta lokalnego i zaktualizować klasę systemu PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Aktualizowanie numeru portu LocalHost w klasie PayPal

Przykładowa aplikacja Wingtip zabawki kupuje produkty, przechodząc do witryny testowania w systemie PayPal i zwracając do lokalnego wystąpienia aplikacji przykładowej programu Wingtip zabawki. Aby w systemie PayPal wrócić do poprawnego adresu URL, należy określić numer portu uruchomionej lokalnie aplikacji przykładowej w kodzie w systemie PayPal wymienionym powyżej.

1. Kliknij prawym przyciskiem myszy nazwę projektu (**WingtipToys**) w **Eksplorator rozwiązań** a następnie wybierz pozycję **Właściwości**.
2. W lewej kolumnie Wybierz kartę **Sieć Web** .
3. Pobierz numer portu z pola **adres URL projektu** .
4. W razie potrzeby zaktualizuj `returnURL` i `cancelURL` w klasie systemu PayPal (`NVPAPICaller`) w pliku *PayPalFunctions.cs* , aby użyć numeru portu aplikacji sieci Web:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

Teraz kod, który został dodany, będzie zgodny z oczekiwanym portem dla lokalnej aplikacji sieci Web. W systemie PayPal będzie można wrócić do poprawnego adresu URL na komputerze lokalnym.

### <a name="add-the-paypal-checkout-button"></a>Dodaj przycisk Wyewidencjonuj w systemie PayPal

Teraz, gdy podstawowe funkcje systemu PayPal zostały dodane do przykładowej aplikacji, możesz rozpocząć dodawanie znaczników i kodu potrzebnego do wywołania tych funkcji. Najpierw należy dodać przycisk wyewidencjonowania, który użytkownik zobaczy na stronie koszyka zakupów.

1. Otwórz plik *ShoppingCart. aspx* .
2. Przewiń w dół pliku i Znajdź `<!--Checkout Placeholder -->` komentarz.
3. Zastąp komentarz formantem `ImageButton`, aby znacznik został zastąpiony w następujący sposób:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. W pliku *ShoppingCart.aspx.cs* , po obsłudze zdarzeń `UpdateBtn_Click` w końcu pliku, Dodaj program obsługi zdarzeń `CheckOutBtn_Click`:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Również w pliku *ShoppingCart.aspx.cs* Dodaj odwołanie do `CheckoutBtn`, aby przycisk Nowy obraz został przywoływany w następujący sposób:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Zapisz zmiany zarówno w pliku *ShoppingCart. aspx* , jak i w pliku *ShoppingCart.aspx.cs* .
7. Z menu wybierz kolejno opcje **debuguj**-&gt;**Build WingtipToys**.  
   Projekt zostanie odbudowany przy użyciu nowo dodanej kontrolki **kliknięto element ImageButton** .

### <a name="send-purchase-details-to-paypal"></a>Wyślij szczegóły zakupu do systemu PayPal

Gdy użytkownik kliknie przycisk **Wyewidencjonuj** na stronie koszyka zakupów (*ShoppingCart. aspx*), rozpocznie proces zakupu. Poniższy kod wywołuje pierwszą funkcję systemu PayPal, która jest wymagana do zakupu produktów.

1. W folderze *wyewidencjonowywanie* Otwórz plik związany z kodem o nazwie *CheckoutStart.aspx.cs*.   
   Pamiętaj, aby otworzyć plik związany z kodem.
2. Zastąp istniejący kod następującym:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Gdy użytkownik aplikacji kliknie przycisk **Wyewidencjonuj** na stronie koszyka zakupów, przeglądarka przejdzie do strony *CheckoutStart. aspx* . Po załadowaniu strony *CheckoutStart. aspx* wywoływana jest metoda `ShortcutExpressCheckout`. W tym momencie użytkownik jest przenoszony do witryny internetowej do testowania w systemie PayPal. W witrynie systemu PayPal użytkownik wprowadza swoje poświadczenia w systemie PayPal, przegląda szczegóły zakupu, akceptuje Umowę PayPal i powraca do przykładowej aplikacji Wingtip zabawki, w której zostanie ukończona Metoda `ShortcutExpressCheckout`. Po zakończeniu `ShortcutExpressCheckout` Metoda przekieruje użytkownika na stronę *CheckoutReview. aspx* określoną w `ShortcutExpressCheckout` metodzie. Dzięki temu użytkownik może przejrzeć szczegóły zamówienia z poziomu przykładowej aplikacji Wingtip zabawki.

### <a name="review-order-details"></a>Przejrzyj szczegóły zamówienia

Po powrocie z systemu PayPal strona *CheckoutReview. aspx* w przykładowej aplikacji programu Wingtip zabawki wyświetla szczegóły zamówienia. Ta strona umożliwia użytkownikowi przejrzenie szczegółów zamówienia przed zakupem produktów. Stronę *CheckoutReview. aspx* należy utworzyć w następujący sposób:

1. W folderze *wyewidencjonowywanie* Otwórz stronę o nazwie *CheckoutReview. aspx*.
2. Zastąp istniejący znacznik następującym:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Otwórz stronę z kodem o nazwie *CheckoutReview.aspx.cs* i Zastąp istniejący kod następującym kodem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

Formant **DetailsView** służy do wyświetlania szczegółów zamówienia, które zostały zwrócone w systemie PayPal. Ponadto powyższy kod zapisuje szczegóły zamówienia do bazy danych Wingtip zabawki jako obiekt `OrderDetail`. Gdy użytkownik kliknie przycisk **Ukończ zamówienie** , zostanie przekierowany do strony *CheckoutComplete. aspx* .

> [!NOTE] 
> 
> **Porada**
> 
> W znaczniku strony *CheckoutReview. aspx* Zwróć uwagę, że tag `<ItemStyle>` jest używany do zmiany stylu elementów w kontrolce **DetailsView** w dolnej części strony. Wyświetlając stronę w **widoku projektu** (wybierając opcję **projekt** w lewym dolnym rogu programu Visual Studio), a następnie wybierając formant **DetailsView** i wybierając **tag inteligentny** (ikona strzałki w prawej górnej części kontrolki), będzie można zobaczyć **zadania DetailsView**.
> 
> ![Wyewidencjonowywanie i płatność w systemie PayPal — Edytuj pola](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Po wybraniu opcji **Edytuj pola**zostanie wyświetlone okno dialogowe **pola** . W tym oknie dialogowym można łatwo sterować właściwościami wizualizacji, takimi jak **element Items**formantu **DetailsView** .
> 
> ![Wyewidencjonowywanie i płatność przy użyciu systemu PayPal — okno dialogowe](checkout-and-payment-with-paypal/_static/image19.png)

### <a name="complete-purchase"></a>Ukończ zakup

Strona *CheckoutComplete. aspx* dokonuje zakupu w systemie PayPal. Jak wspomniano powyżej, użytkownik musi kliknąć przycisk **Ukończ zamówienie** , aby aplikacja przejdzie do strony *CheckoutComplete. aspx* .

1. W folderze *wyewidencjonowywanie* Otwórz stronę o nazwie *CheckoutComplete. aspx*.
2. Zastąp istniejący znacznik następującym:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Otwórz stronę z kodem o nazwie *CheckoutComplete.aspx.cs* i Zastąp istniejący kod następującym kodem:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Po załadowaniu strony *CheckoutComplete. aspx* wywoływana jest metoda `DoCheckoutPayment`. Jak wspomniano wcześniej, Metoda `DoCheckoutPayment` uzupełnia zakup ze środowiska testowania systemu PayPal. Gdy w systemie PayPal zakończysz zakup zamówienia, na stronie *CheckoutComplete. aspx* zostanie wyświetlona transakcja płatności `ID` do zakupu.

### <a name="handle-cancel-purchase"></a>Obsługa anulowania zakupu

Jeśli użytkownik zdecyduje się zrezygnować z zakupu, zostanie skierowany do strony *CheckoutCancel. aspx* , gdzie zobaczy, że ich zamówienie zostało anulowane.

1. Otwórz stronę o nazwie *CheckoutCancel. aspx* w folderze *wyewidencjonowywanie* .
2. Zastąp istniejący znacznik następującym:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Obsługa błędów zakupu

Błędy w trakcie zakupu będą obsługiwane przez stronę *CheckoutError. aspx* . Kod związany ze stroną *CheckoutStart. aspx* na stronie *CheckoutReview. aspx* i *CheckoutComplete. aspx* zostanie przekierowany na stronę *CheckoutError. aspx* w przypadku wystąpienia błędu.

1. Otwórz stronę o nazwie *CheckoutError. aspx* w folderze *wyewidencjonowywanie* .
2. Zastąp istniejący znacznik następującym:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

Zostanie wyświetlona strona *CheckoutError. aspx* z informacjami o błędzie w przypadku wystąpienia błędu podczas procesu wyewidencjonowywania.

## <a name="running-the-application"></a>Uruchamianie aplikacji

Uruchom aplikację, aby dowiedzieć się, jak kupić produkty. Należy pamiętać, że w środowisku testowym systemu PayPal zostanie uruchomiony program. Nie są wymieniane rzeczywiste pieniądze.

1. Upewnij się, że wszystkie pliki zostały zapisane w programie Visual Studio.
2. Otwórz przeglądarkę internetową i przejdź do [https://developer.paypal.com](https://developer.paypal.com/).
3. Zaloguj się przy użyciu konta dewelopera systemu PayPal utworzonego wcześniej w tym samouczku.  
   W przypadku piaskownicy dla deweloperów w systemie PayPal musisz zalogować się w [https://developer.paypal.com](https://developer.paypal.com/) , aby przetestować wyewidencjonowanie. Dotyczy to wyłącznie testów piaskownicy w systemie PayPal, a nie do środowiska na żywo w systemie PayPal.
4. W programie Visual Studio naciśnij klawisz **F5** , aby uruchomić przykładową aplikację Wingtip zabawki.  
   Po ponownym skompilowaniu bazy danych przeglądarka zostanie otwarta i zostanie wyświetlona strona *default. aspx* .
5. Dodaj trzy różne produkty do koszyka zakupów, wybierając kategorię produktu, na przykład "samochody", a następnie klikając pozycję **Dodaj do koszyka** obok każdego produktu.  
   W koszyku zostanie wyświetlony wybrany produkt.
6. Kliknij przycisk **PayPal** , aby przeprowadzić wyewidencjonowanie. 

    ![Wyewidencjonowanie i płatność za pomocą usługi PayPal — koszyk](checkout-and-payment-with-paypal/_static/image20.png)

   Wyewidencjonowanie będzie wymagało posiadania konta użytkownika dla przykładowej aplikacji Wingtip zabawki.
7. Kliknij link **Google** po prawej stronie, aby zalogować się przy użyciu istniejącego konta e-mail gmail.com.  
   Jeśli nie masz konta usługi gmail.com, możesz je utworzyć na potrzeby testowania w witrynie [www.gmail.com](https://www.gmail.com/). Możesz również użyć standardowego konta lokalnego, klikając pozycję "Zarejestruj". 

    ![Wyewidencjonowywanie i płatność przy użyciu systemu PayPal — logowanie](checkout-and-payment-with-paypal/_static/image21.png)
8. Zaloguj się przy użyciu konta i hasła usługi Gmail. 

    ![Wyewidencjonowywanie i płatność przy użyciu systemu PayPal — logowanie do usługi Gmail](checkout-and-payment-with-paypal/_static/image22.png)
9. Kliknij przycisk **Zaloguj** się, aby zarejestrować konto usługi Gmail przy użyciu przykładowej nazwy użytkownika aplikacji Wingtip zabawki. 

    ![Wyewidencjonowanie i płatność przy użyciu systemu PayPal — rejestrowanie konta](checkout-and-payment-with-paypal/_static/image23.png)
10. W witrynie testowej w systemie PayPal Dodaj swój adres e-mail i hasło dla swojego **kupca** , które zostały wcześniej utworzone w tym samouczku, a następnie kliknij przycisk **Zaloguj** się. 

    ![Wyewidencjonowywanie i płatność przy użyciu systemu PayPal — logowanie do systemu PayPal](checkout-and-payment-with-paypal/_static/image24.png)
11. Zaakceptuj zasady systemu PayPal i kliknij przycisk **Zgadzam się i Kontynuuj** .  
    Należy pamiętać, że ta strona jest wyświetlana tylko podczas pierwszego korzystania z tego konta w systemie PayPal. Należy pamiętać, że to konto testowe nie ma żadnych rzeczywistych pieniędzy. 

    ![Wyewidencjonowywanie i płatność przy użyciu systemu PayPal — zasady systemu PayPal](checkout-and-payment-with-paypal/_static/image25.png)
12. Przejrzyj informacje o zamówieniu na stronie Przegląd środowiska testowania w systemie PayPal i kliknij przycisk **Kontynuuj**. 

    ![Wyewidencjonowywanie i płatność przy użyciu systemu PayPal — informacje o przeglądzie](checkout-and-payment-with-paypal/_static/image26.png)
13. Na stronie *CheckoutReview. aspx* Sprawdź kwotę zamówienia i Wyświetl wygenerowany adres wysyłkowy. Następnie kliknij przycisk **Ukończ zamówienie** . 

    ![Wyewidencjonowywanie i płatność przy użyciu systemu PayPal — Przegląd zamówień](checkout-and-payment-with-paypal/_static/image27.png)
14. Zostanie wyświetlona strona **CheckoutComplete. aspx** z identyfikatorem transakcji płatności. 

    ![Wyewidencjonowywanie i płatność przy użyciu systemu PayPal — realizacja wyewidencjonowania](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Przeglądanie bazy danych

Przeglądając zaktualizowane dane w przykładowej bazie danych aplikacji Wingtip zabawki po uruchomieniu aplikacji, można zobaczyć, że aplikacja pomyślnie zarejestrowała zakup produktów.

Dane znajdujące się w pliku bazy danych *wingtiptoys. mdf* można sprawdzić przy użyciu okna **Eksplorator bazy danych** (**Eksplorator serwera** okno w programie Visual Studio) tak jak wcześniej w tej serii samouczków.

1. Zamknij okno przeglądarki, jeśli jest nadal otwarte.
2. W programie Visual Studio wybierz ikonę **Pokaż wszystkie pliki** w górnej części **Eksplorator rozwiązań** , aby umożliwić rozszerzenie folderu **dane\_aplikacji** .
3. Rozwiń folder **dane\_aplikacji** .  
 Może być konieczne wybranie ikony **Pokaż wszystkie pliki** dla folderu.
4. Kliknij prawym przyciskiem myszy plik bazy danych *wingtiptoys. mdf* i wybierz polecenie **Otwórz**.  
    Zostanie wyświetlona **Eksplorator serwera** .
5. Rozwiń folder **tabele** .
6. Kliknij prawym przyciskiem myszy tabelę **Orders (zamówienia**) i wybierz polecenie **Pokaż dane tabeli**.  
 Zostanie wyświetlona tabela **Orders (zamówienia** ).
7. Przejrzyj kolumnę **PaymentTransactionID** , aby potwierdzić pomyślne transakcje. 

    ![Wyewidencjonowywanie i płatność przy użyciu systemu PayPal — przegląd bazy danych](checkout-and-payment-with-paypal/_static/image29.png)
8. Zamknij okno tabela **zamówień** .
9. W Eksplorator serwera kliknij prawym przyciskiem myszy tabelę **OrderDetails** , a następnie wybierz polecenie **Pokaż dane tabeli**.
10. Przejrzyj wartości `OrderId` i `Username` w tabeli **OrderDetails** . Należy zauważyć, że te wartości pasują do `OrderId` i `Username` wartości zawartych w tabeli **Orders** .
11. Zamknij okno tabeli **OrderDetails** .
12. Kliknij prawym przyciskiem myszy plik bazy danych Wingtip zabawki (*wingtiptoys. mdf*) i wybierz pozycję **Zamknij połączenie**.
13. Jeśli nie widzisz okna **Eksplorator rozwiązań** , kliknij przycisk **Eksplorator rozwiązań** u dołu okna **Eksplorator serwera** , aby ponownie wyświetlić **Eksplorator rozwiązań** .

## <a name="summary"></a>Podsumowanie

W tym samouczku dodano schematy Order i Order detail do śledzenia zakupów produktów. Funkcje systemu PayPal można także zintegrować z przykładową aplikacją Wingtip zabawki.

## <a name="additional-resources"></a>Dodatkowe materiały

[Przegląd konfiguracji ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Wdróż bezpieczną aplikację ASP.NET Web Forms z członkostwem, uwierzytelnianiem OAuth i SQL Database, aby Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure — bezpłatna wersja próbna](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Zastrzeżenie

Ten samouczek zawiera przykładowy kod. Taki przykładowy kod jest dostarczany "w takiej postaci, w jakiej jest" bez gwarancji jakiegokolwiek rodzaju. W związku z tym firma Microsoft nie gwarantuje dokładności, integralności lub jakości przykładowego kodu. Użytkownik wyraża zgodę na użycie przykładowego kodu na własne ryzyko. W żadnym wypadku firma Microsoft nie ponosi odpowiedzialności za każdy kod przykładowy, zawartość, łącznie z, ale nie tylko, wszelkie błędy lub pominięcia w kodzie przykładowym, treści lub wszelkich strat lub szkód dowolnego rodzaju wynikającego z użycia dowolnego przykładowego kodu. Użytkownik jest powiadamiany i jest wyrażany zgodą na wszelkiej odpowiedzialności, oszczędność i utrzymywanie firmy Microsoft niegroźnej z i za wszelką utratę, szkody związane z utratą, uszkodzeniem lub zniszczeniem jakiegokolwiek rodzaju, w tym, bez ograniczeń, te, które zostały wprowadzone przez użytkownika lub pochodzące z materiałów, które publikują, Przesyłaj, wykorzystuj lub Polegaj na dołączaniu, ale bez ograniczeń do, widoków wyrażonych w tym obszarze.

> [!div class="step-by-step"]
> [Poprzednie](shopping-cart.md)
> [dalej](membership-and-administration.md)

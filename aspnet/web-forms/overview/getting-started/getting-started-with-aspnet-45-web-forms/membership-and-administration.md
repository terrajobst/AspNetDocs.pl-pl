---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Członkostwo i administracja | Microsoft Docs
author: Erikre
description: Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: ab00bc90bfc767d06e747be6dfb973245b5aae88
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78546740"
---
# <a name="membership-and-administration"></a>Członkostwo i administracja

Autor [Erik Reitan](https://github.com/Erikre)

[Pobierz program Wingtip zabawki (C#)](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) lub [Pobierz książkę elektroniczną (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Ta seria samouczków zawiera informacje na temat tworzenia aplikacji ASP.NET Web Forms przy użyciu ASP.NET 4,5 i Microsoft Visual Studio Express 2013 dla sieci Web. Projekt Visual Studio 2013 [z C# kodem źródłowym](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) jest dostępny do tej serii samouczków.

W tym samouczku pokazano, jak zaktualizować przykładową aplikację Wingtip zabawki, aby dodać rolę niestandardową i użyć ASP.NET Identity. Przedstawiono w nim również sposób implementacji strony administracyjnej, z której użytkownik mający rolę niestandardową może dodawać i usuwać produkty z witryny sieci Web.

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) to system członkostwa używany do kompilowania aplikacji sieci Web ASP.NET i jest dostępny w ASP.NET 4,5. ASP.NET Identity jest używany w szablonie projektu Visual Studio 2013 Web Forms, a także szablony dla [ASP.NET MVC](../../../../mvc/index.md), [ASP.NET Web API](../../../../web-api/index.md)i [ASP.NET aplikacji jednostronicowej](../../../../single-page-application/index.md). Możesz również zainstalować system ASP.NET Identity przy użyciu narzędzia NuGet, gdy zaczniesz od pustej aplikacji sieci Web. Jednak w tej serii samouczków używasz ProjectTemplate **formularzy sieci Web**, który zawiera system ASP.NET Identity. ASP.NET Identity ułatwia integrację danych profilu specyficznych dla użytkownika z danymi aplikacji. Ponadto ASP.NET Identity umożliwia wybranie modelu trwałości dla profilów użytkowników w aplikacji. Dane można przechowywać w bazie danych SQL Server lub w innym magazynie danych, w tym z magazynami danych *NoSQL* , takimi jak tabele magazynu systemu Windows Azure.

Ten samouczek jest oparty na poprzednim samouczku zatytułowanym "Wyewidencjonowywanie i płatność w systemie PayPal" w serii samouczków Wingtip.

## <a name="what-youll-learn"></a>Zawartość:

- Jak dodać rolę niestandardową i użytkownika do aplikacji przy użyciu kodu.
- Jak ograniczyć dostęp do folderu administracyjnego i strony.
- Jak zapewnić nawigację dla użytkownika należącego do roli niestandardowej.
- Jak użyć powiązania modelu, aby wypełnić formant [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) z kategoriami produktów.
- Jak przekazać plik do aplikacji sieci Web za pomocą formantu [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) .
- Jak zaimplementować sprawdzanie poprawności danych wejściowych przy użyciu kontrolek walidacji.
- Jak dodawać i usuwać produkty z aplikacji.

## <a name="these-features-are-included-in-the-tutorial"></a>Te funkcje są zawarte w samouczku:

- ASP.NET Identity
- Konfiguracja i autoryzacja
- Powiązanie modelu
- Niezauważalna weryfikacja

ASP.NET Web Forms zapewnia możliwość członkostwa. Przy użyciu szablonu domyślnego masz wbudowaną funkcję członkostwa, która może być używana natychmiast po uruchomieniu aplikacji. W tym samouczku pokazano, jak za pomocą ASP.NET Identity dodawać rolę niestandardową i przypisywać użytkownika do tej roli. Dowiesz się, jak ograniczyć dostęp do folderu administracyjnego. Dodasz stronę do folderu Administracja, która umożliwia użytkownikowi z rolą niestandardową Dodawanie i usuwanie produktów oraz wyświetlanie podglądu produktu po jego dodaniu.

## <a name="adding-a-custom-role"></a>Dodawanie roli niestandardowej

Za pomocą ASP.NET Identity można dodać rolę niestandardową i przypisać użytkownika do tej roli przy użyciu kodu.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *logiki* i Utwórz nową klasę.
2. Nadaj nowej klasie nazwę *RoleActions.cs*.
3. Zmodyfikuj kod, tak aby pojawił się w następujący sposób:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. W **Eksplorator rozwiązań**otwórz plik *Global.asax.cs* .
5. Zmodyfikuj plik *Global.asax.cs* , dodając kod wyróżniony kolorem żółtym, aby pojawił się w następujący sposób:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Zwróć uwagę, że `AddUserAndRole` jest podkreślony na czerwono. Kliknij dwukrotnie kod AddUserAndRole.  
   Litera "A" na początku wyróżnionej metody zostanie podkreślona.
7. Umieść kursor nad literą "A" i kliknij interfejs użytkownika, który umożliwia wygenerowanie klasy zastępczej metody `AddUserAndRole`. 

    ![Członkostwo i administracja — generowanie zastępcze metody](membership-and-administration/_static/image1.png)
8. Kliknij opcję zatytułowaną:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Otwórz plik *RoleActions.cs* z folderu *Logic* .  
   Metoda `AddUserAndRole` została dodana do pliku klasy.
10. Zmodyfikuj plik *RoleActions.cs* , usuwając `NotImplementedException` i dodając kod wyróżniony kolorem żółtym, tak aby pojawił się w następujący sposób:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Powyższy kod najpierw ustanawia kontekst bazy danych dla bazy danych członkostwa. Baza danych członkostwa jest również przechowywana jako plik *MDF* w folderze *danych\_aplikacji* . Będzie można wyświetlić tę bazę danych, gdy pierwszy użytkownik zaloguje się do tej aplikacji sieci Web. 

> [!NOTE] 
> 
> Jeśli chcesz przechowywać dane członkostwa wraz z danymi produktu, możesz rozważyć użycie tego samego **DbContext** , który został użyty do przechowywania danych produktu w powyższym kodzie.

 Słowo kluczowe *Internal* jest modyfikatorem dostępu dla typów (takich jak klasy) i składowych typu (takich jak metody lub właściwości). Typy wewnętrzne lub składowe są dostępne tylko w plikach znajdujących się w tym samym zestawie *(pliku DLL* ). Podczas kompilowania aplikacji tworzony jest plik zestawu *(. dll*) zawierający kod, który jest wykonywany podczas uruchamiania aplikacji. 

Obiekt `RoleStore`, który zapewnia zarządzanie rolami, jest tworzony na podstawie kontekstu bazy danych.

> [!NOTE] 
> 
> Należy zauważyć, że po utworzeniu obiektu `RoleStore` używa typu ogólnego `IdentityRole`. Oznacza to, że `RoleStore` może zawierać tylko obiekty `IdentityRole`. Również w przypadku używania typów ogólnych zasoby w pamięci są obsługiwane lepiej.

Następnie obiekt `RoleManager` jest tworzony na podstawie obiektu `RoleStore`, który właśnie został utworzony. Obiekt `RoleManager` uwidacznia interfejs API związany z rolą, który może służyć do automatycznego zapisywania zmian w `RoleStore`. `RoleManager` może zawierać tylko obiekty `IdentityRole`, ponieważ kod używa typu ogólnego `<IdentityRole>`.

Należy wywołać metodę `RoleExists`, aby określić, czy w bazie danych członkostwa występuje rola "Anuluj edycję". Jeśli tak nie jest, tworzysz rolę.

Tworzenie obiektu `UserManager` wydaje się bardziej skomplikowane niż kontrolka `RoleManager`, ale jest to niemal takie samo. Jest on właśnie zakodowany w jednym wierszu, a nie na kilku. W tym miejscu parametr, który przekazujesz, jest skonkretyzowany jako nowy obiekt zawarty w nawiasie.

Następnie utworzysz użytkownika "canEditUser", tworząc nowy obiekt `ApplicationUser`. Następnie w przypadku pomyślnego utworzenia użytkownika należy dodać użytkownika do nowej roli.

> [!NOTE] 
> 
> Obsługa błędów zostanie zaktualizowana w samouczku "Obsługa błędów ASP.NET" w dalszej części tej serii samouczków.

Przy następnym uruchomieniu aplikacji użytkownik o nazwie "canEditUser" zostanie dodany jako rola o nazwie "Red Edit" (Edytuj) aplikacji. W dalszej części tego samouczka zalogujesz się jako użytkownik "canEditUser", aby wyświetlić dodatkowe funkcje, które zostaną dodane w ramach tego samouczka. Aby uzyskać szczegółowe informacje dotyczące interfejsu API ASP.NET Identity, zobacz [przestrzeń nazw Microsoft. ASPNET. Identity](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Aby uzyskać dodatkowe informacje na temat inicjowania systemu ASP.NET Identity, zobacz [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Ograniczanie dostępu do strony administracyjnej

Przykładowa aplikacja Wingtip zabawki umożliwia zarówno anonimowym użytkownikom, jak i zalogowanym użytkownikom wyświetlanie i kupowanie produktów. Jednak zalogowany użytkownik, który ma niestandardową rolę "Edytuj", może uzyskać dostęp do strony z ograniczeniami w celu dodawania i usuwania produktów.

#### <a name="add-an-administration-folder-and-page"></a>Dodawanie folderu administracyjnego i strony

Następnie utworzysz folder o nazwie *admin* dla użytkownika "canEditUser" należącego do niestandardowej roli aplikacji przykładowej programu Wingtip zabawki.

1. Kliknij prawym przyciskiem myszy nazwę projektu (**zabawki Wingtip**) w **Eksplorator rozwiązań** a następnie wybierz pozycję **Dodaj** -&gt; **Nowy folder**.
2. Nadaj nowemu folderowi nazwę *administrator*.
3. Kliknij prawym przyciskiem myszy folder *administrator* , a następnie wybierz pozycję **Dodaj** -&gt; **nowy element**.   
   Zostanie wyświetlone okno dialogowe **Dodaj nowy element** .
4. Wybierz grupę szablonów <strong>sieci Web</strong> <strong>Visual C#</strong> -&gt; po lewej stronie. Z listy środkowej wybierz pozycję <strong>formularz sieci Web ze stroną wzorcową</strong>, nadaj jej nazwę <em>AdminPage. aspx</em><strong>,</strong> a następnie wybierz pozycję <strong>Dodaj</strong>.
5. Wybierz plik *site. Master* jako stronę wzorcową, a następnie wybierz przycisk **OK**.

#### <a name="add-a-webconfig-file"></a>Dodaj plik Web. config

Dodając plik *Web. config* do folderu *admin* , możesz ograniczyć dostęp do strony zawartej w folderze.

1. Kliknij prawym przyciskiem myszy folder *administrator* i wybierz polecenie **Dodaj** -&gt; **nowy element**.  
   Zostanie wyświetlone okno dialogowe **Dodaj nowy element** .
2. Z C# listy Visual Webtemplates wybierz pozycję <strong>plik konfiguracji sieci Web</strong>z listy środkowej, zaakceptuj domyślną nazwę pliku <em>Web. config</em><strong>,</strong> a następnie wybierz pozycję <strong>Dodaj</strong>.
3. Zastąp istniejącą zawartość XML w pliku *Web. config* następującym:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Zapisz plik *Web. config* . Plik *Web. config* określa, że tylko użytkownik należący do roli "po edycji" aplikacji może uzyskać dostęp do strony zawartej w folderze *administratora* .

### <a name="including-custom-role-navigation"></a>Dołączenie do nawigacji roli niestandardowej

Aby włączyć użytkownika niestandardowej roli "Edytuj" w celu przejścia do sekcji Administracja aplikacji, należy dodać łącze do strony *site. Master* . Tylko użytkownicy należący do roli "Edytuj" będą mogli zobaczyć link **administratora** i uzyskać dostęp do sekcji Administracja.

1. W Eksplorator rozwiązań Znajdź i Otwórz stronę *site. Master* .
2. Aby utworzyć łącze dla użytkownika roli "Edytuj", należy dodać znaczniki wyróżnione kolorem żółtym do poniższej listy nieuporządkowanej `<ul>` elementu, aby lista pojawiła się w następujący sposób:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Otwórz plik *site.Master.cs* . Ustaw łącze **administratora** jako widoczne tylko dla użytkownika "canEditUser", dodając kod wyróżniony kolorem żółtym do procedury obsługi `Page_Load`. Procedura obsługi `Page_Load` zostanie wyświetlona w następujący sposób:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Po załadowaniu strony kod sprawdza, czy zalogowany użytkownik ma rolę "Edytuj". Jeśli użytkownik należy do roli "Edytuj", element span zawierający link do strony *AdminPage. aspx* (i w związku z tym link wewnątrz zakresu) jest widoczny.

### <a name="enabling-product-administration"></a>Włączanie administrowania produktem

Do tej pory utworzono rolę "Red Edit" i dodano użytkownika "canEditUser", folder administracyjny i stronę administracyjną. Użytkownik ustawił prawa dostępu do folderu administracyjnego i strony i dodał link nawigacji dla użytkownika roli "Edytuj" do aplikacji. Następnie dodasz znaczniki do strony *AdminPage. aspx* i kodu do pliku *AdminPage.aspx.cs* z kodem, który umożliwi użytkownikowi z rolą "Edytuj", Dodawanie i usuwanie produktów.

1. W **Eksplorator rozwiązań**Otwórz plik *AdminPage. aspx* z folderu *admin* .
2. Zastąp istniejący znacznik następującym:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Następnie otwórz plik związany z kodem *AdminPage.aspx.cs* , klikając prawym przyciskiem myszy *AdminPage. aspx* i klikając polecenie **Wyświetl kod**.
4. Zastąp istniejący kod w pliku z kodem *AdminPage.aspx.cs* następującym kodem:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

W kodzie, który został wprowadzony dla pliku z kodem *AdminPage.aspx.cs* , Klasa o nazwie `AddProducts` wykonuje rzeczywistą służbę dodawania produktów do bazy danych. Ta klasa jeszcze nie istnieje, więc utworzysz ją teraz.

1. W **Eksplorator rozwiązań**kliknij prawym przyciskiem myszy folder *logiki* , a następnie wybierz pozycję **Dodaj** -&gt; **nowy element**.   
   Zostanie wyświetlone okno dialogowe **Dodaj nowy element** .
2. Wybierz grupę **Visual C#**  ** -&gt; szablon** szablony po lewej stronie. Następnie wybierz pozycję **Klasa**z listy Środkowej i nadaj jej nazwę *AddProducts.cs*.   
   Zostanie wyświetlony nowy plik klasy.
3. Zastąp istniejący kod następującym:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

Strona *AdminPage. aspx* zezwala użytkownikowi należącemu do roli "Edytuj" w celu dodawania i usuwania produktów. Po dodaniu nowego produktu szczegóły dotyczące produktu są sprawdzane, a następnie wprowadzane do bazy danych programu. Nowy produkt jest natychmiast dostępny dla wszystkich użytkowników aplikacji sieci Web.

#### <a name="unobtrusive-validation"></a>Niezauważalna weryfikacja

Szczegóły produktu udostępniane przez użytkownika na stronie *AdminPage. aspx* są weryfikowane przy użyciu kontrolek weryfikacji (`RequiredFieldValidator` i `RegularExpressionValidator`). Te kontrolki automatycznie używają niezauważalnej weryfikacji. Niedyskretna Walidacja umożliwia kontrolom walidacji używanie języka JavaScript dla logiki walidacji po stronie klienta, co oznacza, że strona nie będzie wymagała sprawdzenia poprawności na serwerze. Domyślnie niedyskretna weryfikacja jest zawarta w pliku *Web. config* na podstawie następującego ustawienia konfiguracji:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Wyrażenia regularne

Cena produktu na stronie *AdminPage. aspx* jest weryfikowana przy użyciu kontrolki **RegularExpressionValidator** . Ta kontrolka sprawdza, czy wartość skojarzonej kontrolki wejściowej (pole tekstowe "AddProductPrice") pasuje do wzorca określonego przez wyrażenie regularne. Wyrażenie regularne jest notacją zgodną z wzorcem, która umożliwia szybkie znajdowanie i dopasowywanie określonych wzorców znaków. Formant **RegularExpressionValidator** zawiera właściwość o nazwie `ValidationExpression`, która zawiera wyrażenie regularne używane do walidacji danych wejściowych ceny, jak pokazano poniżej:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>FileUpload — formant

Oprócz formantów wejściowych i walidacji dodano formant **FileUpload** do strony *AdminPage. aspx* . Ta kontrolka zapewnia możliwość przekazywania plików. W takim przypadku zezwalasz tylko na przekazywanie plików obrazów. W pliku związanym z kodem (*AdminPage.aspx.cs*), gdy kliknięto `AddProductButton`, kod sprawdza Właściwość `HasFile` kontrolki **FileUpload** . Jeśli kontrolka zawiera plik i jeśli typ pliku (na podstawie rozszerzenia pliku) jest dozwolony, obraz jest zapisywany w folderze *obrazy* i folderze *obrazy/kciuki* aplikacji.

#### <a name="model-binding"></a>Powiązanie modelu

Wcześniej w tej serii samouczków użyto powiązania modelu do wypełnienia kontrolki **ListView** , kontrolki **FormsView** , kontrolki **GridView** i kontrolki **kontrolką detailview** . W tym samouczku użyjesz powiązania modelu, aby wypełnić formant **DropDownList** z listą kategorii produktów.

Znacznik dodany do pliku *AdminPage. aspx* zawiera kontrolkę **DropDownList** o nazwie `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

Aby wypełnić to **DropDownList** , należy użyć powiązania modelu, ustawiając atrybut `ItemType` i atrybut `SelectMethod`. Atrybut `ItemType` określa, że należy użyć typu `WingtipToys.Models.Category` podczas wypełniania formantu. Ten typ został zdefiniowany na początku tej serii samouczków przez utworzenie klasy `Category` (pokazanej poniżej). Klasa `Category` znajduje się w folderze *models* wewnątrz pliku *Category.cs* .

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

Atrybut `SelectMethod` formantu **DropDownList** określa, że należy użyć metody `GetCategories` (pokazanej poniżej), która jest uwzględniona w pliku związanym z kodem (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Ta metoda określa, że interfejs `IQueryable` służy do szacowania zapytania względem typu `Category`. Zwracana wartość jest używana do wypełniania **DropDownList** w znaczniku strony (*AdminPage. aspx*).

Tekst wyświetlany dla każdego elementu na liście jest określany przez ustawienie atrybutu `DataTextField`. Atrybut `DataTextField` używa `CategoryName` klasy `Category` (pokazany powyżej) do wyświetlania każdej kategorii w kontrolce **DropDownList** . Wartość rzeczywista, która jest przesyłana po wybraniu elementu w kontrolce **DropDownList** , jest oparta na atrybucie `DataValueField`. Atrybut `DataValueField` jest ustawiany na `CategoryID`, jak zdefiniowano w klasie `Category` (pokazany powyżej).

### <a name="how-the-application-will-work"></a>Jak działa aplikacja

Gdy użytkownik należący do roli "" Edytuj "nawiguje do strony po raz pierwszy, formant `DropDownAddCategory`**DropDownList** jest wypełniany zgodnie z powyższym opisem. Formant **DropDownList** `DropDownRemoveProduct`jest również wypełniany produktami przy użyciu tego samego podejścia. Użytkownik należący do roli "Edytuj" wybiera typ kategorii i dodaje szczegóły produktu (**Nazwa**, **Opis**, **Cena**i **plik obrazu**). Gdy użytkownik należący do roli "edytujesz" kliknie przycisk **Dodaj produkt** , zostanie wyzwolona obsługa zdarzeń `AddProductButton_Click`. Program obsługi zdarzeń `AddProductButton_Click` znajdujący się w pliku związanym z kodem (*AdminPage.aspx.cs*) sprawdza plik obrazu, aby upewnić się, że jest zgodny z dozwolonymi typami plików *(GIF*, *PNG*, *JPEG*lub *jpg*). Następnie plik obrazu jest zapisywany w folderze przykładowej aplikacji Wingtip zabawki. Następnie nowy produkt zostanie dodany do bazy danych programu. Aby wykonać Dodawanie nowego produktu, tworzone jest nowe wystąpienie klasy `AddProducts` i nazwane produkty. Klasa `AddProducts` ma metodę o nazwie `AddProduct`, a obiekt Products wywołuje tę metodę, aby dodać produkty do bazy danych.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Jeśli kod pomyślnie dodaje nowy produkt do bazy danych, Strona zostanie ponownie załadowana przy użyciu wartości ciągu zapytania `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Po ponownym załadowaniu strony ciąg zapytania jest uwzględniany w adresie URL. Po ponownym załadowaniu strony użytkownik należący do roli "Edytuj" może natychmiast zobaczyć aktualizacje w kontrolkach **DropDownList** na stronie *AdminPage. aspx* . Ponadto, dołączając ciąg zapytania z adresem URL, na stronie może zostać wyświetlony komunikat o powodzeniu należący do roli "Edytuj".

Po ponownym załadowaniu strony *AdminPage. aspx* zostanie wywołane zdarzenie `Page_Load`.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

Program obsługi zdarzeń `Page_Load` sprawdza wartość ciągu zapytania i określa, czy ma być pokazywany komunikat o powodzeniu.

## <a name="running-the-application"></a>Uruchamianie aplikacji

Teraz możesz uruchomić aplikację, aby zobaczyć, jak można dodawać, usuwać i aktualizować elementy w koszyku. Kwota koszyka zakupów będzie odzwierciedlać łączny koszt wszystkich elementów w koszyku.

1. W Eksplorator rozwiązań naciśnij klawisz **F5** , aby uruchomić przykładową aplikację z programem Wingtip.  
   Zostanie otwarta przeglądarka i zostanie wyświetlona strona *default. aspx* .
2. Kliknij link **Zaloguj** się u góry strony. 

    ![Członkostwo i administracja — link do logowania](membership-and-administration/_static/image2.png)

   Zostanie wyświetlona strona *login. aspx* .
3. Użyj następującej nazwy użytkownika i hasła:  
   Nazwa użytkownika: canEditUser@wingtiptoys.com  
   Hasło: PA $ $word 1 

    ![Członkostwo i administracja — Strona logowania](membership-and-administration/_static/image3.png)
4. Kliknij przycisk **Zaloguj się w** dolnej części strony.
5. W górnej części następnej strony wybierz łącze **administratora** , aby przejść do strony *AdminPage. aspx* . 

    ![Członkostwo i administracja — link administracyjny](membership-and-administration/_static/image4.png)
6. Aby przetestować sprawdzanie poprawności danych wejściowych, kliknij przycisk **Dodaj produkt** bez dodawania szczegółowych informacji o produkcie. 

    ![Członkostwo i administracja — Strona administracyjna](membership-and-administration/_static/image5.png)

   Zauważ, że wyświetlane są wymagane komunikaty pól.
7. Dodaj szczegóły nowego produktu, a następnie kliknij przycisk **Dodaj produkt** . 

    ![Członkostwo i administracja — Dodawanie produktu](membership-and-administration/_static/image6.png)
8. Wybierz pozycję **produkty** z górnego menu nawigacji, aby wyświetlić nowy produkt, który został dodany. 

    ![Członkostwo i administracja — Pokaż nowy produkt](membership-and-administration/_static/image7.png)
9. Kliknij link **administratora** , aby powrócić do strony Administracja.
10. W sekcji **Usuń produkt** na stronie wybierz nowy produkt, który został dodany do **DropDownListBox**.
11. Kliknij przycisk **Usuń produkt** , aby usunąć nowy produkt z aplikacji. 

    ![Członkostwo i administracja — usuwanie produktu](membership-and-administration/_static/image8.png)
12. Wybierz pozycję **produkty** z górnego menu nawigacji, aby potwierdzić, że produkt został usunięty.
13. Kliknij pozycję **Wyloguj** się, aby istnieć tryb administrowania.   
    Należy zauważyć, że Górne okienko nawigacji nie pokazuje już elementu menu **administratora** .

## <a name="summary"></a>Podsumowanie

W tym samouczku dodano rolę niestandardową i użytkownika należącego do roli niestandardowej, ograniczony dostęp do folderu i strony administracji oraz udostępnienie nawigacji dla użytkownika należącego do roli niestandardowej. Użyto powiązania modelu, aby wypełnić formant **DropDownList** danymi. Zaimplementowano kontrolkę **FileUpload** i kontrolki walidacji. Dowiesz się również, jak dodawać i usuwać produkty z bazy danych. W następnym samouczku dowiesz się, jak wdrożyć Routing ASP.NET.

## <a name="additional-resources"></a>Dodatkowe materiały

[Web. config — element autoryzacji](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Wdrażanie bezpiecznej aplikacji ASP.NET Web Forms z członkostwem, uwierzytelnianiem OAuth i SQL Database w witrynie sieci Web systemu Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure — bezpłatna wersja próbna](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Poprzednie](checkout-and-payment-with-paypal.md)
> [dalej](url-routing.md)

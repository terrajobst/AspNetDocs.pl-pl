---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: Korzystanie z dostawców OAuth z MVC 4 | Microsoft Docs
author: Rick-Anderson
description: W tym samouczku pokazano, jak utworzyć aplikację sieci Web ASP.NET MVC 4, która umożliwia użytkownikom logowanie się przy użyciu poświadczeń od zewnętrznego dostawcy, takiego jak Facebo...
ms.author: riande
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5dfd1305376a62f4987caea242ca0f6aac1018e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539082"
---
# <a name="using-oauth-providers-with-mvc-4"></a>Korzystanie z dostawców uwierzytelniania OAuth we wzorcu MVC 4

Autor [FitzMacken](https://github.com/tfitzmac)

> W tym samouczku pokazano, jak utworzyć aplikację sieci Web ASP.NET MVC 4, która umożliwia użytkownikom logowanie się przy użyciu poświadczeń od zewnętrznego dostawcy, takiego jak Facebook, Twitter, Microsoft lub Google, a następnie zintegrowanie niektórych funkcji od tych dostawców z Aplikacja sieci Web. Dla uproszczenia ten samouczek koncentruje się na pracy z poświadczeniami z serwisu Facebook.
> 
> Aby używać poświadczeń zewnętrznych w aplikacji sieci Web ASP.NET MVC 5, zobacz [Tworzenie aplikacji ASP.NET MVC 5 z logowaniem w serwisie Facebook i Google OAuth2 i OpenID Connect](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> Włączenie tych poświadczeń w witrynach sieci Web zapewnia znaczną korzyść, ponieważ miliony użytkowników mają już konta z dostawcami zewnętrznymi. Mogą być bardziej nachylone użytkownicy, którzy nie muszą tworzyć i zapamiętać nowego zestawu poświadczeń. Ponadto po zalogowaniu się użytkownika za pośrednictwem jednego z tych dostawców można dołączyć do niego operacje społecznościowe.

## <a name="what-youll-build"></a>Co będziesz kompilować

W tym samouczku są dostępne dwa główne cele:

1. Umożliwia użytkownikowi zalogowanie się przy użyciu poświadczeń od dostawcy protokołu OAuth.
2. Pobierz informacje o koncie od dostawcy i Zintegruj te informacje z rejestracją konta dla Twojej witryny.

Mimo że przykłady w tym samouczku koncentrują się na korzystaniu z serwisu Facebook jako dostawcy uwierzytelniania, można zmodyfikować kod w celu użycia dowolnego dostawcy. Kroki implementowania dowolnego dostawcy są podobne do kroków opisanych w tym samouczku. Podczas wykonywania bezpośrednich wywołań do zestawu interfejsów API dostawcy będą widoczne tylko znaczące różnice.

## <a name="prerequisites"></a>Wymagania wstępne

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) lub [Microsoft Visual Studio Express 2012 dla sieci Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Lub

- Microsoft Visual Studio 2010 z dodatkiem SP1 lub [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

Ponadto w tym temacie założono, że masz podstawową wiedzę na temat ASP.NET MVC i programu Visual Studio. Jeśli potrzebujesz wprowadzenia do ASP.NET MVC 4, zobacz [wprowadzenie do ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Tworzenie projektu

W programie Visual Studio Utwórz nową aplikację sieci Web ASP.NET MVC 4 i nadaj jej nazwę &quot;OAuthMVC&quot;. Możesz określić wartość docelową .NET Framework 4,5 lub 4.

![Utwórz projekt](using-oauth-providers-with-mvc/_static/image1.png)

W oknie Nowy projekt ASP.NET MVC 4, wybierz pozycję **aplikacja internetowa** i pozostaw **Razor** jako aparat widoku.

![Wybierz aplikację internetową](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Włączanie dostawcy

Podczas tworzenia aplikacji sieci Web MVC 4 przy użyciu szablonu aplikacji internetowej projekt jest tworzony z plikiem o nazwie AuthConfig.cs w folderze Start\_aplikacji.

![Plik AuthConfig](using-oauth-providers-with-mvc/_static/image3.png)

Plik AuthConfig zawiera kod służący do rejestrowania klientów dla zewnętrznych dostawców uwierzytelniania. Domyślnie ten kod jest oznaczony jako komentarz, więc żaden z zewnętrznych dostawców nie jest włączony.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Musisz usunąć komentarz tego kodu, aby użyć klienta uwierzytelniania zewnętrznego. Usuń komentarz tylko dla dostawców, które chcesz dołączyć do witryny. W tym samouczku zostaną włączone tylko poświadczenia usługi Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Zwróć uwagę, że w powyższym przykładzie metoda zawiera puste ciągi parametrów rejestracji. Jeśli spróbujesz uruchomić aplikację teraz, aplikacja zgłasza wyjątek argumentu, ponieważ puste ciągi są niedozwolone dla parametrów. Aby zapewnić prawidłowe wartości, należy zarejestrować witrynę sieci Web u zewnętrznych dostawców, jak pokazano w następnej sekcji.

## <a name="registering-with-an-external-provider"></a>Rejestrowanie przy użyciu dostawcy zewnętrznego

Aby uwierzytelnić użytkowników przy użyciu poświadczeń od dostawcy zewnętrznego, należy zarejestrować swoją witrynę sieci Web u dostawcy. Po zarejestrowaniu lokacji zostaną wyświetlone parametry (takie jak klucz lub identyfikator oraz wpis tajny), które mają zostać uwzględnione podczas rejestrowania klienta. Musisz mieć konto z dostawcami, których chcesz użyć.

W tym samouczku nie są wyświetlane wszystkie kroki, które należy wykonać, aby zarejestrować się u tych dostawców. Te kroki zazwyczaj nie są trudne. Aby pomyślnie zarejestrować swoją witrynę, postępuj zgodnie z instrukcjami znajdującymi się w tych witrynach. Aby rozpocząć rejestrowanie witryny, zapoznaj się z witryną dewelopera:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Programu](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Podczas rejestrowania witryny w usłudze Facebook można podać &quot;localhost&quot; dla domeny lokacji i `&quot; http://localhost/&quot;` dla adresu URL, jak pokazano na poniższej ilustracji. Korzystanie z hosta localhost działa z większością dostawców, ale obecnie nie współpracuje z dostawcą firmy Microsoft. W przypadku dostawcy firmy Microsoft należy uwzględnić prawidłowy adres URL witryny sieci Web.

![Rejestrowanie witryny](using-oauth-providers-with-mvc/_static/image4.png)

Na powyższym obrazie usunięto wartości dla identyfikatora aplikacji, wpisu tajnego aplikacji oraz kontaktu z adresem e-mail. Po faktycznym zarejestrowaniu lokacji te wartości będą obecne. Należy zwrócić uwagę na wartości identyfikatora aplikacji i klucza tajnego aplikacji, ponieważ zostaną one dodane do aplikacji.

## <a name="creating-test-users"></a>Tworzenie użytkowników testowych

Jeśli nie chcesz korzystać z istniejącego konta w serwisie Facebook do testowania witryny, możesz pominąć tę sekcję.

Możesz łatwo tworzyć użytkowników testowych dla swojej aplikacji na stronie zarządzania aplikacjami w serwisie Facebook. Możesz użyć tych kont testowych, aby zalogować się do witryny. Użytkowników testowych można utworzyć, klikając link **role** w lewym okienku nawigacji, a następnie klikając link **Utwórz** .

![Tworzenie użytkowników testowych](using-oauth-providers-with-mvc/_static/image5.png)

Witryna Facebook automatycznie tworzy żądaną liczbę kont testowych.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Dodawanie identyfikatora aplikacji i klucza tajnego z dostawcy

Po odebraniu identyfikatora i wpisu tajnego z serwisu Facebook Wróć do pliku AuthConfig i Dodaj je jako wartości parametrów. Podane poniżej wartości nie są wartościami rzeczywistymi.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Zaloguj się przy użyciu poświadczeń zewnętrznych

To wszystko, co należy zrobić, aby włączyć zewnętrzne poświadczenia w Twojej lokacji. Uruchom aplikację, a następnie kliknij link logowania w prawym górnym rogu. Szablon automatycznie rozpoznaje, że zarejestrowano w usłudze Facebook jako dostawcę i zawiera przycisk dla dostawcy. Jeśli rejestrujesz wielu dostawców, zostanie automatycznie dołączony przycisk dla każdego z nich.

![Logowanie zewnętrzne](using-oauth-providers-with-mvc/_static/image6.png)

Ten samouczek nie obejmuje sposobu dostosowywania przycisków logowania dla zewnętrznych dostawców. Aby uzyskać te informacje, zobacz [Dostosowywanie interfejsu użytkownika logowania przy użyciu protokołu OAuth/OpenID Connect](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Kliknij przycisk Facebook, aby zalogować się przy użyciu poświadczeń serwisu Facebook. Po wybraniu jednego z zewnętrznych dostawców nastąpi przekierowanie do tej lokacji i wyświetlenie monitu o zalogowanie się.

Na poniższej ilustracji przedstawiono ekran logowania w serwisie Facebook. Informacje o tym, że używasz konta w serwisie Facebook do logowania się do witryny o nazwie oauthmvcexample.

![uwierzytelnianie w usłudze Facebook](using-oauth-providers-with-mvc/_static/image7.png)

Po zalogowaniu się przy użyciu poświadczeń usługi Facebook strona informuje użytkownika, że lokacja będzie miała dostęp do podstawowych informacji.

![Żądaj uprawnienia](using-oauth-providers-with-mvc/_static/image8.png)

Po wybraniu opcji **Przejdź do aplikacji**użytkownik musi zarejestrować się w lokacji. Na poniższej ilustracji przedstawiono stronę rejestracji po zalogowaniu się użytkownika przy użyciu poświadczeń w usłudze Facebook. Nazwa użytkownika jest zwykle wstępnie wypełniana nazwą dostawcy.

![zarejestruj](using-oauth-providers-with-mvc/_static/image9.png)

Kliknij pozycję **zarejestruj** , aby zakończyć rejestrację. Zamknij okno przeglądarki.

Zobaczysz, że nowe konto zostało dodane do bazy danych. W Eksplorator serwera Otwórz bazę danych **DefaultConnection** i Otwórz folder **Tables** .

![tabele bazy danych](using-oauth-providers-with-mvc/_static/image10.png)

Kliknij prawym przyciskiem myszy tabelę **USERPROFILE** i wybierz polecenie **Pokaż dane tabeli**.

![Pokaż dane](using-oauth-providers-with-mvc/_static/image11.png)

Zobaczysz nowe dodane konto. Przyjrzyj się danych na **stronie sieci web\_tabeli OAuthMembership** . Zobaczysz więcej danych związanych z zewnętrznym dostawcą dla właśnie dodanego konta.

Jeśli chcesz włączyć uwierzytelnianie zewnętrzne, wszystko jest gotowe. Można jednak dodatkowo zintegrować informacje od dostawcy z nowym procesem rejestracji użytkownika, jak pokazano w poniższych sekcjach.

## <a name="create-models-for-additional-user-information"></a>Tworzenie modeli dla dodatkowych informacji o użytkowniku

Jak zauważono w poprzednich sekcjach, nie trzeba pobierać żadnych dodatkowych informacji dla wbudowanej rejestracji konta do pracy. Jednak większość dostawców zewnętrznych przekazuje dodatkowe informacje o użytkowniku. W poniższych sekcjach pokazano, jak zachować te informacje i zapisać je w bazie danych. W odniesieniu do podanych wartości pełna nazwa użytkownika, identyfikator URI osobistej strony internetowej użytkownika oraz wartość wskazująca, czy w serwisie Facebook zweryfikowano konto.

Użyjesz [migracje Code First](https://msdn.microsoft.com/data/jj591621) , aby dodać tabelę do przechowywania dodatkowych informacji o użytkownikach. Dodajesz tabelę do istniejącej bazy danych, więc najpierw musisz utworzyć migawkę bieżącej bazy danych. Utworzenie migawki bieżącej bazy danych umożliwia późniejsze utworzenie migracji zawierającej tylko nową tabelę. Aby utworzyć migawkę bieżącej bazy danych:

1. Otwórz **konsolę Menedżera pakietów**
2. Uruchom polecenie **enable-migrations**
3. Uruchamianie polecenia **Add-Migration Initial-IgnoreChanges**
4. Uruchom polecenie **Update-Database**

Teraz dodasz nowe właściwości. W folderze modele Otwórz plik AccountModels.cs i Znajdź klasę RegisterExternalLoginModel. Klasa RegisterExternalLoginModel przechowuje wartości, które powracają z dostawcy uwierzytelniania. Dodaj właściwości o nazwie FullName i link, jak wyróżniono poniżej.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

Również w AccountModels.cs Dodaj nową klasę o nazwie ExtraUserInformation. Ta klasa reprezentuje nową tabelę, która zostanie utworzona w bazie danych.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

W klasie UsersContext Dodaj wyróżniony kod poniżej, aby utworzyć właściwość Nieogólnymi dla nowej klasy.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Teraz można przystąpić do tworzenia nowej tabeli. Otwórz ponownie konsolę Menedżera pakietów i ten czas:

1. Uruchom polecenie **Add-Migration AddExtraUserInformation**
2. Uruchom polecenie **Update-Database**

Nowa tabela istnieje już w bazie danych.

## <a name="retrieve-the-additional-data"></a>Pobierz dodatkowe dane

Istnieją dwa sposoby pobrania dodatkowych danych użytkownika. Pierwszy sposób polega na zachowywaniu danych użytkownika, które są domyślnie przesyłane z powrotem podczas żądania uwierzytelnienia. Drugi sposób to wywołanie interfejsu API dostawcy i zażądanie dodatkowych informacji. Wartości dla FullName i linku są automatycznie przenoszone z powrotem przez serwis Facebook. Wartość wskazująca, czy w serwisie Facebook zostało zweryfikowane, że konto jest pobierane przez wywołanie interfejsu API usługi Facebook. Najpierw należy wypełnić wartości dla FullName i linku, a następnie uzyskać zweryfikowaną wartość.

Aby pobrać dodatkowe dane użytkownika, Otwórz plik **AccountController.cs** w folderze **controllers** .

Ten plik zawiera logikę rejestrowania, rejestrowania i zarządzania kontami. W szczególności należy zauważyć metody o nazwie **ExternalLoginCallback** i **ExternalLoginConfirmation**. W ramach tych metod można dodać kod w celu dostosowania zewnętrznych operacji logowania do aplikacji. Pierwszy wiersz metody **ExternalLoginCallback** zawiera:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Dodatkowe dane użytkownika są przesyłane z powrotem do właściwości **ExtraData** obiektu **AuthenticationResult** , który jest zwracany z metody **VerifyAuthentication** . Klient Facebook zawiera następujące wartości właściwości **ExtraData** :

- {1&gt;identyfikator&lt;1}
- name
- połączenie
- płeć
- accesstoken

Inni dostawcy będą mieć podobne, ale nieco inne dane we właściwości ExtraData.

Jeśli użytkownik jest nowym użytkownikiem witryny, zostaną pobrane pewne dodatkowe dane i przekazane dane do widoku potwierdzenia. Ostatni blok kodu w metodzie jest uruchamiany tylko wtedy, gdy użytkownik jest nowy w witrynie. Zastąp następujący wiersz:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

z tym wierszem:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Ta zmiana jedynie zawiera wartości właściwości FullName i link.

W metodzie **ExternalLoginConfirmation** zmodyfikuj kod wyróżniony poniżej, aby zapisać dodatkowe informacje o użytkowniku.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Dostosowywanie widoku

Dodatkowe dane użytkownika pobierane od dostawcy zostaną wyświetlone na stronie Rejestracja.

W folderze **widoki**/**konta** Otwórz **ExternalLoginConfirmation. cshtml**. Poniżej istniejącego pola Nazwa użytkownika Dodaj pola dla FullName, linku i PictureLink.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Teraz prawie gotowe do uruchomienia aplikacji i rejestrowania nowego użytkownika z dodatkowymi zapisanymi informacjami. Musisz mieć konto, które nie jest już zarejestrowane w witrynie. Możesz użyć innego konta testowego lub usunąć wiersze w obszarze **USERPROFILE** i **stronach sieci Web\_tabele OAuthMembership** dla konta, którego chcesz użyć ponownie. Usuwając te wiersze, upewnij się, że konto zostanie ponownie zarejestrowane.

Uruchom aplikację i Zarejestruj nowego użytkownika. Należy zauważyć, że ta godzina na stronie potwierdzenia zawiera więcej wartości.

![zarejestruj](using-oauth-providers-with-mvc/_static/image12.png)

Po zakończeniu rejestracji Zamknij przeglądarkę. Zapoznaj się z bazą danych, aby zauważyć nowe wartości w tabeli **ExtraUserInformation** .

## <a name="install-nuget-package-for-facebook-api"></a>Zainstaluj pakiet NuGet dla interfejsu API usługi Facebook

Serwis Facebook udostępnia [interfejs API](https://developers.facebook.com/docs/reference/apis/) , który można wywołać w celu wykonania operacji. Interfejs API usługi Facebook można wywołać, kierując do wysyłania żądań HTTP lub instalując pakiet NuGet, który ułatwia wysyłanie tych żądań. W tym samouczku przedstawiono pakiet NuGet, ale instalacja pakietu NuGet nie jest istotna. W tym samouczku pokazano, jak korzystać C# z pakietu SDK serwisu Facebook. Istnieją inne pakiety NuGet, które pomagają w wywołaniu interfejsu API usługi Facebook.

Z okna **Zarządzanie pakietami NuGet** wybierz pakiet SDK serwisu C# Facebook.

![Zainstaluj pakiet](using-oauth-providers-with-mvc/_static/image13.png)

Użyjesz zestawu SDK usługi C# Facebook do wywołania operacji wymagającej tokenu dostępu dla użytkownika. W następnej sekcji pokazano, jak uzyskać token dostępu.

## <a name="retrieve-access-token"></a>Pobieranie tokenu dostępu

Większość dostawców zewnętrznych przeszedł do zwrotnego tokenu dostępu po zweryfikowaniu poświadczeń użytkownika. Ten token dostępu jest bardzo ważny, ponieważ umożliwia wywoływanie operacji, które są dostępne tylko dla uwierzytelnionych użytkowników. W związku z tym, pobieranie i przechowywanie tokenu dostępu jest niezbędne, jeśli chcesz zapewnić większą funkcjonalność.

W zależności od dostawcy zewnętrznego token dostępu może być ważny przez ograniczoną ilość czasu. Aby upewnić się, że masz prawidłowy token dostępu, będzie on pobierany za każdym razem, gdy użytkownik zaloguje się i zapisuje jako wartość sesji zamiast zapisywać ją w bazie danych.

W metodzie **ExternalLoginCallback** token dostępu jest również przenoszona z powrotem do właściwości **ExtraData** obiektu **AuthenticationResult** . Dodaj wyróżniony kod do **ExternalLoginCallback** , aby zapisać token dostępu w obiekcie **sesji** . Ten kod jest uruchamiany za każdym razem, gdy użytkownik zaloguje się przy użyciu konta w serwisie Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Mimo że ten przykład pobiera token dostępu z serwisu Facebook, można pobrać token dostępu od dowolnego dostawcy zewnętrznego za pośrednictwem tego samego klucza o nazwie &quot;AccessToken&quot;.

## <a name="logging-off"></a>Wylogowywanie

Domyślna metoda **wylogowywania** rejestruje użytkownika z aplikacji, ale nie rejestruje użytkownika poza dostawcą zewnętrznym. Aby wylogować użytkownika z serwisu Facebook i uniemożliwić utrwalanie tokenu po wylogowaniu się użytkownika, można dodać następujący wyróżniony kod do metody **Logoff** w elementu AccountController.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

Wartość podaną w `next` parametr jest adresem URL, który będzie używany po wylogowaniu się użytkownika z serwisu Facebook. Podczas testowania na komputerze lokalnym należy podać prawidłowy numer portu dla witryny lokalnej. W środowisku produkcyjnym należy podać stronę domyślną, taką jak contoso.com.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Pobierz informacje o użytkowniku, które wymagają tokenu dostępu

Po zapisaniu tokenu dostępu i zainstalowaniu pakietu SDK usługi Facebook C# można użyć ich razem w celu zażądania dodatkowych informacji o użytkownikach z serwisu Facebook. W metodzie **ExternalLoginConfirmation** Utwórz wystąpienie klasy **FacebookClient** przez przekazanie wartości tokenu dostępu. Zażądaj wartości właściwości **zweryfikowanej** dla bieżącego, uwierzytelnionego użytkownika. **Zweryfikowana** Właściwość wskazuje, czy w serwisie Facebook zweryfikowano konto za pomocą innych metod, takich jak wysyłanie komunikatów do telefonu komórkowego. Zapisz tę wartość w bazie danych.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Konieczne będzie ponowne usunięcie rekordów z bazy danych dla użytkownika lub użycie innego konta w serwisie Facebook.

Uruchom aplikację i Zarejestruj nowego użytkownika. Sprawdź tabelę **ExtraUserInformation** , aby wyświetlić wartość zweryfikowanej właściwości.

## <a name="conclusion"></a>Podsumowanie

W tym samouczku utworzono lokację zintegrowaną z usługą Facebook w celu uwierzytelniania użytkowników i danych rejestracyjnych. Zapoznaj się z domyślnym zachowaniem skonfigurowanym dla aplikacji sieci Web MVC 4 i sposobem dostosowania tego zachowania domyślnego.

## <a name="related-topics"></a>Tematy pokrewne

- [Utwórz aplikację ASP.NET MVC z uwierzytelnianiem i bazą danych SQL, a następnie wdróż ją w Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)

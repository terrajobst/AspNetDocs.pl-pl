---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Zmień klucz podstawowy dla użytkowników w ASP.NET Identity ASP.NET 4. x
author: Rick-Anderson
description: W Visual Studio 2013 domyślna aplikacja sieci Web używa wartości ciągu klucza dla kont użytkowników. ASP.NET Identity umożliwia zmianę typu...
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0afea8eacfc646f1489b87629fdb2d437815d88c
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519144"
---
# <a name="change-primary-key-for-users-in-aspnet-identity"></a>Zmiana klucza podstawowego dla użytkowników w systemie ASP.NET Identity

przez [Tom FitzMacken](https://github.com/tfitzmac)

> W Visual Studio 2013 domyślna aplikacja sieci Web używa wartości ciągu klucza dla kont użytkowników. ASP.NET Identity umożliwia zmianę typu klucza w celu spełnienia wymagań dotyczących danych. Na przykład można zmienić typ klucza z ciągu na liczbę całkowitą.
> 
> W tym temacie pokazano, jak rozpocząć od domyślnej aplikacji sieci Web i zmienić klucz konta użytkownika na liczbę całkowitą. Można użyć tych samych modyfikacji w celu zaimplementowania dowolnego typu klucza w projekcie. Pokazuje, jak wprowadzić te zmiany w domyślnej aplikacji sieci Web, ale można zastosować podobne modyfikacje do niestandardowej aplikacji. Pokazuje zmiany, które są niezbędne podczas pracy z kontrolerami MVC lub Web Forms.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używane w samouczku
> 
> 
> - Visual Studio 2013 z aktualizacją Update 2 (lub nowszą)
> - ASP.NET Identity 2,1 lub nowszy

Aby wykonać kroki opisane w tym samouczku, musisz mieć Visual Studio 2013 Update 2 (lub nowsze wersje) oraz aplikację sieci Web utworzoną na podstawie szablonu aplikacji sieci Web ASP.NET. Szablon został zmieniony w Update 3. W tym temacie pokazano, jak zmienić szablon w Update 2 i Update 3.

Ten temat zawiera następujące sekcje:

- [Zmień typ klucza w klasie User Identity](#userclass)
- [Dodawanie niestandardowych klas tożsamości, które używają typu klucza](#customclass)
- [Zmień klasę kontekstu i Menedżera użytkowników, aby używały typu klucza](#context)
- [Zmień konfigurację początkową, aby użyć typu klucza](#startup)
- [W przypadku składnika MVC z aktualizacją Update 2 Zmień elementu AccountController, aby przekazać typ klucza](#mvcupdate2)
- [W przypadku składnika MVC z aktualizacją Update 3 Zmień wartość elementu AccountController i ManageController, aby przekazać typ klucza](#mvcupdate3)
- [W przypadku formularzy sieci Web z aktualizacją Update 2 Zmień strony kont w celu przekazania typu klucza](#webformsupdate2)
- [W przypadku formularzy sieci Web z aktualizacją Update 3 Zmień strony kont w celu przekazania typu klucza](#webformsupdate3)
- [Uruchom aplikację](#run)
- [Inne zasoby](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Zmień typ klucza w klasie User Identity

W projekcie utworzonym na podstawie szablonu aplikacji sieci Web ASP.NET Określ, że Klasa ApplicationUser używa liczby całkowitej dla klucza dla kont użytkowników. W IdentityModels.cs Zmień klasę ApplicationUser, aby dziedziczyć z IdentityUser, która ma typ **int** dla parametru ogólnego TKey. Można również przekazać nazwy trzech dostosowanych klas, które jeszcze nie zostały zaimplementowane.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Typ klucza został zmieniony, ale domyślnie reszta aplikacji nadal zakłada, że klucz jest ciągiem. Należy jawnie wskazać typ klucza w kodzie, który przyjmuje ciąg.

W klasie **ApplicationUser** Zmień metodę **GenerateUserIdentityAsync** , tak aby obejmowała int, jak pokazano w wyróżnionym kodzie poniżej. Ta zmiana nie jest wymagana w przypadku projektów formularzy sieci Web z szablonem Update 3.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Dodawanie niestandardowych klas tożsamości, które używają typu klucza

Inne klasy tożsamości, takie jak IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, są nadal skonfigurowane do używania klucza ciągu. Utwórz nowe wersje tych klas, które określają liczbę całkowitą dla klucza. W tych klasach nie trzeba podawać wielu kodów implementacji, ale wystarczy tylko ustawić int jako klucz.

Dodaj następujące klasy do pliku IdentityModels.cs.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Zmień klasę kontekstu i Menedżera użytkowników, aby używały typu klucza

W IdentityModels.cs Zmień definicję klasy **ApplicationDbContext** , tak aby korzystała z nowych niestandardowych klas i **int** dla klucza, jak pokazano w wyróżnionym kodzie.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

Parametr ThrowIfV1Schema nie jest już prawidłowy w konstruktorze. Zmień konstruktora, aby nie przeszedł wartości ThrowIfV1Schema.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

Otwórz IdentityConfig.cs i Zmień klasę **ApplicationUserManger** , tak aby używała nowej klasy magazynu użytkownika do utrwalania danych i **int** dla klucza.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

W szablonie Update 3 należy zmienić klasę ApplicationSignInManager.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Zmień konfigurację początkową, aby użyć typu klucza

W Startup.Auth.cs Zastąp kod OnValidateIdentity jako wyróżniony poniżej. Należy zauważyć, że definicja getUserIdCallback analizuje wartość ciągu na liczbę całkowitą.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Jeśli projekt nie rozpoznaje ogólnej implementacji metody **GetUserID** , może być konieczne zaktualizowanie ASP.NET Identity pakietu NuGet do wersji 2,1

Wprowadzono wiele zmian klas infrastruktury używanych przez ASP.NET Identity. Jeśli spróbujesz skompilować projekt, zobaczysz wiele błędów. Na szczęście pozostałe błędy są podobne. Klasa Identity oczekuje liczby całkowitej dla klucza, ale kontroler (lub formularz sieci Web) przekazuje wartość ciągu. W każdym przypadku należy przekonwertować ciąg na i liczbę całkowitą, wywołując metodę **GetUserID&lt;int&gt;** . Możesz przejść przez listę błędów z kompilacji lub wykonać poniższe zmiany.

Pozostałe zmiany są zależne od typu tworzonego projektu i aktualizacji zainstalowanego w programie Visual Studio. Możesz przejść bezpośrednio do odpowiedniej sekcji, korzystając z następujących linków

- [W przypadku składnika MVC z aktualizacją Update 2 Zmień elementu AccountController, aby przekazać typ klucza](#mvcupdate2)
- [W przypadku składnika MVC z aktualizacją Update 3 Zmień wartość elementu AccountController i ManageController, aby przekazać typ klucza](#mvcupdate3)
- [W przypadku formularzy sieci Web z aktualizacją Update 2 Zmień strony kont w celu przekazania typu klucza](#webformsupdate2)
- [W przypadku formularzy sieci Web z aktualizacją Update 3 Zmień strony kont w celu przekazania typu klucza](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>W przypadku składnika MVC z aktualizacją Update 2 Zmień elementu AccountController, aby przekazać typ klucza

Otwórz plik AccountController.cs. Należy zmienić następujące metody.

**ConfirmEmail** , Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**Usuń skojarzenie** metody

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Zarządzanie (ManageUserViewModel)** — Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**LinkLoginCallback** , Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**RemoveAccountList** , Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword** , Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

Teraz możesz [uruchomić aplikację](#run) i zarejestrować nowego użytkownika.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>W przypadku składnika MVC z aktualizacją Update 3 Zmień wartość elementu AccountController i ManageController, aby przekazać typ klucza

Otwórz plik AccountController.cs. Należy zmienić następującą metodę.

**ConfirmEmail** , Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**Kontrolka sendcode** , Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Otwórz plik ManageController.cs. Należy zmienić następujące metody.

**Index** — Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

Metody **RemoveLogin**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

Add-No-- **-Metoda**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

**EnableTwoFactorAuthentication** , Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

**DisableTwoFactorAuthentication** , Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

Metody **VerifyPhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

**RemovePhoneNumber** , Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**ChangePassword** — Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword** — Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

**ManageLogins** , Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

**LinkLoginCallback** , Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

**HasPassword** , Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

**HasPhoneNumber** , Metoda

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

Teraz możesz [uruchomić aplikację](#run) i zarejestrować nowego użytkownika.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>W przypadku formularzy sieci Web z aktualizacją Update 2 Zmień strony kont w celu przekazania typu klucza

W przypadku formularzy sieci Web z aktualizacją Update 2 należy zmienić następujące strony.

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Teraz możesz [uruchomić aplikację](#run) i zarejestrować nowego użytkownika.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>W przypadku formularzy sieci Web z aktualizacją Update 3 Zmień strony kont w celu przekazania typu klucza

W przypadku formularzy sieci Web z aktualizacją Update 3 należy zmienić następujące strony.

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>Uruchom aplikację

Zakończono wszystkie wymagane zmiany w szablonie domyślnego aplikacji sieci Web. Uruchom aplikację i Zarejestruj nowego użytkownika. Po zarejestrowaniu użytkownika można zauważyć, że tabela AspNetUsers ma kolumnę ID, która jest liczbą całkowitą.

![nowy klucz podstawowy](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Jeśli wcześniej utworzono tabele ASP.NET Identity z innym kluczem podstawowym, musisz wprowadzić pewne dodatkowe zmiany. Jeśli to możliwe, po prostu Usuń istniejącą bazę danych. Baza danych zostanie ponownie utworzona przy użyciu poprawnego projektu podczas uruchamiania aplikacji sieci Web i dodania nowego użytkownika. Jeśli nie można usunąć, uruchom najpierw migracje kodu, aby zmienić tabele. Jednak nowy klucz podstawowy w postaci wartości całkowitej nie zostanie skonfigurowany jako właściwość tożsamości SQL w bazie danych. Należy ręcznie ustawić kolumnę ID jako tożsamość.

<a id="other"></a>
## <a name="other-resources"></a>Inne zasoby

- [Omówienie niestandardowych dostawców magazynu dla produktu ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Migrowanie istniejącej witryny internetowej z członkostwa SQL do produktu ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Migrowanie danych dostawcy uniwersalnego dla członkostwa i profilów użytkowników do ASP.NET Identity](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Przykładowa aplikacja](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) ze zmienionym kluczem podstawowym

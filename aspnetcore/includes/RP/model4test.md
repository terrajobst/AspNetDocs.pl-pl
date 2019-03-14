---
ms.openlocfilehash: 22c540058e0b3b9ae612f1b2612cecc08627ea2b
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57077858"
---
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="1de0f-101">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="1de0f-101">Test the app</span></span>

* <span data-ttu-id="1de0f-102">Uruchom aplikację i dołączyć `/Movies` do adresu URL w przeglądarce (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="1de0f-102">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="1de0f-103">Test **Utwórz** łącza.</span><span class="sxs-lookup"><span data-stu-id="1de0f-103">Test the **Create** link.</span></span>

  ![Tworzenie strony](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="1de0f-105">Test **Edytuj**, **szczegóły**, i **Usuń** łącza.</span><span class="sxs-lookup"><span data-stu-id="1de0f-105">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="1de0f-106">Jeśli zostanie wyświetlony następujący błąd, sprawdź masz Uruchom migracje i aktualizacji bazy danych:</span><span class="sxs-lookup"><span data-stu-id="1de0f-106">If you get the following error, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```

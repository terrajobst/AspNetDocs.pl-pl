---
title: Zewnętrznych dostawców uwierzytelniania OAuth
author: rick-anderson
description: Odkryj dostawców uwierzytelniania OAuth zewnętrznych, które działają z aplikacji platformy ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/otherlogins
ms.openlocfilehash: b69c366ec1bf12ccf434991fc8a79eaf8c09da3d
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57074009"
---
# <a name="external-oauth-authentication-providers"></a><span data-ttu-id="564ff-103">Zewnętrznych dostawców uwierzytelniania OAuth</span><span class="sxs-lookup"><span data-stu-id="564ff-103">External OAuth authentication providers</span></span>

<span data-ttu-id="564ff-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT), [autorem jest Pranav Rastogi](https://github.com/rustd), i [Valeriy Novytskyy](https://github.com/01binary)</span><span class="sxs-lookup"><span data-stu-id="564ff-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Pranav Rastogi](https://github.com/rustd), and [Valeriy Novytskyy](https://github.com/01binary)</span></span>

<span data-ttu-id="564ff-105">Poniższa lista zawiera typowe zewnętrznych dostawców uwierzytelniania OAuth współpracujących z aplikacji platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="564ff-105">The following list includes common external OAuth authentication providers that work with ASP.NET Core apps.</span></span> <span data-ttu-id="564ff-106">Pakiety NuGet innych firm, takich jak te obsługiwane przez [aspnet contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth), może służyć jako uzupełnienie dostawców uwierzytelniania implementowane przez zespół programu ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="564ff-106">Third-party NuGet packages, such as the ones maintained by [aspnet-contrib](https://www.nuget.org/packages?q=owners%3Aaspnet-contrib+title%3AOAuth), can be used to complement the authentication providers implemented by the ASP.NET Core team.</span></span>

* <span data-ttu-id="564ff-107">[LinkedIn](https://www.linkedin.com/developer/apps) ([instrukcje](https://developer.linkedin.com/docs/oauth2))</span><span class="sxs-lookup"><span data-stu-id="564ff-107">[LinkedIn](https://www.linkedin.com/developer/apps) ([Instructions](https://developer.linkedin.com/docs/oauth2))</span></span>

* <span data-ttu-id="564ff-108">[Instagram](https://www.instagram.com/developer/register/) ([instrukcje](https://www.instagram.com/developer/authentication/))</span><span class="sxs-lookup"><span data-stu-id="564ff-108">[Instagram](https://www.instagram.com/developer/register/) ([Instructions](https://www.instagram.com/developer/authentication/))</span></span>

* <span data-ttu-id="564ff-109">[Reddit](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps) ([instrukcje](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example))</span><span class="sxs-lookup"><span data-stu-id="564ff-109">[Reddit](https://www.reddit.com/login?dest=https%3A%2F%2Fwww.reddit.com%2Fprefs%2Fapps) ([Instructions](https://github.com/reddit/reddit/wiki/OAuth2-Quick-Start-Example))</span></span>

* <span data-ttu-id="564ff-110">[Github](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew) ([instrukcje](https://developer.github.com/v3/oauth/))</span><span class="sxs-lookup"><span data-stu-id="564ff-110">[Github](https://github.com/login?return_to=https%3A%2F%2Fgithub.com%2Fsettings%2Fapplications%2Fnew) ([Instructions](https://developer.github.com/v3/oauth/))</span></span>

* <span data-ttu-id="564ff-111">[Yahoo](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F) ([instrukcje](https://developer.yahoo.com/bbauth/user.html))</span><span class="sxs-lookup"><span data-stu-id="564ff-111">[Yahoo](https://login.yahoo.com/config/login?src=devnet&.done=http%3A%2F%2Fdeveloper.yahoo.com%2Fapps%2Fcreate%2F) ([Instructions](https://developer.yahoo.com/bbauth/user.html))</span></span>

* <span data-ttu-id="564ff-112">[Tumblr](https://www.tumblr.com/oauth/apps) ([instrukcje](https://www.tumblr.com/docs/api/v2#auth))</span><span class="sxs-lookup"><span data-stu-id="564ff-112">[Tumblr](https://www.tumblr.com/oauth/apps) ([Instructions](https://www.tumblr.com/docs/api/v2#auth))</span></span>

* <span data-ttu-id="564ff-113">[Pinterest](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F) ([instrukcje](https://developers.pinterest.com/docs/api/overview/?))</span><span class="sxs-lookup"><span data-stu-id="564ff-113">[Pinterest](https://www.pinterest.com/login/?next=http%3A%2F%2Fdevsite%2Fapps%2F) ([Instructions](https://developers.pinterest.com/docs/api/overview/?))</span></span>

* <span data-ttu-id="564ff-114">[Pocket](https://getpocket.com/developer/apps/new) ([instrukcje](https://getpocket.com/developer/docs/authentication))</span><span class="sxs-lookup"><span data-stu-id="564ff-114">[Pocket](https://getpocket.com/developer/apps/new) ([Instructions](https://getpocket.com/developer/docs/authentication))</span></span>

* <span data-ttu-id="564ff-115">[Flickr](https://www.flickr.com/services/apps/create) ([instrukcje](https://www.flickr.com/services/api/auth.oauth.html))</span><span class="sxs-lookup"><span data-stu-id="564ff-115">[Flickr](https://www.flickr.com/services/apps/create) ([Instructions](https://www.flickr.com/services/api/auth.oauth.html))</span></span>

* <span data-ttu-id="564ff-116">[Dribble](https://dribbble.com/signup) ([instrukcje](http://developer.dribbble.com/v1/oauth/))</span><span class="sxs-lookup"><span data-stu-id="564ff-116">[Dribble](https://dribbble.com/signup) ([Instructions](http://developer.dribbble.com/v1/oauth/))</span></span>

* <span data-ttu-id="564ff-117">[Usługi Vimeo](https://vimeo.com/join) ([instrukcje](https://developer.vimeo.com/api/authentication))</span><span class="sxs-lookup"><span data-stu-id="564ff-117">[Vimeo](https://vimeo.com/join) ([Instructions](https://developer.vimeo.com/api/authentication))</span></span>

* <span data-ttu-id="564ff-118">[SoundCloud](https://soundcloud.com/you/apps/new) ([instrukcje](https://developers.soundcloud.com/blog/we-love-oauth-2))</span><span class="sxs-lookup"><span data-stu-id="564ff-118">[SoundCloud](https://soundcloud.com/you/apps/new) ([Instructions](https://developers.soundcloud.com/blog/we-love-oauth-2))</span></span>

* <span data-ttu-id="564ff-119">[VK](https://vk.com/apps?act=manage) ([instrukcje](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites))</span><span class="sxs-lookup"><span data-stu-id="564ff-119">[VK](https://vk.com/apps?act=manage) ([Instructions](https://vk.com/pages?oid=-17680044&p=Authorizing_Sites))</span></span>

[!INCLUDE[Multiple authentication providers](includes/chain-auth-providers.md)]

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

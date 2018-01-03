---
uid: signalr/overview/security/persistent-connection-authorization
title: "Проверка подлинности и авторизация для постоянного подключения SignalR | Документы Microsoft"
author: pfletcher
description: "В этом разделе описывается принудительной авторизации для постоянного подключения. Общие сведения об интеграции безопасности в приложении SignalR..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9c6fff86ae6b1b65e6ba9922b6b8448643ef1f15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="b37bf-104">Проверка подлинности и авторизация для постоянного подключения SignalR</span><span class="sxs-lookup"><span data-stu-id="b37bf-104">Authentication and Authorization for SignalR Persistent Connections</span></span>
====================
<span data-ttu-id="b37bf-105">по [Флетчера Патрик](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b37bf-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b37bf-106">В этом разделе описывается принудительной авторизации для постоянного подключения.</span><span class="sxs-lookup"><span data-stu-id="b37bf-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="b37bf-107">Общие сведения об интеграции безопасности в приложении SignalR см. в разделе [введение в безопасность](introduction-to-security.md).</span><span class="sxs-lookup"><span data-stu-id="b37bf-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="b37bf-108">Версии программного обеспечения, используемого в этом разделе</span><span class="sxs-lookup"><span data-stu-id="b37bf-108">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="b37bf-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b37bf-109">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="b37bf-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b37bf-110">.NET 4.5</span></span>
> - <span data-ttu-id="b37bf-111">SignalR версии 2</span><span class="sxs-lookup"><span data-stu-id="b37bf-111">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="b37bf-112">Предыдущие версии этого раздела</span><span class="sxs-lookup"><span data-stu-id="b37bf-112">Previous versions of this topic</span></span>
> 
> <span data-ttu-id="b37bf-113">Сведения о более ранних версий SignalR см. в разделе [более ранних версий SignalR](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="b37bf-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="b37bf-114">Вопросы и комментарии</span><span class="sxs-lookup"><span data-stu-id="b37bf-114">Questions and comments</span></span>
> 
> <span data-ttu-id="b37bf-115">Оставьте отзыв на том, как вам понравилось этого учебника и что можно улучшить в комментарии в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="b37bf-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="b37bf-116">Если у вас есть вопросы, которые не связаны непосредственно для работы с учебником, их можно разместить [форум по ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) или [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="b37bf-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


## <a name="enforce-authorization"></a><span data-ttu-id="b37bf-117">Принудительной авторизации</span><span class="sxs-lookup"><span data-stu-id="b37bf-117">Enforce authorization</span></span>

<span data-ttu-id="b37bf-118">Для применения правила авторизации при использовании [подключение PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) необходимо переопределить `AuthorizeRequest` метод.</span><span class="sxs-lookup"><span data-stu-id="b37bf-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="b37bf-119">Нельзя использовать `Authorize` атрибута с постоянные подключения.</span><span class="sxs-lookup"><span data-stu-id="b37bf-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="b37bf-120">`AuthorizeRequest` Метод вызывается инфраструктурой SignalR перед каждым запросом, чтобы убедиться, что пользователь авторизован для выполнения запрошенного действия.</span><span class="sxs-lookup"><span data-stu-id="b37bf-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="b37bf-121">`AuthorizeRequest` Метод не вызывается из клиента; вместо этого проверки подлинности пользователя через приложения стандартным механизмом проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="b37bf-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="b37bf-122">В приведенном ниже примере показано, как ограничить запросы пользователям, прошедшим проверку.</span><span class="sxs-lookup"><span data-stu-id="b37bf-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="b37bf-123">Можно добавить логику авторизации, настроенные в методе AuthorizeRequest; например проверка принадлежности пользователя к определенной роли.</span><span class="sxs-lookup"><span data-stu-id="b37bf-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
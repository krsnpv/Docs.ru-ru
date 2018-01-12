---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: "Решение диспетчера контактов | Документы Microsoft"
author: jrjlee
description: "Эта серия учебников использует образец решения & #x 2014; диспетчера контактов решения & #x 2014; для представления приложения корпоративного уровня с реалистичных уровень..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: b7f691a1ee855788f6a57616aea35d960e4c85c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="the-contact-manager-solution"></a><span data-ttu-id="c7d88-103">Решение диспетчера контактов</span><span class="sxs-lookup"><span data-stu-id="c7d88-103">The Contact Manager Solution</span></span>
====================
<span data-ttu-id="c7d88-104">по [Джейсон Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="c7d88-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="c7d88-105">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="c7d88-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="c7d88-106">Это [серию учебников,](web-deployment-in-the-enterprise.md) использует образец решения & #x 2014; диспетчера контактов решения & #x 2014; для представления приложения корпоративного уровня с реалистичных уровень сложности.</span><span class="sxs-lookup"><span data-stu-id="c7d88-106">This [series of tutorials](web-deployment-in-the-enterprise.md) uses a sample solution&#x2014;the Contact Manager solution&#x2014;to represent an enterprise-scale application with a realistic level of complexity.</span></span> <span data-ttu-id="c7d88-107">В этом разделе представлены решения диспетчера контактов, описаны основные компоненты решения и идентифицирует сложностей при развертывании таких приложений на различных платформах назначения в корпоративной среде.</span><span class="sxs-lookup"><span data-stu-id="c7d88-107">This topic introduces the Contact Manager solution, describes the key components of the solution, and identifies the challenges in deploying this kind of application to various destination platforms in an enterprise environment.</span></span>
> 
> <span data-ttu-id="c7d88-108">Как знакомиться с разделами в этих учебниках можно использовать как Эталонная реализация, в котором показано соответствие конкретных проблем, возникающих в корпоративный сценарий развертывания решения диспетчера контактов.</span><span class="sxs-lookup"><span data-stu-id="c7d88-108">As you work through the topics in these tutorials, you can use the Contact Manager solution as a reference implementation that demonstrates how you can meet specific challenges in enterprise deployment scenarios.</span></span> <span data-ttu-id="c7d88-109">Следующий раздел [параметр вверх диспетчера контактов решения](setting-up-the-contact-manager-solution.md), описывает, как загрузить и запустить решение на компьютере разработчика.</span><span class="sxs-lookup"><span data-stu-id="c7d88-109">The next topic, [Setting Up the Contact Manager Solution](setting-up-the-contact-manager-solution.md), describes how to download and run the solution on your developer workstation.</span></span>


## <a name="solution-overview"></a><span data-ttu-id="c7d88-110">Обзор решения</span><span class="sxs-lookup"><span data-stu-id="c7d88-110">Solution Overview</span></span>

<span data-ttu-id="c7d88-111">Диспетчер контактов решение состоит из четырех отдельных проектов:</span><span class="sxs-lookup"><span data-stu-id="c7d88-111">The Contact Manager solution consists of four individual projects:</span></span>

![](the-contact-manager-solution/_static/image1.png)

- <span data-ttu-id="c7d88-112">**ContactManager.Mvc**.</span><span class="sxs-lookup"><span data-stu-id="c7d88-112">**ContactManager.Mvc**.</span></span> <span data-ttu-id="c7d88-113">Это проект веб-приложения ASP.NET MVC 3, представляющий точку входа для решения.</span><span class="sxs-lookup"><span data-stu-id="c7d88-113">This is an ASP.NET MVC 3 web application project that represents the entry point for the solution.</span></span> <span data-ttu-id="c7d88-114">Он предлагает некоторые основные web функциональные возможности приложений, как предоставить пользователям возможность создавать и просматривать сведения о контакте.</span><span class="sxs-lookup"><span data-stu-id="c7d88-114">It offers some basic web application functionality, like providing users with the ability to create and view contact details.</span></span> <span data-ttu-id="c7d88-115">Приложение зависит от службы Windows Communication Foundation (WCF) для управления контактами и базу данных служб приложения ASP.NET для управления проверки подлинности и авторизации.</span><span class="sxs-lookup"><span data-stu-id="c7d88-115">The application relies on a Windows Communication Foundation (WCF) service to manage contacts and an ASP.NET application services database to manage authentication and authorization.</span></span>
- <span data-ttu-id="c7d88-116">**ContactManager.Database**.</span><span class="sxs-lookup"><span data-stu-id="c7d88-116">**ContactManager.Database**.</span></span> <span data-ttu-id="c7d88-117">Это проект базы данных Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c7d88-117">This is a Visual Studio database project.</span></span> <span data-ttu-id="c7d88-118">Проект определяет схему для базы данных, сведения о контакте хранилищ.</span><span class="sxs-lookup"><span data-stu-id="c7d88-118">The project defines the schema for a database that stores contact details.</span></span>
- <span data-ttu-id="c7d88-119">**ContactManager.Service**.</span><span class="sxs-lookup"><span data-stu-id="c7d88-119">**ContactManager.Service**.</span></span> <span data-ttu-id="c7d88-120">Это проект веб-службы WCF.</span><span class="sxs-lookup"><span data-stu-id="c7d88-120">This is a WCF web service project.</span></span> <span data-ttu-id="c7d88-121">Предоставляет службы WCF, создайте конечную точку, которая позволяет вызывающим объектам осуществлять, получения, обновления и удаления (CRUD) на **ContactManager** базы данных.</span><span class="sxs-lookup"><span data-stu-id="c7d88-121">The WCF service exposes an endpoint that allows callers to perform create, retrieve, update, and delete (CRUD) operations on the **ContactManager** database.</span></span> <span data-ttu-id="c7d88-122">Зависит от службы **ContactManager** базы данных и **ContactManager.Common.dll** сборки.</span><span class="sxs-lookup"><span data-stu-id="c7d88-122">The service relies on the **ContactManager** database and the **ContactManager.Common.dll** assembly.</span></span>
- <span data-ttu-id="c7d88-123">**ContactManager.Common**.</span><span class="sxs-lookup"><span data-stu-id="c7d88-123">**ContactManager.Common**.</span></span> <span data-ttu-id="c7d88-124">Это проект библиотеки классов.</span><span class="sxs-lookup"><span data-stu-id="c7d88-124">This is a class library project.</span></span> <span data-ttu-id="c7d88-125">Служба WCF использует типы, определенные в этой сборке.</span><span class="sxs-lookup"><span data-stu-id="c7d88-125">The WCF service relies on types defined in this assembly.</span></span>

<span data-ttu-id="c7d88-126">Решение также содержит папку решения с именем публикации.</span><span class="sxs-lookup"><span data-stu-id="c7d88-126">The solution also includes a solution folder named Publish.</span></span> <span data-ttu-id="c7d88-127">Содержит различные файлы проектов и командные файлы, которые демонстрируют, как можно управлять и работать с процессом построения и развертывания.</span><span class="sxs-lookup"><span data-stu-id="c7d88-127">This contains various custom project files and command files that demonstrate how you can control and manipulate the build and deployment process.</span></span> <span data-ttu-id="c7d88-128">Они описаны более подробно далее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="c7d88-128">These are covered in more detail later in this tutorial.</span></span>

<span data-ttu-id="c7d88-129">На концептуальном уровне компоненты решения соединяются следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c7d88-129">At a conceptual level, the components of the solution fit together like this:</span></span>

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="c7d88-130">Веб-приложение ASP.NET MVC 3 использует поставщик членства ASP.NET, все страницы в веб-приложении разрешить анонимный доступ.</span><span class="sxs-lookup"><span data-stu-id="c7d88-130">While the ASP.NET MVC 3 web application uses the ASP.NET membership provider, all the pages within the web application allow anonymous access.</span></span> <span data-ttu-id="c7d88-131">Это не очевидно реалистичных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c7d88-131">This is clearly not a realistic configuration.</span></span> <span data-ttu-id="c7d88-132">Однако решение настроена таким образом, чтобы упростить развертывание и тестирование решения без настройки учетных записей пользователей и ролей.</span><span class="sxs-lookup"><span data-stu-id="c7d88-132">However, the solution is set up in this way to make it easier for you to deploy and test the solution without configuring user accounts and roles.</span></span>


## <a name="deployment-challenges"></a><span data-ttu-id="c7d88-133">Задачи развертывания</span><span class="sxs-lookup"><span data-stu-id="c7d88-133">Deployment Challenges</span></span>

<span data-ttu-id="c7d88-134">Решение диспетчера контактов рассмотрены несколько проблем при развертывании, являющиеся общими для большого количества корпоративных сценариях развертывания:</span><span class="sxs-lookup"><span data-stu-id="c7d88-134">The Contact Manager solution illustrates several deployment challenges that are common to lots of enterprise deployment scenarios:</span></span>

- <span data-ttu-id="c7d88-135">Решение состоит из нескольких зависимые проекты.</span><span class="sxs-lookup"><span data-stu-id="c7d88-135">The solution consists of multiple dependent projects.</span></span> <span data-ttu-id="c7d88-136">Необходимо развернуть эти проекты одновременно.</span><span class="sxs-lookup"><span data-stu-id="c7d88-136">You need to deploy these projects simultaneously.</span></span>
- <span data-ttu-id="c7d88-137">Строки соединения и конечные точки службы должны быть обновлены для каждой среды, и в большинстве случаев эта информация будет недоступен для разработчика.</span><span class="sxs-lookup"><span data-stu-id="c7d88-137">Connection strings and service endpoints need to be updated for each environment, and in a lot of cases this information will not be available to the developer.</span></span>
- <span data-ttu-id="c7d88-138">При развертывании **ContactManager** базы данных для промежуточной и производственной средах, необходимо сохранить существующие данные в последующих развертываниях.</span><span class="sxs-lookup"><span data-stu-id="c7d88-138">When you deploy the **ContactManager** database to staging and production environments, you need to preserve existing data on subsequent deployments.</span></span>
- <span data-ttu-id="c7d88-139">При развертывании базы данных служб приложения ASP.NET, необходимо развернуть некоторые данные конфигурации, но не указывается данные учетных записей пользователей.</span><span class="sxs-lookup"><span data-stu-id="c7d88-139">When you deploy the ASP.NET application services database, you need to deploy some configuration data but omit any user account data.</span></span>
- <span data-ttu-id="c7d88-140">Проекты содержат файлы и папки, которые не должны быть развернуты.</span><span class="sxs-lookup"><span data-stu-id="c7d88-140">The projects include some files and folders that should not be deployed.</span></span> <span data-ttu-id="c7d88-141">Необходимо исключить эти файлы и папки из процесса развертывания.</span><span class="sxs-lookup"><span data-stu-id="c7d88-141">You need to exclude these files and folders from the deployment process.</span></span>
- <span data-ttu-id="c7d88-142">Решение должна поддерживать автоматическое развертывание с сервера построения Team Foundation Server (TFS).</span><span class="sxs-lookup"><span data-stu-id="c7d88-142">The solution needs to support automated deployment from a Team Foundation Server (TFS) build server.</span></span>

## <a name="conclusion"></a><span data-ttu-id="c7d88-143">Заключение</span><span class="sxs-lookup"><span data-stu-id="c7d88-143">Conclusion</span></span>

<span data-ttu-id="c7d88-144">В этом разделе предоставлен общий обзор решения диспетчера контактов и определены некоторые специфические развертывания задач, которые являются общими для большого количества корпоративный сценарий развертывания.</span><span class="sxs-lookup"><span data-stu-id="c7d88-144">This topic provided a high-level overview of the Contact Manager solution and identified some of the inherent deployment challenges that are common to lots of enterprise deployment scenarios.</span></span> <span data-ttu-id="c7d88-145">Остальные разделы в этом руководстве описываются некоторые методы, которые можно использовать для выполнения этих задач.</span><span class="sxs-lookup"><span data-stu-id="c7d88-145">The remaining topics in this tutorial describe some of the techniques you can use to meet these challenges.</span></span>

<span data-ttu-id="c7d88-146">Следующий раздел [параметр вверх диспетчера контактов решения](setting-up-the-contact-manager-solution.md), описывает, как загрузить и запустить решение на компьютере разработчика.</span><span class="sxs-lookup"><span data-stu-id="c7d88-146">The next topic, [Setting Up the Contact Manager Solution](setting-up-the-contact-manager-solution.md), describes how to download and run the solution on your developer workstation.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c7d88-147">[Назад](web-deployment-in-the-enterprise.md)
[Вперед](setting-up-the-contact-manager-solution.md)</span><span class="sxs-lookup"><span data-stu-id="c7d88-147">[Previous](web-deployment-in-the-enterprise.md)
[Next](setting-up-the-contact-manager-solution.md)</span></span>
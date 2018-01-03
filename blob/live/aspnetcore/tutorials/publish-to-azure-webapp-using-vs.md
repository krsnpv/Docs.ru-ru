---
title: "Публикация приложения ASP.NET Core в Azure с помощью Visual Studio"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: 6f697ed4d8876a19cd058533e4f6a5d4f7cdc2fb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a><span data-ttu-id="1c3f8-103">Публикация веб-приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c3f8-103">Publish an ASP.NET Core web app to Azure App Service using Visual Studio</span></span>

<span data-ttu-id="1c3f8-104">Авторы: [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT), [Сезар Блум Сильвейра (Cesar Blum Silveira)](https://github.com/cesarbs) и [Рейчел Аппель (Rachel Appel)](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="1c3f8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs), and [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="1c3f8-105">Если вы работаете на компьютере Mac, см. раздел [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) (Публикация в Azure из Visual Studio для компьютера Mac).</span><span class="sxs-lookup"><span data-stu-id="1c3f8-105">See [Publish to Azure from Visual Studio for Mac](https://blog.xamarin.com/publish-azure-visual-studio-mac/) if you are working on a Mac.</span></span>

## <a name="set-up"></a><span data-ttu-id="1c3f8-106">Настройка</span><span class="sxs-lookup"><span data-stu-id="1c3f8-106">Set up</span></span>

* <span data-ttu-id="1c3f8-107">Создайте [бесплатную учетную запись Azure](https://aka.ms/K5y5yh), если у вас ее нет.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-107">Open a [free Azure account](https://aka.ms/K5y5yh) if you do not have one.</span></span> 

## <a name="create-a-web-app"></a><span data-ttu-id="1c3f8-108">Создание веб-приложения</span><span class="sxs-lookup"><span data-stu-id="1c3f8-108">Create a web app</span></span>

<span data-ttu-id="1c3f8-109">На начальной странице Visual Studio последовательно выберите **Файл > Создать > Проект…**</span><span class="sxs-lookup"><span data-stu-id="1c3f8-109">In the Visual Studio Start Page, select **File > New > Project...**</span></span>

![меню "Файл"](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

<span data-ttu-id="1c3f8-111">В диалоговом окне **Создание проекта** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-111">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="1c3f8-112">В левой области выберите **.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-112">In the left pane, select **.NET Core**.</span></span>

* <span data-ttu-id="1c3f8-113">В центральной области выберите **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-113">In the center pane, select **ASP.NET Core Web Application**.</span></span>

* <span data-ttu-id="1c3f8-114">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-114">Select **OK**.</span></span>

![Диалоговое окно создания нового проекта](publish-to-azure-webapp-using-vs/_static/new_prj.png)

<span data-ttu-id="1c3f8-116">В диалоговом окне **Создание веб-приложения ASP.NET Core** сделайте следующее.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-116">In the **New ASP.NET Core Web Application** dialog:</span></span>

* <span data-ttu-id="1c3f8-117">Выберите **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-117">Select **Web Application**.</span></span>

* <span data-ttu-id="1c3f8-118">Выберите **Изменить способ проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-118">Select **Change Authentication**.</span></span>

![Диалоговое окно создания нового проекта](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

<span data-ttu-id="1c3f8-120">Появится диалоговое окно **Изменение способа проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-120">The **Change Authentication** dialog appears.</span></span> 

* <span data-ttu-id="1c3f8-121">Выберите **Учетные записи отдельных пользователей**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-121">Select **Individual User Accounts**.</span></span>

* <span data-ttu-id="1c3f8-122">Нажмите кнопку **ОК**, чтобы вернуться на страницу **Создание веб-приложения ASP.NET Core**, а затем снова нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-122">Select **OK** to return to the **New ASP.NET Core Web Application**, then select **OK** again.</span></span>

![Диалоговое окно "Новый способ веб-проверки подлинности ASP.NET Core"](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

<span data-ttu-id="1c3f8-124">Visual Studio создает решение.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-124">Visual Studio creates the solution.</span></span>

## <a name="run-the-app-locally"></a><span data-ttu-id="1c3f8-125">Локальный запуск приложения</span><span class="sxs-lookup"><span data-stu-id="1c3f8-125">Run the app locally</span></span>

* <span data-ttu-id="1c3f8-126">Выберите **Отладка**, а затем **Запустить без отладки**, чтобы запустить приложение локально.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-126">Choose **Debug** then **Start Without Debugging** to run the app locally.</span></span>

* <span data-ttu-id="1c3f8-127">Щелкните ссылки **Сведения** и **Контакты**, чтобы проверить работу веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-127">Click the **About** and **Contact** links to verify the web application works.</span></span>

![Веб-приложение откроется в localhost.](publish-to-azure-webapp-using-vs/_static/show.png)

* <span data-ttu-id="1c3f8-129">Выберите **Зарегистрироваться** и зарегистрируйте нового пользователя.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-129">Select **Register** and register a new user.</span></span> <span data-ttu-id="1c3f8-130">Можно использовать вымышленный адрес электронной почты.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-130">You can use a fictitious email address.</span></span> <span data-ttu-id="1c3f8-131">После отправки на странице отображается следующая ошибка.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-131">When you submit, the page displays the following error:</span></span>

    <span data-ttu-id="1c3f8-132">*"Внутренняя ошибка сервера: не удалось выполнить операцию базы данных при обработке запроса. Исключения SQL: не удается открыть базу данных. Применение имеющихся миграций для контекста базы данных приложения, возможно, позволит решить проблему."*</span><span class="sxs-lookup"><span data-stu-id="1c3f8-132">*"Internal Server Error: A database operation failed while processing the request. SQL exception: Cannot open the database. Applying existing migrations for Application DB context may resolve this issue."*</span></span>

* <span data-ttu-id="1c3f8-133">Выберите **Применить миграции** и после обновления страницы перезагрузите ее.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-133">Select **Apply Migrations** and, once the page updates, refresh the page.</span></span>

![Внутренняя ошибка сервера: сбой операции из базы данных при обработке запроса.](publish-to-azure-webapp-using-vs/_static/mig.png)

<span data-ttu-id="1c3f8-137">Приложение отобразит адрес электронной почты, который использовался для регистрации нового пользователя, и ссылку **Выйти**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-137">The app displays the email used to register the new user and a **Log out** link.</span></span>

![Веб-приложение откроется в Microsoft Edge.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a><span data-ttu-id="1c3f8-140">Развертывание приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="1c3f8-140">Deploy the app to Azure</span></span>

<span data-ttu-id="1c3f8-141">Закройте веб-страницу, вернитесь в Visual Studio и выберите **Остановить отладку** в меню **Отладка**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-141">Close the web page, return to Visual Studio, and select **Stop Debugging** from the **Debug** menu.</span></span>

<span data-ttu-id="1c3f8-142">В обозревателе решений щелкните правой кнопкой мыши проект и выберите команду **Опубликовать…**</span><span class="sxs-lookup"><span data-stu-id="1c3f8-142">Right-click on the project in Solution Explorer and select **Publish...**.</span></span>

![Открытое контекстное меню с выделенной ссылкой "Опубликовать"](publish-to-azure-webapp-using-vs/_static/pub.png)

<span data-ttu-id="1c3f8-144">В диалоговом окне **Публикация** выберите **Служба приложений Microsoft Azure** и щелкните **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-144">In the **Publish** dialog, select **Microsoft Azure App Service** and click **Publish**.</span></span>

![Диалоговое окно публикации](publish-to-azure-webapp-using-vs/_static/maas1.png)

* <span data-ttu-id="1c3f8-146">Присвойте приложению уникальное имя.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-146">Name the app a unique name.</span></span> 

* <span data-ttu-id="1c3f8-147">Выберите подписку.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-147">Select a subscription.</span></span>

* <span data-ttu-id="1c3f8-148">Щелкните **Создать…** рядом с группой ресурсов и введите имя для новой группы ресурсов.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-148">Select **New...** for the resource group and enter a name for the new resource group.</span></span>

* <span data-ttu-id="1c3f8-149">Щелкните **Создать…** рядом с планом службы приложений и выберите ближайшее к вам расположение.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-149">Select **New...** for the app service plan and select a location near you.</span></span> <span data-ttu-id="1c3f8-150">Вы можете сохранить имя, созданное по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-150">You can keep the name that is generated by default.</span></span>

![Диалоговое окно "Служба приложений"](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* <span data-ttu-id="1c3f8-152">Выберите вкладку **Службы**, чтобы создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-152">Select the **Services** tab to create a new database.</span></span>

* <span data-ttu-id="1c3f8-153">Выберите зеленый значок **+**, чтобы создать базу данных SQL.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-153">Select the green **+** icon to create a new SQL Database</span></span>

![Новая база данных SQL](publish-to-azure-webapp-using-vs/_static/sql.png)

* <span data-ttu-id="1c3f8-155">Щелкните **Создать…** в диалоговом окне **Настройка базы данных SQL**, чтобы создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-155">Select **New...** on the **Configure SQL Database** dialog to create a new database.</span></span>

![Новый сервер и база данных SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

<span data-ttu-id="1c3f8-157">Появится диалоговое окно **Настройка SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-157">The **Configure SQL Server** dialog appears.</span></span>

* <span data-ttu-id="1c3f8-158">Введите имя пользователя и пароль администратора, а затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-158">Enter an administrator user name and password, and then select **OK**.</span></span> <span data-ttu-id="1c3f8-159">Не забудьте имя пользователя и пароль, созданные на этом шаге.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-159">Don't forget the user name and password you create in this step.</span></span> <span data-ttu-id="1c3f8-160">Вы можете сохранить **Имя сервера** по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-160">You can keep the default **Server Name**.</span></span> 

* <span data-ttu-id="1c3f8-161">Введите имена базы данных и строки подключения.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-161">Enter names for the database and connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="1c3f8-162">"admin" не может использоваться в качестве имени пользователя администратора.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-162">"admin" is not allowed as the administrator user name.</span></span>

![Диалоговое окно "Настройка SQL Server"](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* <span data-ttu-id="1c3f8-164">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-164">Select **OK**.</span></span>

<span data-ttu-id="1c3f8-165">Visual Studio вернет диалоговое окно **Создание службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-165">Visual Studio returns to the **Create App Service** dialog.</span></span>

* <span data-ttu-id="1c3f8-166">Нажмите кнопку **Создать** в диалоговом окне **Создание службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-166">Select **Create** on the **Create App Service** dialog.</span></span>

![Диалоговое окно "Настройка базы данных SQL"](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* <span data-ttu-id="1c3f8-168">Щелкните ссылку **Параметры** в диалоговом окне **Публикация**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-168">Click the **Settings** link in the **Publish** dialog.</span></span>

![Диалоговое окно "Публикация": панель "Подключение"](publish-to-azure-webapp-using-vs/_static/pubc.png)

<span data-ttu-id="1c3f8-170">На странице **Параметры** диалогового окна **Публикация**</span><span class="sxs-lookup"><span data-stu-id="1c3f8-170">On the **Settings** page of the **Publish** dialog:</span></span>

  * <span data-ttu-id="1c3f8-171">Разверните раздел **Базы данных** и установите флажок **Использовать эту строку подключения во время выполнения**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-171">Expand **Databases** and check **Use this connection string at runtime**.</span></span>

  * <span data-ttu-id="1c3f8-172">Разверните раздел **Миграции Entity Framework** и установите флажок **Использовать эту миграцию при публикации**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-172">Expand **Entity Framework Migrations** and check **Apply this migration on publish**.</span></span>

* <span data-ttu-id="1c3f8-173">Нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-173">Select **Save**.</span></span> <span data-ttu-id="1c3f8-174">Visual Studio вернется в диалоговое окно **Публикация**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-174">Visual Studio returns to the **Publish** dialog.</span></span> 

![Диалоговое окно "Публикация": панель "Параметры"](publish-to-azure-webapp-using-vs/_static/pubs.png)

<span data-ttu-id="1c3f8-176">Нажмите кнопку **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-176">Click **Publish**.</span></span> <span data-ttu-id="1c3f8-177">Visual Studio опубликует приложение в Azure и запустит облачное приложение в браузере.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-177">Visual Studio will publish your app to Azure and launch the cloud app in your browser.</span></span>

### <a name="test-your-app-in-azure"></a><span data-ttu-id="1c3f8-178">Тестирование приложения в Azure</span><span class="sxs-lookup"><span data-stu-id="1c3f8-178">Test your app in Azure</span></span>

* <span data-ttu-id="1c3f8-179">Протестируйте ссылки **О программе** и **Контакт**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-179">Test the **About** and **Contact** links</span></span>

* <span data-ttu-id="1c3f8-180">Регистрация нового пользователя</span><span class="sxs-lookup"><span data-stu-id="1c3f8-180">Register a new user</span></span>

![Веб-приложение, открытое в Microsoft Edge в службе приложений Azure](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a><span data-ttu-id="1c3f8-182">Обновление приложения</span><span class="sxs-lookup"><span data-stu-id="1c3f8-182">Update the app</span></span>

* <span data-ttu-id="1c3f8-183">Измените страницу Razor *Pages/About.cshtml* и ее содержимое.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-183">Edit the *Pages/About.cshtml* Razor page and change its contents.</span></span> <span data-ttu-id="1c3f8-184">Например, вы можете изменить абзац на "Hello ASP.NET Core!":</span><span class="sxs-lookup"><span data-stu-id="1c3f8-184">For example, you can modify the paragraph to say "Hello ASP.NET Core!":</span></span>

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* <span data-ttu-id="1c3f8-185">Щелкните правой кнопкой мыши проект и снова выберите пункт **Опубликовать...**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-185">Right-click on the project and select **Publish...** again.</span></span>

![Открытое контекстное меню с выделенной ссылкой "Опубликовать"](publish-to-azure-webapp-using-vs/_static/pub.png)

* <span data-ttu-id="1c3f8-187">После публикации приложения убедитесь, что внесенные изменения доступны в Azure.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-187">After the app is published, verify the changes you made are available on Azure.</span></span>

![Убедитесь, что задача завершена.](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a><span data-ttu-id="1c3f8-189">Очистка</span><span class="sxs-lookup"><span data-stu-id="1c3f8-189">Clean up</span></span>

<span data-ttu-id="1c3f8-190">Завершив тестирование приложения, перейдите на [портал Azure](https://portal.azure.com/) и удалите приложение.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-190">When you have finished testing the app, go to the [Azure portal](https://portal.azure.com/) and delete the app.</span></span>

* <span data-ttu-id="1c3f8-191">Выберите пункт **Группы ресурсов**, а затем созданную группу ресурсов.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-191">Select **Resource groups**, then select the resource group you created.</span></span>

![Портал Azure: пункт "Группы ресурсов" в меню боковой панели](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* <span data-ttu-id="1c3f8-193">На странице **Группы ресурсов** выберите **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-193">In the **Resource groups** page, select **Delete**.</span></span>

![Портал Azure: страница "Группы ресурсов"](publish-to-azure-webapp-using-vs/_static/rgd.png)

* <span data-ttu-id="1c3f8-195">Введите имя группы ресурсов и выберите **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-195">Enter the name of the resource group and select **Delete**.</span></span> <span data-ttu-id="1c3f8-196">Ваше приложение и все ресурсы, созданные при работе с этим руководством, удалены из Azure.</span><span class="sxs-lookup"><span data-stu-id="1c3f8-196">Your app and all other resources created in this tutorial are now deleted from Azure.</span></span>

### <a name="next-steps"></a><span data-ttu-id="1c3f8-197">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="1c3f8-197">Next steps</span></span>

* [<span data-ttu-id="1c3f8-198">Непрерывное развертывание в Azure с помощью Visual Studio и Git</span><span class="sxs-lookup"><span data-stu-id="1c3f8-198">Continuous Deployment to Azure with Visual Studio and Git</span></span>](../publishing/azure-continuous-deployment.md)
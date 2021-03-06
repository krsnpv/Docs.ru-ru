---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: "Отображение страницы настраиваемой ошибки (C#) | Документы Microsoft"
author: rick-anderson
description: "Что пользователю видеть при возникновении ошибки времени выполнения в веб-приложение ASP.NET? Ответ зависит от как веб-сайта &lt;customErrors&gt; конфигурации..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 92a7e945a6f82e78b848bae8f4f362e16a567b1f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="displaying-a-custom-error-page-c"></a>Отображение страницы настраиваемой ошибки (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить код](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip) или [скачать PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> Что пользователю видеть при возникновении ошибки времени выполнения в веб-приложение ASP.NET? Ответ зависит от как веб-сайта &lt;customErrors&gt; конфигурации. По умолчанию пользователям предоставляется соединительных желтый экрана, proclaiming, что произошла ошибка среды выполнения. Этого учебника показано, как настроить эти параметры отображения красоту-при помощи пользовательскую страницу ошибки, соответствующий внешний вид и поведение веб-узла.


## <a name="introduction"></a>Вступление

В идеальном мире бы без ошибок во время выполнения. Программисты написать код с n-арных объектов ошибки и с помощью надежной проверки ввода данных и внешние ресурсы, такие как серверы баз данных и электронной почты испытывали бы вне сети. Конечно в действительности неизбежны ошибки. Классы в .NET Framework сообщения об ошибке, создавая исключение. Например метод Open для объекта вызова объекта SqlConnection устанавливает соединение для базы данных, указанной строкой подключения. Тем не менее, если базы данных не работает или недопустимые учетные данные в строке подключения затем метод Open вызывает `SqlException`. Исключения могут быть обработаны с помощью `try/catch/finally` блоков. Если код в `try` блок вызывает исключение, управление передается блоку catch соответствующие которой разработчик может попытаться восстановить из-за ошибки. Если нет соответствующий блок catch или если код, который вызвал исключение не находится в блоке try, исключение percolates по стеку вызовов в search `try/catch/finally` блоков.

Если исключение выносит вплоть до выполнения ASP.NET без обработки, [ `HttpApplication` класса](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.aspx) [ `Error` событий](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.error.aspx) возникает и настроенного *страницы ошибок*  отображается. По умолчанию ASP.NET отображается страница ошибки, его называют разработчики называется [желтый экрана смерти](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow) (YSOD). Существуют две версии YSOD: один показывает сведения об исключении, трассировку стека и другие сведения полезны для разработчиков, отладки приложения (см. **рис. 1**); другой просто указывает, что произошла ошибка во время выполнения (см. в разделе  **На рисунке 2**).

Сведения об исключении YSOD весьма полезна для разработчиков, отладку приложения, но отображение YSOD конечным пользователям tacky и непрофессионально. Вместо этого пользователям попадете на страницу ошибки, который поддерживает внешний вид и поведение веб-узла с более удобный прозы, описывающее ситуацию. Хорошие новости является создание страницы настраиваемой ошибки довольно просто. Этот учебник начинается с рассмотрения ASP. Страницы в NET различных ошибок. Затем показано, как настроить веб-приложение для отображения пользователей собственную страницу ошибки при возникновении ошибки.

### <a name="examining-the-three-types-of-error-pages"></a>Изучение три вида страницы ошибок

Если возникает необработанное исключение в приложении ASP.NET, один из трех типов страниц ошибок отображается:

- Исключение сведения желтый экрана из смерти страницы ошибки
- Страницу ошибок времени выполнения ошибки желтый экрана из смерти или
- Страницы настраиваемой ошибки

Разработчики странице ошибка, знакома с YSOD сведения об исключении. По умолчанию эта страница отображается для пользователей, которые посещали локально, следовательно страницы, которое будет отображаться при возникновении ошибки во время тестирования сайта в среде разработки. Как и предполагает его имя, YSOD сведения об исключении предоставляет подробные сведения об исключении - тип, сообщение и трассировка стека. Более того Если было создано исключение из кода в классе кода страницы ASP.NET, и если приложение настроено для отладки YSOD сведения об исключении также будет показано эту строку кода (и несколько строк кода выше и ниже).

**Рис. 1** показана страница YSOD сведения об исключении. Запишите URL-адрес в браузере адрес: `http://localhost:62275/Genre.aspx?ID=foo`. Помните, что `Genre.aspx` странице перечислены рецензий определенного жанра. Необходимо, `GenreId` значение ( `uniqueidentifier`) нужно передать строку запроса; например, соответствующий URL-адрес, чтобы просмотреть обзоры фантастики — `Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`. Если значение, отличное от`uniqueidentifier` значение передается в через строку запроса (например, «foo») создается исключение.

> [!NOTE]
> Воспроизведение ошибки в веб-приложении Демонстрация доступны для загрузки вы найдете `Genre.aspx?ID=foo` напрямую или щелкните ссылку «Создать ошибку во время выполнения» в `Default.aspx`.


Обратите внимание, сведения об исключении, представленных в **рис. 1**. Сообщение об исключении, «ошибка при преобразовании строки символов в тип uniqueidentifier» присутствует в верхней части страницы. Тип исключения, `System.Data.SqlClient.SqlException`, присутствует в списке, а также. Имеется также трассировку стека.

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**Рис. 1**: сведения об исключении YSOD включает сведения об исключении  
 ([Просмотр полноразмерное изображение](displaying-a-custom-error-page-cs/_static/image3.png))

Тип YSOD YSOD ошибки времени выполнения и отображается в **рис. 2**. YSOD ошибка среды выполнения сообщает посетителя, произошла ошибка во время выполнения, но не включает все сведения об исключении, которое было создано. (Тем не менее содержат инструкции по созданию можно просматривать сведения об ошибке, изменив `Web.config` файл, который является частью делает таких YSOD выглядеть непрофессионально.)

По умолчанию YSOD ошибка среды выполнения отображается для посетителей удаленно (с помощью http://www.yoursite.com), о чем свидетельствует URL-адрес в адресную строку браузера **рис. 2**: `http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`. Существуют две различные YSOD экраны, поскольку разработчики, хочет знать, сведения об ошибке, но такую информацию, не должен отображаться на действующем сайте, как он может раскрыть потенциальным уязвимостям безопасности и другую важную информацию для тех, кто посещает ваш веб-узел.

> [!NOTE]
> Если вы следуете и используете DiscountASP.NET как ваш веб-узел, вы можете заметить, что YSOD ошибка среды выполнения не отображаются при просмотре на действующем сайте. Это происходит потому DiscountASP.NET имеет свои серверы, по умолчанию настроен на отображение YSOD сведения об исключении. Хорошо то, что это поведение по умолчанию можно переопределить путем добавления `<customErrors>` раздел вашей `Web.config` файла. В разделе «Настройка которой ошибка откроется страница» проверяет `<customErrors>` раздела подробно.


[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**На рисунке 2**: Ошибка выполнения YSOD не включает все сведения об ошибке  
 ([Просмотр полноразмерное изображение](displaying-a-custom-error-page-cs/_static/image6.png))

Третий тип страницы ошибок является настраиваемую страницу ошибки, который является веб-страницы, создаваемой. Преимущество собственную страницу ошибки заключается в наличии полный контроль над информацию, которая отображается для пользователя вместе с внешний вид страницы; настраиваемой страницы ошибки можно использовать одну главную страницу и стили, как и на другие страницы. В разделе «Использование пользовательской страницы ошибок» вы научитесь создавать собственную страницу ошибки и ее настройка для отображения в случае необработанное исключение. **Рис. 3** предлагает о внутреннем данную страницу. Как видите, внешний вид страницы ошибки является более профессионально чем любая желтый экраны из смерти показано на рис. 1 и 2.

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**Рис. 3**: собственную страницу ошибки предлагает более специализированных внешний вид и поведение  
 ([Просмотр полноразмерное изображение](displaying-a-custom-error-page-cs/_static/image9.png))

Теперь пора проверить адресную строку браузера **рис. 3**. Обратите внимание, что в адресной строке отображается URL-адрес настраиваемой страницы ошибки (`/ErrorPages/Oops.aspx`). На рисунках 1 и 2 желтый экраны смерти отображаются на одной странице, которая возникла ошибка из (`Genre.aspx`). Настраиваемой страницы ошибки передается URL-адрес страницы, где произошла ошибка через `aspxerrorpath` параметр строки запроса.

## <a name="configuring-which-error-page-is-displayed"></a>Настройка страницы ошибки которого отображается

Отображаемые на трех страницах возможна ошибка основан на двух переменных:

- Сведения о конфигурации в `<customErrors>` раздела, и
- Ли пользователь посещает сайт локально или удаленно.

[ `<customErrors>` Раздел](https://msdn.microsoft.com/en-us/library/h0hfz6fc.aspx) в `Web.config` имеет два атрибута, которые влияют на какой странице ошибка отображается: `defaultRedirect` и `mode`. Атрибут `defaultRedirect` является необязательным. Если указано, оно указывает URL-адрес настраиваемой страницы ошибки и указывает отображение настраиваемой страницы ошибки вместо YSOD ошибки среды выполнения. `mode` Атрибут является обязательным и принимает одно из трех значений: `On`, `Off`, или `RemoteOnly`. Эти значения имеют следующие особенности:

- `On`-Указывает, настраиваемой страницы ошибки или YSOD ошибка среды выполнения отображается для всех посетителей, независимо от того, являются ли они локальными или удаленными.
- `Off`-Указывает, что YSOD сведения об исключении отображаются для всех посетителей, независимо от того, являются ли они локальными или удаленными.
- `RemoteOnly`-Указывает, что настраиваемой страницы ошибки или YSOD ошибка среды выполнения отображается удаленного посетителям во время локального посетителям отображается YSOD сведения об исключении.

Если не указано иное, ASP.NET действует так, будто установить атрибут режима в `RemoteOnly` и не был указан `defaultRedirect` значение. Другими словами по умолчанию выполняется отображения YSOD сведения об исключении для локального посетителей пока YSOD ошибка среды выполнения отображается для удаленного посетителей. Можно переопределить это поведение по умолчанию, добавив `<customErrors>` раздел в веб-приложение`Web.config file.`

## <a name="using-a-custom-error-page"></a>С помощью страницы настраиваемой ошибки

Каждое веб-приложение должно иметь собственную страницу ошибки. Он обеспечивает альтернативу профессиональный вид YSOD ошибка среды выполнения, легко создавать и настройка приложения для использования настраиваемой страницы ошибки принимает только несколько секунд. Первым шагом является создание настраиваемой страницы ошибки. Я добавил новую папку с именем приложения рецензий `ErrorPages` и добавить новую страницу ASP.NET с именем `Oops.aspx`. Иметь использовать одну главную страницу как остальная часть страницы на веб-узла, чтобы она автоматически наследует же внешний вид страницы.

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**Рис. 4**: создать собственную страницу ошибки

Далее Потратьте несколько минут, создания содержимого для страницы ошибки. Я создал довольно простой собственную страницу ошибки сообщения, указывающую, что произошла непредвиденная ошибка и обратной ссылки на домашнюю страницу веб-узла.

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**Рис. 5**: разрабатывать страницы настраиваемой ошибки  
 ([Просмотр полноразмерное изображение](displaying-a-custom-error-page-cs/_static/image14.png))

Страницы ошибок, завершено настройте веб-приложение для использования настраиваемой страницы ошибки вместо YSOD ошибки среды выполнения. Это достигается путем указания URL-адрес страницы ошибки в `<customErrors>` раздела `defaultRedirect` атрибута. Добавьте следующую разметку для вашего приложения `Web.config` файла:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

Выше разметки настраивает приложение для отображения YSOD сведения об исключении для посетителей локально, при использовании настраиваемой страницы ошибки Oops.aspx для тех пользователей, посетив удаленно. Чтобы увидеть это в действии, развертывание веб-сайт в рабочей среде и затем перейдите на страницу Genre.aspx на действующем сайте со значением недопустимые строки запроса. Вы увидите настраиваемой страницы ошибки (обращаться к **рис. 3**).

Чтобы убедиться, что настраиваемой страницы ошибки отображается только для удаленных пользователей, посетите `Genre.aspx` страница с Недопустимая строка запроса из среды разработки. Вы увидите по-прежнему YSOD сведения об исключении (обращаться к **рис. 1**). `RemoteOnly` Параметр гарантирует, что пользователей, посетив этот сайт в рабочей среде см. на странице настраиваемой ошибки разработчиков, работающих локально продолжено см. Подробности исключения.

## <a name="notifying-developers-and-logging-error-details"></a>Уведомления разработчиков и записи сведений об ошибках

Ошибки, возникающие в среде разработки были вызваны на своем компьютере разработчика. Она отображается информация об исключениях в YSOD сведения об исключении, и ей известно, какие действия, она при выполнении произошла ошибка. Однако при возникновении ошибки в рабочей среде, разработчик может не знает, что произошла ошибка, если конечный пользователь на узле времени сообщить об ошибке. И даже если пользователь покидает его способ предупреждения разработчиков, произошла ошибка, не зная тип исключения, сообщение и трассировка стека, он может быть трудно диагностировать причину ошибки, не говоря ее устранению.

По этой причине, возможно, это важнейшей регистрацию ошибок в рабочей среде, некоторые из постоянного хранилища (например, база данных) и что разработчики оповещения этой ошибки. Настраиваемой страницы ошибки может показаться лучше всего сделать это ведение журнала и уведомления. К сожалению настраиваемой страницы ошибки не имеет доступа к сведения об ошибке и поэтому не может использоваться для входа эти сведения. Хорошие новости — существует несколько способов для перехвата сведения об ошибке, а также их регистрации, что в этом разделе более подробно исследовать трех следующих руководствах.

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>С помощью различных настраиваемые страницы ошибок для состояния различных ошибок HTTP

Если исключение возникает страницы ASP.NET и не обрабатывается, до выполнения ASP.NET, которая отображает страницу ошибок настроенного percolates исключение. Если запрос поступает в механизм ASP.NET, но не может обрабатываться по некоторым причинам — возможно запрошенный файл не найдена или не читать разрешения отключены для файла -, а затем вызывает обработчик ASP.NET `HttpException`. Это исключение, как исключения, вызванные из страниц ASP.NET выносит во время выполнения, для отображения страницы соответствующее сообщение об ошибке.

Для веб-приложения в рабочей среде это значит, что если пользователь запрашивает страницу, не найден, то он увидит настраиваемой страницы ошибки. **Рис. 6** показывает этот пример. Так как запрос для страницы несуществующий (`NoSuchPage.aspx`), `HttpException` возникает исключение и отображения настраиваемой страницы ошибки (Обратите внимание, ссылку на `NoSuchPage.aspx` в `aspxerrorpath` параметр строки запроса).

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**Рис. 6**: среда выполнения ASP.NET отображает настройки страницы в ответ на ошибку недопустимый запрос ([Просмотр полноразмерное изображение](displaying-a-custom-error-page-cs/_static/image17.png))

По умолчанию все типы ошибок вызывают же настраиваемой страницы ошибки для отображения. Тем не менее, можно указать другой собственную страницу ошибки для конкретных HTTP состояние кода с помощью `<error>` дочерние элементы в пределах `<customErrors>` раздела. Например, для различных ошибок страницы отображается в случае страницы, не удалось найти ошибки, которая содержит код состояния HTTP 404, обновить `<customErrors>` раздел, включив следующую разметку:

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

После этого изменения на месте всякий раз, когда пользователю, просматривающему удаленно запрашивает ресурс ASP.NET, который не существует, они будут перенаправляться в `404.aspx` пользовательскую страницу ошибки вместо `Oops.aspx`. Как **рис. 7** видно, `404.aspx` страница может содержать более конкретное сообщение чем общие настраиваемой страницы ошибки.

> [!NOTE]
> Извлечение [404 страницы ошибок, один раз более](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/) рекомендации по созданию эффективной страницы ошибки 404.


[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)**Рис. 7**: более целенаправленную сообщений, чем отображается страница ошибки 404 с помощью пользовательской`Oops.aspx`  
 ([Просмотр полноразмерное изображение](displaying-a-custom-error-page-cs/_static/image20.png)) 

Так как известно, что `404.aspx` страницы достигается только в том случае, когда пользователь делает запрос страницы, не найден, можно улучшить данную страницу для включения функции, чтобы помочь пользователю устранить этот особого типа ошибки. Например, можно построить таблицу базы данных, которая сопоставляет известные неправильный URL-адреса на хорошо URL-адреса и затем `404.aspx` пользовательских ошибок страницы выполнить запрос к таблице и предложить страниц, пользователь может пытаться для доступа.

> [!NOTE]
> Настраиваемой страницы ошибки отображается только при выполнении запроса к ресурсу, выполняемая подсистемой ASP.NET. Как уже говорилось в [основные различия между IIS и ASP.NET Development Server](core-differences-between-iis-and-the-asp-net-development-server-cs.md) учебника, веб-сервер может обрабатывать определенные запросы сам. По умолчанию IIS веб-сервер обрабатывает запросы для статического содержимого, например, изображения и HTML-файлы без вызова обработчика ASP.NET. Следовательно Если пользователь запрашивает файл образа несуществующий получат обратно сообщение об ошибке 404 по умолчанию в IIS, а не ASP. NET настроить страницу ошибки.


## <a name="summary"></a>Сводка

При возникновении необработанного исключения в приложении ASP.NET выводится одно из трех страниц ошибок: исключение сведения желтый экрана из смерти; Ошибка выполнения желтый экрана смерти; или собственную страницу ошибки. Ошибка-страница зависит от приложения `<customErrors>` конфигурации и является ли пользователь посещает локально или удаленно. Поведение по умолчанию — Показать YSOD подробности исключения для локального посетителей и YSOD ошибки среды выполнения для удаленного посетителей.

Хотя YSOD ошибка среды выполнения скрывает конфиденциальные сведения от пользователя на узле, он прерывается из внешний вид и поведение веб-узла и делает вид ошибка здесь есть приложения. Рекомендуется использовать собственную страницу ошибки, который указывает на создание и проектирование настраиваемой страницы ошибки и указав его URL-адрес в `<customErrors>` раздела `defaultRedirect` атрибута. Может содержать несколько страниц настраиваемых ошибок для различные состояния ошибки HTTP.

Настраиваемой страницы ошибки является первым шагом в комплексное ошибок стратегии для веб-сайта в производственной среде. Разработчик ошибки, предупреждения и ведение журнала сведений о нем также являются важными шагами. Следующие три учебники по изучению приемов уведомления об ошибках и ведения журнала.

Программирование довольны!

### <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, рассматриваемые в этом учебнике см. в следующих ресурсах:

- [Страницы ошибок, еще раз](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [Правила разработки исключений](https://msdn.microsoft.com/en-us/library/ms229014.aspx)
- [Страницы ошибок удобной для пользователей](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Обработка и создание исключений](https://msdn.microsoft.com/en-us/library/5b2yeyab.aspx)
- [Надлежащим образом при использовании пользовательских страниц ошибок в ASP.NET](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

>[!div class="step-by-step"]
[Назад](strategies-for-database-development-and-deployment-cs.md)
[Вперед](processing-unhandled-exceptions-cs.md)

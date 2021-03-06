---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
title: "С помощью кэш-зависимости SQL (C#) | Документы Microsoft"
author: rick-anderson
description: "Простейшая стратегия кэширования является предоставление кэшированные данные по истечении указанного периода времени. Однако этот простой подход означает, что maintai кэшированных данных..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 0e91842c-7f10-4aed-8c23-4ee3e2774014
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
msc.type: authoredcontent
ms.openlocfilehash: a6089b847dfd662e9b32128036170322823aac97
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-sql-cache-dependencies-c"></a>С помощью кэш-зависимости SQL (C#)
====================
по [Скотт Митчелл](https://twitter.com/ScottOnWriting)

[Загрузить код](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_CS.zip) или [скачать PDF](using-sql-cache-dependencies-cs/_static/datatutorial61cs1.pdf)

> Простейшая стратегия кэширования является предоставление кэшированные данные по истечении указанного периода времени. Однако этот простой подход означает, что кэшированных данных поддерживается не связан с его базового источника данных, в результате устаревшие данные, который проводится слишком длинное или текущих данных, истек срок действия слишком рано. Лучшим подходом является использование класса SqlCacheDependency, чтобы данные остаются кэшированных, пока его базовых данных был изменен в базе данных SQL. Этот учебник показывает, каким образом.


## <a name="introduction"></a>Вступление

Методы кэширования проверяются в [кэширование данных с помощью ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) и [кэширование данных в архитектуре](caching-data-in-the-architecture-cs.md) учебники позволяет исключить данные из кэша после указанного времени истечения срока действия период времени. Этот подход является самым простым способом сбалансировать выигрыш в производительности кэширования с устареванием данных. Выбрав срока истечения времени *x* секунд, разработчик concedes пользоваться преимуществами производительности кэширования для *x* секунд, но можно задержать просто что свои данные никогда не будет устаревших дольше, чем более из *x* секунд. Конечно, для статических данных *x* может распространяться на время существования веб-приложения, как было анализировать [кэширование данных при запуске приложения](caching-data-at-application-startup-cs.md) учебника.

При кэшировании данных базы данных, на основе времени окончания срока действия выбран часто для удобства его использования, но часто — это решение для недостаточно. В идеальном случае базы данных будет остаются в кэше до базовых данных был изменен в базе данных. только после этого будет вытеснение кэша. Такой подход обеспечивает максимальную производительность преимущества кэширования и сводит к минимуму время устаревшие данные. Однако чтобы существует эти преимущества предоставляются необходимо некоторые системы в месте, которое знает, когда основной базы данных был изменен и исключает — соответствующие элементы из кэша. До ASP.NET 2.0 разработчики страниц были отвечать за реализацию этой системе.

ASP.NET 2.0 предоставляет [ `SqlCacheDependency` класса](https://msdn.microsoft.com/en-us/library/system.web.caching.sqlcachedependency.aspx) и необходимую инфраструктуру, чтобы определить, когда произошло изменение в базе данных, чтобы соответствующие элементы в кэше может быть исключен. Существует два способа определить, когда базовые данные изменились: уведомления и опроса. После рассмотрения различий между уведомлением и опроса, мы создадим инфраструктуру необходимые для поддержки опроса и исследуйте способы использования `SqlCacheDependency` класса в декларативном и программно сценариев.

## <a name="understanding-notification-and-polling"></a>Основные сведения о уведомления и опроса

Существует два метода, которые могут использоваться для определения, когда был изменен в базе данных: уведомления и опроса. Уведомления базы данных автоматически оповещает среды выполнения ASP.NET, когда результаты определенного запроса были изменены с момента последнего выполнения запроса, после чего вытесняются кэшированные элементы, связанные с запросом. С использованием опроса, сервер базы данных сохраняет сведения о конкретной таблицы при последней были обновлены. Среда выполнения ASP.NET периодически опрашивает базу данных для проверки таблицы, которые были изменены с момента их поступления в кэш. Эти таблицы был изменен, данные которого имеют их кэша связанных элементов, удаленных.

Параметр уведомлений требуется настройка, меньше, чем опроса и более детализированные, так как он отслеживает изменения на уровне запроса, а не на уровне таблицы. К сожалению уведомления доступны только в полных выпусках Microsoft SQL Server 2005 (т. е. выпусков). Однако параметр опроса можно использовать для всех версий Microsoft SQL Server 7.0 до 2005. Поскольку в этих учебниках используется экспресс-выпуска SQL Server 2005, мы основное внимание уделяется настройке и использовании параметр опроса. В конце этого учебника дополнительные ресурсы по возможности уведомления SQL Server 2005 s см. в разделе дальнейшей чтения.

С использованием опроса, необходимо настроить базу данных для включения таблицы с именем `AspNet_SqlCacheTablesForChangeNotification` с тремя столбцами: `tableName`, `notificationCreated`, и `changeId`. Эта таблица содержит по одной строке для каждой таблицы, содержащий данные, которые могут понадобиться для использования в зависимости кэша SQL в веб-приложения. `tableName` Указывает имя таблицы во время `notificationCreated` указывает дату и время добавления строки в таблицу. `changeId` Столбец имеет тип `int` и начальное значение 0. Его значение увеличивается каждый после внесения изменений в таблицу.

В дополнение к `AspNet_SqlCacheTablesForChangeNotification` таблицы, базы данных также должен включать триггеры для каждой из таблиц, которые могут присутствовать в зависимости кэша SQL. Эти триггеры выполняются при вставке, обновлении или удалить строки и увеличить таблицу s `changeId` значение в `AspNet_SqlCacheTablesForChangeNotification`.

Среда выполнения ASP.NET отслеживает текущий `changeId` для таблицы при кэшировании данных с помощью `SqlCacheDependency` объекта. Периодически проверять базу данных, а также `SqlCacheDependency` объектов которого `changeId` отличается от значения в базе данных извлекаются с разными значениями `changeId` значение указывает, что произошло изменение в таблицу с момента кэширования данных.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>Шаг 1: Изучение`aspnet_regsql.exe`программы командной строки

Подход с использованием опроса базы данных должны быть программу установки, чтобы содержать инфраструктуры, описанные выше: предопределенной таблице (`AspNet_SqlCacheTablesForChangeNotification`), небольшое число хранимых процедур и триггеров в каждой из таблиц, которые могут использоваться в зависимости кэша SQL в Интернете приложение. Эти таблицы, хранимые процедуры и триггеры могут создаваться по программе командной строки `aspnet_regsql.exe`, которая находится в `$WINDOWS$\Microsoft.NET\Framework\version` папки. Для создания `AspNet_SqlCacheTablesForChangeNotification` таблицы и связанных хранимых процедур, запустите из командной строки следующую программу:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample1.cmd)]

> [!NOTE]
> Для выполнения этих команд login указанной базы данных должны находиться в [ `db_securityadmin` ](https://msdn.microsoft.com/en-us/library/ms188685.aspx) и [ `db_ddladmin` ](https://msdn.microsoft.com/en-us/library/ms190667.aspx) ролей. Для проверки T-SQL, отправляемые в базу данных по `aspnet_regsql.exe` командной строки программы, ознакомьтесь с [записи блога](http://scottonwriting.net/sowblog/posts/10709.aspx).


Например, чтобы добавить инфраструктуру для опроса базы данных Microsoft SQL Server с именем `pubs` на сервере базы данных с именем `ScottsServer` с использованием проверки подлинности Windows, перейдите в соответствующий каталог и, в командной строке введите следующую команду:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample2.cmd)]

После добавления инфраструктуры уровня базы данных, необходимо добавить триггеры для таблиц, которые будут использоваться в зависимости кэша SQL. Используйте `aspnet_regsql.exe` командной строки программы снова, но указано имя таблицы с помощью `-t` переключения и вместо `-ed` переключения используйте `-et`, следующим образом:


[!code-html[Main](using-sql-cache-dependencies-cs/samples/sample3.html)]

Чтобы добавить триггеры в `authors` и `titles` таблицы на `pubs` базы данных на `ScottsServer`, используйте:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample4.cmd)]

В этом учебнике добавить триггеры в `Products`, `Categories`, и `Suppliers` таблицы. Мы рассмотрим синтаксис командной строки, определенный на шаге 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>Шаг 2: Создание ссылок на базе Microsoft SQL Server 2005, экспресс-выпуск в`App_Data`

`aspnet_regsql.exe` Программа командной строки необходимо указать имя сервера и базы данных для добавления необходимых опроса инфраструктуры. Однако имя сервера и базы данных для базы данных Microsoft SQL Server 2005 Express, который находится в `App_Data` папку? Вместо того, чтобы обнаружить, что имена сервера и базы данных представляют собой ли хранить обнаружил, что простой подход для присоединения базы данных, чтобы `localhost\SQLExpress` базы данных экземпляра и переименовать данных с помощью [SQL Server Management Studio](https://msdn.microsoft.com/en-us/library/ms174173.aspx). Если один из полных версий SQL Server 2005, установлены на компьютере, затем скорее всего уже установлены на компьютере SQL Server Management Studio. Если экспресс-выпуск, можно загрузить бесплатную [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Сначала необходимо закрыть Visual Studio. Затем откройте SQL Server Management Studio и выберите для подключения к `localhost\SQLExpress` серверу с помощью проверки подлинности Windows.


![Присоединение к localhost\SQLExpress сервера](using-sql-cache-dependencies-cs/_static/image1.gif)

**Рис. 1**: присоединение к `localhost\SQLExpress` сервера


После подключения к серверу Management Studio Показать сервера и имеют вложенные папки для базы данных, безопасности и т. д. Щелкните правой кнопкой мыши папку базы данных и выберите параметр присоединения. Откроется диалоговое окно Присоединение баз данных (см. рис. 2). Нажмите кнопку "Добавить" и выберите `NORTHWND.MDF` папки базы данных в вашей s приложения web `App_Data` папки.


[![Присоедините NORTHWND. MDF базы данных из папки App_Data](using-sql-cache-dependencies-cs/_static/image2.gif)](using-sql-cache-dependencies-cs/_static/image1.png)

**На рисунке 2**: присоединение `NORTHWND.MDF` из базы данных `App_Data` папку ([Просмотр полноразмерное изображение](using-sql-cache-dependencies-cs/_static/image2.png))


Базы данных добавляются в папку базы данных. Имя базы данных может быть полный путь к файлу базы данных или полный путь с префиксом [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Чтобы избежать необходимости введите в это имя базы данных длительное время при использовании aspnet\_regsql.exe и средства командной строки, переименовать, присоединить базу данных с именем более понятную, просто щелкнув базы данных, выбрав переименование. Я хранить Моя база данных переименована в DataTutorials.


![Присвойте имя более понятную присоединенная база данных](using-sql-cache-dependencies-cs/_static/image3.gif)

**Рис. 3**: переименуйте более понятную имя присоединяемой базы данных


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Шаг 3: Добавление инфраструктуры опроса базы данных Northwind

Теперь, когда мы привязали `NORTHWND.MDF` из базы данных `App_Data` папки, мы готов для добавления в инфраструктуре опроса. Предположим, что можно сохранить базу данных переименован в DataTutorials, выполните следующие четыре команды:


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample5.cmd)]

После выполнения этих четырех команд, правой кнопкой мыши имя базы данных в среде Management Studio и перейдите в меню задач выберите отсоединения. Затем закройте среду Management Studio и снова откройте Visual Studio.

После снова открыть Visual Studio, можно перейти к базе данных с помощью обозревателя серверов. Обратите внимание, новая таблица (`AspNet_SqlCacheTablesForChangeNotification`), новые хранимые процедуры и триггеры на `Products`, `Categories`, и `Suppliers` таблицы.


![База данных теперь включает инфраструктуру необходимости опроса](using-sql-cache-dependencies-cs/_static/image4.gif)

**Рис. 4**: база данных теперь включает инфраструктуру необходимости опроса


## <a name="step-4-configuring-the-polling-service"></a>Шаг 4: Настройка службы опроса

После создания необходимых таблиц, триггеров и хранимых процедур в базе данных, последний шаг — Настройка опроса службы, которое выполняется с помощью `Web.config` путем указания базы данных для использования и частоты опроса в миллисекундах. Приведенная ниже разметка опрашивает базу данных "Борей" каждую секунду.


[!code-xml[Main](using-sql-cache-dependencies-cs/samples/sample6.xml)]

`name` Значение в `<add>` (NorthwindDB) элемент связывает понятное имя с определенной базы данных. При работе с зависимости кэша SQL, нам нужно будет ссылаться на имя базы данных, определенные здесь, а также в таблице на основе кэшированных данных. Мы рассмотрим способы использования `SqlCacheDependency` класса для программной связи зависимости кэша SQL с кэшированных данных в шаге 6.

После установления зависимости кэша SQL опроса компьютер подключается к базам данных, определенных в `<databases>` элементы каждого `pollTime` миллисекунд и выполнения `AspNet_SqlCachePollingStoredProcedure` хранимой процедуры. Эта хранимая процедура - которой был добавлен обратно в шаге 3 с помощью `aspnet_regsql.exe` Командная строка - возвращает `tableName` и `changeId` значения для каждой записи в `AspNet_SqlCacheTablesForChangeNotification`. Устаревший кэш-зависимости SQL вытесняются из кэша.

`pollTime` Параметр представляет компромисс между производительностью и устареванием данных. Небольшое `pollTime` значение увеличивает количество запросов к базе данных, но более быстро вытесняет устаревшие данные из кэша. Чтобы увеличить размер `pollTime` значение уменьшает количество запросов к базе данных, но увеличивает задержку между при изменении данных серверной части, и когда извлекаются связанные элементы. К счастью запрос базы данных выполняется простая хранимая процедура возврат только несколько строк из таблицы упрощенный s. Но поэкспериментировать с различными `pollTime` значения, чтобы найти идеальный баланс между базы данных устаревание доступа и данные для приложения. Наименьшего `pollTime` допустимое значение равно 500.

> [!NOTE]
> Приведенный выше пример предоставляет одну `pollTime` значение в `<sqlCacheDependency>` элемент, но при необходимости можно указать `pollTime` значение в `<add>` элемент. Это полезно в том случае, если указано несколько баз данных и настроить частоту опроса на базу данных.


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Шаг 5: Работа декларативно с зависимости кэша SQL

В шаги с 1 по 4 мы рассматривали настройке инфраструктуры необходимые базы данных и настроить систему опроса. С этой инфраструктуры мы теперь можно добавить элементы в кэше данных связанные зависимости кэша SQL, либо программные или декларативные методами. На этом шаге мы изучим, как декларативно работать с зависимости кэша SQL. На шаге 6 мы рассмотрим программный подход.

[Кэширование данных с помощью ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) декларативных возможностей кэширования ObjectDataSource изучена учебника. Просто задав `EnableCaching` свойства `true` и `CacheDuration` некоторые интервала времени для свойства, элемент управления ObjectDataSource автоматически будет кэшировать данные, возвращенные из базового объекта в течение указанного интервала. Элемент управления ObjectDataSource можно также использовать одну или несколько зависимостей кэша SQL.

Чтобы продемонстрировать декларативно с помощью зависимости кэша SQL, откройте `SqlCacheDependencies.aspx` страницы в `Caching` папки и перетащите элемент управления GridView с панели элементов в конструктор. Набор GridView s `ID` для `ProductsDeclarative` и в его смарт-тег, выберите, чтобы привязать его к новой ObjectDataSource с именем `ProductsDataSourceDeclarative`.


[![Создать новый элемент управления ObjectDataSource ProductsDataSourceDeclarative с именем](using-sql-cache-dependencies-cs/_static/image5.gif)](using-sql-cache-dependencies-cs/_static/image3.png)

**Рис. 5**: создать новый именованный ObjectDataSource `ProductsDataSourceDeclarative` ([Просмотр полноразмерное изображение](using-sql-cache-dependencies-cs/_static/image4.png))


Настройка ObjectDataSource для использования `ProductsBLL` класс и установить на вкладке ВЫБЕРИТЕ в раскрывающемся списке `GetProducts()`. На вкладке «обновление» выберите `UpdateProduct` перегрузка с три входных параметра - `productName`, `unitPrice`, и `productID`. Задайте раскрывающиеся списки (нет) на вкладках INSERT и DELETE.


[![Используйте перегрузку UpdateProduct с тремя входными параметрами.](using-sql-cache-dependencies-cs/_static/image6.gif)](using-sql-cache-dependencies-cs/_static/image5.png)

**Рис. 6**: используйте перегрузки UpdateProduct с тремя параметрами ввода ([Просмотр полноразмерное изображение](using-sql-cache-dependencies-cs/_static/image6.png))


[![Выберите в списке раскрывающегося списка (нет) для вставки и удаления вкладок](using-sql-cache-dependencies-cs/_static/image7.gif)](using-sql-cache-dependencies-cs/_static/image7.png)

**Рис. 7**: значение раскрывающегося списка (нет) для вставки и удаления вкладок ([Просмотр полноразмерное изображение](using-sql-cache-dependencies-cs/_static/image8.png))


После завершения работы мастера настройки источника данных Visual Studio создаст стояли и CheckBoxFields в GridView для каждого поля данных. Удаление всех полей, но `ProductName`, `CategoryName`, и `UnitPrice`и отформатировать эти поля, по своему усмотрению. В смарт-теге s GridView установите флажки включить разбиение на страницы, включить сортировку и разрешить изменение. Visual Studio установит ObjectDataSource s `OldValuesParameterFormatString` свойства `original_{0}`. Чтобы для правильной работы средства редактирования s GridView, удалите это свойство полностью декларативного синтаксиса или верните значение по умолчанию `{0}`.

Наконец, добавьте веб-управления Label выше GridView и задайте его `ID` свойства `ODSEvents` и его `EnableViewState` свойства `false`. После внесения этих изменений, на странице s должна выглядеть следующим образом. Обратите внимание что хранить внесли настройки внешнего вида к GridView поля, которые не нужны для демонстрации возможностей зависимость кэша SQL.


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample7.aspx)]

Создайте обработчик событий для ObjectDataSource s `Selecting` событий в его и добавьте следующий код:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample8.cs)]

Помните, что ObjectDataSource s `Selecting` событие запускается только в том случае, при получении данных из базового объекта. Если элемент управления ObjectDataSource обращается к данным из собственный кэш, это событие не вызывается.

Теперь посетите эту страницу через браузер. Поскольку мы хранять еще для реализации, кэширование, каждый раз, страницы, сортировку или отредактировать сетку страницы отображать текст, события Selecting запускается, как показано на рис. 8.


[![S ObjectDataSource события Selecting срабатывает каждый выгружаемого GridView, редактировать, время или Sorted](using-sql-cache-dependencies-cs/_static/image8.gif)](using-sql-cache-dependencies-cs/_static/image9.png)

**Рис. 8**: s ObjectDataSource `Selecting` событие активируется каждый времени выгружаемого GridView, редактирования или Sorted ([Просмотр полноразмерное изображение](using-sql-cache-dependencies-cs/_static/image10.png))


Как мы видели в [кэширование данных с помощью ObjectDataSource](caching-data-with-the-objectdatasource-cs.md) учебник, установка `EnableCaching` свойства `true` вызывает ObjectDataSource для кэширования данных в течение заданного по его `CacheDuration` свойство. ObjectDataSource также имеет [ `SqlCacheDependency` свойство](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), который добавляет одну или несколько зависимостей кэша SQL к кэшированным данным, используя шаблон:


[!code-css[Main](using-sql-cache-dependencies-cs/samples/sample9.css)]

Где *databaseName* — имя базы данных, как указано в `name` атрибут `<add>` элемент в `Web.config`, и *tableName* имя таблицы базы данных. Например, для создания ObjectDataSource, который кэширует данные в течение неограниченного времени на основе зависимость кэша SQL для Northwind s `Products` таблица, набор ObjectDataSource s `EnableCaching` свойства `true` и его `SqlCacheDependency` свойства NorthwindDB:Products.

> [!NOTE]
> Можно использовать зависимость кэша SQL *и* на основе времени окончания срока действия, задав `EnableCaching` для `true`, `CacheDuration` для интервала времени и `SqlCacheDependency` на имена базы данных и таблицы. ObjectDataSource исключим его данные при достижении на основе времени окончания срока действия или опроса система сообщает, что основной базы данных изменилось, какое событие произойдет раньше.


GridView с `SqlCacheDependencies.aspx` отображает данные из двух таблиц - `Products` и `Categories` (продукта s `CategoryName` поля извлекается из `JOIN` на `Categories`). Таким образом, необходимо указать две зависимости кэша SQL: NorthwindDB:Products; NorthwindDB:Categories.


[![Настройка ObjectDataSource для поддержки кэширования с помощью зависимости кэша SQL на продукты и категории](using-sql-cache-dependencies-cs/_static/image9.gif)](using-sql-cache-dependencies-cs/_static/image11.png)

**Рис. 9**: Настройка ObjectDataSource для поддержки кэширования с помощью кэш-зависимости SQL на `Products` и `Categories` ([Просмотр полноразмерное изображение](using-sql-cache-dependencies-cs/_static/image12.png))


После настройки ObjectDataSource для поддержки кэширования, обращаются к странице через браузер. Опять же возникает событие выделение текста появится при первом посещении страницы, но должна исчезнуть при разбиение по страницам, сортировки или кнопки редактирования или "Отмена". Это, поскольку после загрузки данных в кэш s ObjectDataSource остается там до `Products` или `Categories` таблицы изменять или обновлять данные с помощью GridView.

AFTER, срабатывающим разбиения по страницам сетки и отметить отсутствие события Selecting текст, откройте новое окно браузера и перейдите в учебнике основы редактирования, вставки и удаления раздела (`~/EditInsertDelete/Basics.aspx`). Обновите имя или цену продукта. Затем из первого окно браузера, просмотра на другую страницу данных, сортировки сетки или нажмите кнопку Правка строк. Это время события Selecting создается, следует вновь как основной базы данных, данные были изменены (см. рис. 10). Если текст не отображается, подождите немного и повторите попытку. Помните, что опроса службы проверяет наличие изменений в `Products` таблицы каждый `pollTime` миллисекунд, так что задержка между при обновлении базовых данных и если вытеснение кэшированных данных.


[![Изменение таблицы Products исключает кэшированные данные](using-sql-cache-dependencies-cs/_static/image10.gif)](using-sql-cache-dependencies-cs/_static/image13.png)

**Рис. 10**: изменение таблицы Products исключает кэшированных данных продукта ([Просмотр полноразмерное изображение](using-sql-cache-dependencies-cs/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Шаг 6: Программными средствами работы с`SqlCacheDependency`класса

[Кэширование данных в архитектуре](caching-data-in-the-architecture-cs.md) учебника рассказывали преимуществ использования архитектуры, в отличие от тесной взаимозависимости между обработчиком кэширование с ObjectDataSource отдельном слое кэширования. В этом учебнике мы создали `ProductsCL` для демонстрации программными средствами работы с кэшем данных. Чтобы использовать зависимости кэша SQL в слое кэширование, `SqlCacheDependency` класса.

В системе опроса `SqlCacheDependency` объект должен быть связан с парой определенной базы данных и таблицы. Например, следующий код создает `SqlCacheDependency` объекта, основанного на базе данных Northwind s `Products` таблицы:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample10.cs)]

Два входных параметров для `SqlCacheDependency` конструктор s являются именами базы данных и таблицы, соответственно. Как и с ObjectDataSource s `SqlCacheDependency` свойство, используется имя базы данных является таким же, как указано в значении `name` атрибут `<add>` элемент в `Web.config`. Имя таблицы — это реальное имя таблицы базы данных.

Чтобы связать `SqlCacheDependency` с элемента, добавленных в кэш данных, используйте один из `Insert` перегрузки метода, которые принимает зависимость. Следующий код добавляет *значение* для кэширования данных неопределенное время, но связывает его с `SqlCacheDependency` на `Products` таблицы. Иными словами *значение* будет оставаться в кэше, пока он удаляется из-за нехватки памяти или из-за опроса система обнаружила, `Products` таблица была изменена, так как он был кэширован.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample11.cs)]

Кэширование слоя s `ProductsCL` класс в настоящее время кэширует данные из `Products` таблицу на основе времени окончания срока действия 60 секунд. Разрешить обновление этого класса, чтобы он использовал зависимости кэша SQL s. `ProductsCL` Класса s `AddCacheItem` метод, который отвечает за добавление данных в кэш, в настоящее время содержит следующий код:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample12.cs)]

Обновление кода, используемого `SqlCacheDependency` объекта вместо `MasterCacheKeyArray` зависимости кэша:


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample13.cs)]

Протестировать эти функции, добавьте элемент управления GridView на страницу под существующий `ProductsDeclarative` GridView. Задайте этот новый s GridView `ID` для `ProductsProgrammatic` и через его смарт-тег, привязать его к новой ObjectDataSource с именем `ProductsDataSourceProgrammatic`. Настройка ObjectDataSource для использования `ProductsCL` класса, задание раскрывающихся списков в инструкции SELECT и UPDATE вкладок, чтобы `GetProducts` и `UpdateProduct`соответственно.


[![Настройка ObjectDataSource с помощью класса ProductsCL](using-sql-cache-dependencies-cs/_static/image11.gif)](using-sql-cache-dependencies-cs/_static/image15.png)

**Рис. 11**: Настройка ObjectDataSource для использования `ProductsCL` класса ([Просмотр полноразмерное изображение](using-sql-cache-dependencies-cs/_static/image16.png))


[![Выберите метод GetProducts из раскрывающегося списка ВЫБЕРИТЕ вкладку s](using-sql-cache-dependencies-cs/_static/image12.gif)](using-sql-cache-dependencies-cs/_static/image17.png)

**Рис. 12**: выберите `GetProducts` метод из раскрывающегося списка ВЫБЕРИТЕ вкладку s ([Просмотр полноразмерное изображение](using-sql-cache-dependencies-cs/_static/image18.png))


[![Выберите метод UpdateProduct из обновления вкладку s раскрывающегося списка](using-sql-cache-dependencies-cs/_static/image13.gif)](using-sql-cache-dependencies-cs/_static/image19.png)

**Рис. 13**: выберите метод UpdateProduct из раскрывающегося списка вкладку обновление s ([Просмотр полноразмерное изображение](using-sql-cache-dependencies-cs/_static/image20.png))


После завершения работы мастера настройки источника данных Visual Studio создаст стояли и CheckBoxFields в GridView для каждого поля данных. Как и первый GridView, добавляемые на эту страницу, удалите все поля, но `ProductName`, `CategoryName`, и `UnitPrice`и отформатировать эти поля, по своему усмотрению. В смарт-теге s GridView установите флажки включить разбиение на страницы, включить сортировку и разрешить изменение. Как и в `ProductsDataSourceDeclarative` ObjectDataSource, Visual Studio установит `ProductsDataSourceProgrammatic` ObjectDataSource s `OldValuesParameterFormatString` свойства `original_{0}`. В порядке, для правильной работы, установите это свойство компонента GridView s изменения обратно в `{0}` (или полностью удалить назначение свойства из декларативного синтаксиса).

После выполнения этих задач, полученный GridView и ObjectDataSource декларативная разметка должна выглядеть следующим образом:


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample14.aspx)]

Для проверки SQL зависимости в кэше в слое кэширования установите точку останова в `ProductCL` класса s `AddCacheItem` метода и затем запустить отладку. При первом посещении `SqlCacheDependencies.aspx`, точка останова должна быть достигнута как запрошен в первый раз и помещенных в кэш данных. После этого перехода на другую страницу в GridView или один из столбцов сортировки. В результате GridView для повторного запроса данных, но данные должны находиться в кэше с момента `Products` таблицы базы данных не была изменена. Если данные повторно не найден в кэше, убедитесь, что имеется достаточно памяти на компьютере и повторите попытку.

После разбиение по страницам нескольких страниц GridView, откройте второе окно браузера и перейдите в учебнике основы редактирования, вставки и удаления раздела (`~/EditInsertDelete/Basics.aspx`). Обновление записи из таблицы Products и просмотрите новую страницу из первого окна браузера или щелкните один из заголовков сортировки.

В этом случае вы увидите одно из следующих действий: либо будут иметь точки останова, указывающее, что кэшированные данные был исключен из-за изменения в базе данных. или не быть останова, это значит, что `SqlCacheDependencies.aspx` отображается устаревшие данные. Если не будет достигнута точка останова, вполне вероятно, так как служба опроса не еще сработал с момента изменения данных. Помните, что опроса службы проверяет наличие изменений в `Products` таблицы каждый `pollTime` миллисекунд, так что задержка между при обновлении базовых данных и если вытеснение кэшированных данных.

> [!NOTE]
> Эта задержка чаще всего для отображения при изменении одного из продуктов через GridView в `SqlCacheDependencies.aspx`. В [кэширование данных в архитектуре](caching-data-in-the-architecture-cs.md) учебника мы добавили `MasterCacheKeyArray` зависимости, чтобы убедиться, что данные редактируемого через кэша `ProductsCL` класса s `UpdateProduct` метод был исключен из кэша. Тем не менее, мы заменили зависимости в кэше, при изменении `AddCacheItem` метод ранее на этом шаге и, следовательно, `ProductsCL` класса будут отображать кэшированные данные, пока система опроса сообщает изменение `Products` таблицы. Мы рассмотрим способы повторное добавление `MasterCacheKeyArray` кэшировать зависимостей в шаге 7.


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Шаг 7: Связывание несколько зависимостей с кэшированного элемента

Помните, что `MasterCacheKeyArray` зависимости в кэше используется, чтобы убедиться, что *все* продуктах данных извлекается из кэша, когда обновляется связанный внутри его ни от одного объекта. Например `GetProductsByCategoryID(categoryID)` кэши метод `ProductsDataTables` экземпляров для каждого уникальный *categoryID* значение. Если вытеснение одного из этих объектов `MasterCacheKeyArray` зависимости в кэше гарантирует, что другие также удаляются. Без этой зависимости кэша при изменении кэшированных данных существует возможность, других продукта кэшированные данные могут быть устаревшими. Следовательно, он s, важно, что мы поддерживать `MasterCacheKeyArray` зависимости кэша при использовании зависимости кэша SQL. Тем не менее, данные кэша s `Insert` метод допускает только одну зависимость объекта.

Кроме того при работе с зависимости кэша SQL мы может понадобиться связать нескольких таблиц баз данных в качестве зависимостей. Например `ProductsDataTable` кэшированных в `ProductsCL` класс содержит категорию и поставщика имена для каждого продукта, но `AddCacheItem` метод использует только зависимости на `Products`. В этом случае если пользователь обновляет имя категории или поставщика, кэшированные данные будут остаются в кэше и имеет более старую версию. Таким образом, мы хотим сделать зависимым от данных кэшированные продукта не только `Products` таблицы, но на `Categories` и `Suppliers` таблицы.

[ `AggregateCacheDependency` Класс](https://msdn.microsoft.com/en-us/library/system.web.caching.aggregatecachedependency.aspx) предоставляет средства для связывания несколько зависимостей с элементом кэша. Начните с создания `AggregateCacheDependency` экземпляра. Добавьте набор зависимостей с помощью `AggregateCacheDependency` s `Add` метод. При вставке элемента в кэше данных соответственно, передайте `AggregateCacheDependency` экземпляра. Когда *любой* из `AggregateCacheDependency` изменить экземпляр s зависимости, удаления кэшированного элемента.

Ниже показан обновленный код для `ProductsCL` класса s `AddCacheItem` метод. Этот метод создает `MasterCacheKeyArray` кэшировать зависимостей вместе с `SqlCacheDependency` объектов для `Products`, `Categories`, и `Suppliers` таблицы. Они были объединены в одну `AggregateCacheDependency` объект с именем `aggregateDependencies`, который затем передается в `Insert` метод.


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample15.cs)]

Проверьте новый код out. Он изменится `Products`, `Categories`, или `Suppliers` таблиц привести к вытеснению кэшированных данных. Кроме того `ProductsCL` класса s `UpdateProduct` метод, который вызывается при изменении продукта через GridView, исключает `MasterCacheKeyArray` зависимости, что приведет к тому кэшированные кэша `ProductsDataTable` вытеснения и данные, извлекаемые повторно при следующем запрос.

> [!NOTE]
> Кэш-зависимости SQL также может использоваться с [кэширование вывода](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Демонстрацию этой функции см. в разделе: [с помощью кэширования выходных данных ASP.NET с помощью SQL Server](https://msdn.microsoft.com/en-us/library/e3w8402y(VS.80).aspx).


## <a name="summary"></a>Сводка

При кэшировании данных базы данных, данные в идеале будет оставаться в кэше до его изменения в базе данных. В ASP.NET 2.0 можно создать и использовать в сценариях декларативные и программные зависимости кэша SQL. Одна из сложностей при таком подходе возможности обнаружения при изменении данных. Полные версии Microsoft SQL Server 2005 предоставляют возможности уведомления, которые можно предупредить приложения при изменении результатов запроса. Express Edition SQL Server 2005 и более ранних версий SQL Server необходимо использовать систему опроса. К счастью настройке инфраструктуры необходимости опроса выполняется очень просто.

Программирование довольны!

## <a name="further-reading"></a>Дополнительные сведения

Дополнительные сведения по темам, рассматриваемые в этом учебнике см. в следующих ресурсах:

- [Использование уведомлений о запросах в Microsoft SQL Server 2005](https://msdn.microsoft.com/en-us/library/ms175110.aspx)
- [Создание уведомления о запросе](https://msdn.microsoft.com/en-us/library/ms188669.aspx)
- [Кэширование в ASP.NET с `SqlCacheDependency` класса](https://msdn.microsoft.com/en-us/library/ms178604(VS.80).aspx)
- [Средство регистрации ASP.NET SQL Server (`aspnet_regsql.exe`)](https://msdn.microsoft.com/en-us/library/ms229862(vs.80).aspx)
- [Общие сведения о`SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Об авторе

[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года. Скотт — независимый консультант, trainer и записи. Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Благодарности

Этот учебник ряд прошел проверку многие полезные рецензентов. Основными редакторами этого учебника были Marko Rangel Тереза д Мерфи и – Хилтон Гизнау. Объясняются моих последующих статей для MSDN? Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Назад](caching-data-at-application-startup-cs.md)
[Вперед](caching-data-with-the-objectdatasource-vb.md)

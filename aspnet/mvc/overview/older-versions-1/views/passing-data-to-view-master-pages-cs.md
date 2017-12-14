---
uid: mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
title: "Передача данных в представления главных страниц (C#) | Документы Microsoft"
author: microsoft
description: "Целью данного учебника является объясняется способ передачи данных из контроллера для главной страницы представления. Рассматриваются две стратегии передачи данных в представление m..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: 5fee879b-8bde-42a9-a434-60ba6b1cf747
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/passing-data-to-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: b8bc8ce0690d2e45877be75011d8883facbc74a7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="passing-data-to-view-master-pages-c"></a>Передача данных представления главных страниц (C#)
====================
по [Microsoft](https://github.com/microsoft)

[Скачать PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_13_CS.pdf)

> Целью данного учебника является объясняется способ передачи данных из контроллера для главной страницы представления. Рассматриваются две стратегии передачи данных для главной страницы представления. Во-первых мы обсудим простое решение, полученный в приложении, которое трудно обслуживать. Далее мы рассмотрим более удачное решение, требуется немного больше начальной работы, но приводит к гораздо более простым в обслуживании приложения.


## <a name="passing-data-to-view-master-pages"></a>Передача данных представления главных страниц

Целью данного учебника является объясняется способ передачи данных из контроллера для главной страницы представления. Рассматриваются две стратегии передачи данных для главной страницы представления. Во-первых мы обсудим простое решение, полученный в приложении, которое трудно обслуживать. Далее мы рассмотрим более удачное решение, требуется немного больше начальной работы, но приводит к гораздо более простым в обслуживании приложения.

### <a name="the-problem"></a>Проблема

Предположим, что вы создаете приложение фильма базы данных и будет отображаться список категорий фильма каждой страницы в приложении (см. рис. 1). Кроме того, себе представьте, что список категорий фильма хранится в таблице базы данных. В этом случае он смысла для извлечения категорий из базы данных и отображения списка категорий фильма в главной страницы представления.


[![Отображение категории фильма в главной страницы представления](passing-data-to-view-master-pages-cs/_static/image2.png)](passing-data-to-view-master-pages-cs/_static/image1.png)

**На рисунке 01**: отображение категорий фильма в главной страницы представления ([Просмотр полноразмерное изображение](passing-data-to-view-master-pages-cs/_static/image3.png))


Вот проблему. Как получить список категорий фильма на главной странице Это заманчивой для прямого вызова методов классов модели на главной странице. Другими словами заманчивой в него код для получения данных из базы данных право на главной странице. Тем не менее обход ваших контроллеров MVC, для доступа к базе данных, нарушающих четкого разделения проблем, является одним из основных преимуществ создания приложения MVC.

В MVC-приложении необходимо все взаимодействие между представлений MVC и модели MVC должны обрабатываться ваших контроллеров MVC. Такое разделение областей ответственности приводит к более простым в обслуживании, настраиваться и тестируемого приложения.

В приложении MVC все данные, передаваемые в представление, в том числе главной страницы представления — должны передаваться в представление с помощью действия контроллера. Кроме того данные должны передаваться за счет использования представления данных. В оставшейся части этого учебника я просматривать двух методов передачи данных представления главной страницы представления.

### <a name="the-simple-solution"></a>Простое решение

Давайте начнем с простым решением для передачи данных в представлении с контроллера главной страницы представления. Самым простым решением является передача данные представления для главной страницы в каждой действия контроллера.

Рассмотрите возможность контроллера в листинге 1. Он предоставляет два действия с именем `Index()` и `Details()`. `Index()` Метод действия возвращает каждого фильма в таблице базы данных фильмов. `Details()` Метод действия возвращает каждого фильма в категории определенного фильма.

**Листинг 1.`Controllers\HomeController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample1.cs)]

Обратите внимание, что Index() и действия Details() добавить два элемента для просмотра данных. Действие Index() добавляет два ключа: категории и фильмов. Ключ категории представляет список категорий фильма, отображаемого элементом главной страницы представления. Ключ фильмы представляет список фильмов, отображаемых на странице представление индекса.

Действие Details() также добавляет два ключа с именем категории и фильмов. Ключ категории, опять же, представляет список категорий фильма, отображаемого элементом главной страницы представления. Ключ фильмы представляет список фильмов в определенной категории, отображаемых на странице представления сведений (см. рис. 2).


[![Просмотр подробностей](passing-data-to-view-master-pages-cs/_static/image5.png)](passing-data-to-view-master-pages-cs/_static/image4.png)

**На рисунке 02**: Просмотр подробностей ([Просмотр полноразмерное изображение](passing-data-to-view-master-pages-cs/_static/image6.png))


Представление индекса содержится в списке 2. Он просто проходит по список фильмов, представленный фильмы элемента в представлении данных.

**Листинг 2.`Views\Home\Index.aspx`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample2.aspx)]

В списке 3 содержится главной страницы представления. Главной страницы представления выполняет итерацию и отображает все категории фильм, представленный категории элемента из представления данных.

**Листинг 3.`Views\Shared\Site.master`**

[!code-aspx[Main](passing-data-to-view-master-pages-cs/samples/sample3.aspx)]

Все данные передаются данные представления для представления и главной страницы представления. Это правильный способ передачи данных на главную страницу.

Таким образом что происходит в это решение? Проблема заключается в том, что это решение нарушает принцип ПРОБНОГО (не повторяться). Все действия контроллера необходимо добавить тот же список категорий фильма для просмотра данных. Наличие повторяющихся кода в приложении приложения значительно усложняет для обслуживания, адаптировать и изменить.

### <a name="the-good-solution"></a>Хорошее решение

В этом разделе рассматриваются альтернативные и лучше, решение для передачи данных из действия контроллера в главной страницы представления. Вместо добавления категории фильма для главной страницы в каждой действия контроллера, мы добавим категории фильма Просмотр данных только один раз. Все данные представления, используемые представления главной страницы добавляется в контроллером приложения.

Класс ApplicationController содержится в листинге 4.

**Листинг 4.`Controllers\ApplicationController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample4.cs)]

Существует три вещи, которые можно заметить о контроллере приложения в листинге 4. Во-первых Обратите внимание, что класс наследует от базового класса System.Web.Mvc.Controller. Контроллер приложения является класс контроллера.

Во-вторых Обратите внимание, что класс контроллера приложения является абстрактным классом. Абстрактный класс является класс, который должен быть реализован для конкретного класса. Поскольку контроллера приложения — это абстрактный класс, не невозможно вызвать любые методы, определенные в классе напрямую. При попытке непосредственно вызывать класс приложений вы получите сообщение об ошибке не удается найти ресурс.

В-третьих Обратите внимание, что контроллера приложения содержит конструктор, который добавляет список категорий фильма для просмотра данных. Каждый класс контроллера, наследующий от контроллера приложение вызывает конструктор контроллера приложения автоматически. При каждом вызове на любом контроллере, который наследует от контроллера приложения какие-либо действия, категории фильма автоматически включается в представление данных.

Контроллер фильмов в листинге 5 наследует от контроллера приложения.

**Листинг 5.`Controllers\MoviesController.cs`**

[!code-csharp[Main](passing-data-to-view-master-pages-cs/samples/sample5.cs)]

Контроллер фильмы, так же, как описано в предыдущем разделе контроллера Home предоставляет два метода действия с именем `Index()` и `Details()`. Обратите внимание, что список фильма категорий, отображаемых на странице главного представления не добавлен для просмотра данных в любом `Index()` или `Details()` метод. Поскольку контроллер фильмы наследуется от контроллера приложения, список категорий фильма добавляется автоматически просматривать данные.

Обратите внимание, что это решение для добавления данных представления для главной страницы представления не нарушает принцип ПРОБНОГО (не повторяться). Код для добавления в список категорий фильма для просмотра данных содержится только в одном месте: конструктор для контроллера приложения.

### <a name="summary"></a>Сводка

В этом учебнике мы рассмотрели два способа передачи данных в представлении с контроллера главной страницы представления. Во-первых мы рассмотрели простой, но трудно поддерживать подход. В первом разделе мы рассмотрели, как добавить данные представления для главной страницы представления в каждый каждое действие контроллера в приложении. Мы подтвердила, что это был неверный подход, поскольку оно нарушает принцип ПРОБНОГО (не повторяться).

Далее мы рассмотрели гораздо лучшей стратегией для добавления данных, необходимых для просмотра данных главной страницы представления. Вместо добавления представления данных в каждой действия контроллера, мы добавили данные представления только один раз в течение контроллером приложения. Таким образом можно избежать повторяющийся код, при передаче данных в представления главной страницы в приложении ASP.NET MVC.

>[!div class="step-by-step"]
[Назад](creating-page-layouts-with-view-master-pages-cs.md)
[Вперед](asp-net-mvc-views-overview-vb.md)
---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: "Поддержка действия OData в ASP.NET Web API 2 | Документы Microsoft"
author: MikeWasson
description: "В OData действий, способ добавления серверных поведения, которые легко не определены как операций CRUD в объектах. Некоторые варианты применения действия включают: реализуйте..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Поддержка действия OData в ASP.NET Web API 2
====================
по [Mike Wasson](https://github.com/MikeWasson)

[Загрузка завершенного проекта](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> В OData *действия* позволяют добавить серверные поведения, которые легко не определены как операций CRUD в объектах. Некоторые варианты применения действия включают:
> 
> - Реализация сложных транзакциях.
> - Управление сразу несколько сущностей.
> - Разрешение обновлений только на определенные свойства сущности.
> - Отправка сведений на сервер, который не определен в сущности.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Веб-API 2
> - OData версии 3
> - Entity Framework 6


## <a name="example-rating-a-product"></a>Пример: Оценка продукта

В этом примере мы хотим позволяют оценить продукты, а затем предоставить средние оценки для каждого продукта. В базе данных сохраняется список оценок, соответствующие продукты.

Вот модели, который может использоваться для представления рейтинга в Entity Framework.

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Но мы не хотим клиентам POST `ProductRating` объекта из коллекции «Оценки». Интуитивно оценка связан с коллекцией продуктов и клиента необходимо только учет значение оценки.

Таким образом вместо обычных операций CRUD, действие, которое может вызывать клиент определяется по продукту. В терминологии OData, такое действие было *привязан* для сущности продукта.

>Действия имеют побочные эффекты на сервере. По этой причине они вызываются с помощью HTTP-запросы POST. Действия могут иметь параметры и возвращаемые типы, которые описаны в метаданных службы. Клиент отправляет параметров в тексте запроса, а сервер отправляет возвращаемого значения в тексте ответа. Для вызова действия «Частота продукт», клиент отправляет POST URI следующим образом:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

Данные в запросе POST — просто оценку продукта:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Объявить действие в модели EDM

Добавьте действие в настройку веб-API модели EDM (модель EDM):

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Этот код определяет «RateProduct» как действие, которое может выполняться на сущностях продукта. Он объявляет, что действие принимает **int** параметр с именем «Оценка» и возвращает **int** значение.

## <a name="add-the-action-to-the-controller"></a>Добавить действие к контроллеру

Действие «RateProduct» привязан к сущности продукта. Для выполнения действий, добавьте метод с именем `RateProduct` для контроллера Products:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Обратите внимание, что имя метода совпадает с именем действия в модели EDM. Метод имеет два параметра:

- *ключ*: ключ продукта для частоты.
- *Параметры*: словарь значений параметра действия.

При использовании соглашений о маршрутизации по умолчанию ключевой параметр должен иметь имя «key». Также важно для включения **[FromOdataUri]** атрибута, как показано. Этот атрибут сообщает веб-API для использования правилами синтаксиса OData при синтаксическом анализе ключ от URI запроса.

Используйте *параметры* словарь для получения параметров действия:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Если клиент отправляет параметров действия в правильный формат, значение **ModelState.IsValid** имеет значение true. В этом случае можно использовать **ODataActionParameters** словарь для получения значений параметров. В этом примере `RateProduct` действие принимает один параметр с именем «Оценка».

## <a name="action-metadata"></a>Метаданные действия

Чтобы просмотреть метаданные службы, отправка запроса GET к метаданным /odata/$. Вот часть метаданных, который объявляет `RateProduct` действия:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

**FunctionImport** элемент объявляет действие. Большинство полей говорят сами за себя, но два — Обратите внимание:

- **Имеет значение IsBindable** означает действие может вызываться в целевой сущности по крайней мере некоторое время.
- **IsAlwaysBindable** означает всегда можно вызвать действие в целевой сущности.

Разница заключается в некоторых действий, всегда будут доступны клиентам, что другие действия могут опираться на состояние сущности. Например предположим, что определяется действие «Покупки». Только вы можете приобрести элемента, который находится на складе. Если элемент отсутствует на складе, клиент не может вызвать это действие.

При определении модели EDM, **действия** метод создает всегда привязываемых действие:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Мы поговорим о не всегда привязки действия (также называется *временные* действия) Далее в этом разделе.

## <a name="invoking-the-action"></a>Вызвать действие

Теперь давайте посмотрим, как клиент может вызвать это действие. Предположим, что клиенту необходимо предоставить оценку 2 для продукта с Идентификатором = 4. Ниже приведен пример сообщения запроса, с использованием формата JSON для текста запроса.

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Вот ответное сообщение.

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Привязка действие в наборе сущностей

В предыдущем примере действие привязано к одной сущности: клиент оценивает за единицу продукта. Можно также привязать действие к коллекцию сущностей. Сделать следующие изменения:

Добавьте действие сущности в модели EDM, **коллекции** свойство.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

В методе контроллера опустить *ключ* параметра.

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Теперь клиент вызывает действие в наборе сущностей продуктов:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Действия с параметрами коллекции

Действия могут иметь параметры, которые принимают коллекцию значений. В модели EDM, используйте **CollectionParameter&lt;T&gt;**  для объявления параметра.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Этот код объявляет параметр с именем «Оценки», который принимает коллекцию **int** значения. В этом методе контроллера, по-прежнему получить значение параметра из **ODataActionParameters** объекта, но теперь значение **ICollection&lt;int&gt;**  значение:

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Временные действия

В примере «RateProduct» пользователи всегда можно оценить продукт, действие всегда доступно. Однако некоторые действия зависят от состояния сущности. Например в службе видео аренды, действие «Оформление заказа» не всегда доступен. (Оно зависит, доступен ли копия этого видео). Такие действия называются *временные* действие.

В метаданных службы, имеет временный действие **IsAlwaysBindable** равно false. Это фактически значение по умолчанию, так что метаданных будет выглядеть следующим образом:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Вот почему это важно: Если действие является временным, этот сервер должен сообщить клиенту, когда действие доступно. Это делается посредством добавления ссылки к действию в сущности. Ниже приведен пример для сущности фильма:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

Свойство «#CheckOut» содержит ссылку на действие извлечения. Если действие недоступно, сервер пропускает по ссылке.

Чтобы объявить временных действий в модели EDM, вызовите **TransientAction** метод:

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Кроме того необходимо предоставить функцию, которая возвращает ссылку действия для данной сущности. Задать эту функцию, вызвав **HasActionLink**. Можно написать функцию как лямбда-выражения:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Если действие доступно, лямбда-выражение возвращает ссылку действия. Сериализатор OData включает эта ссылка при сериализации сущности. Если действие недоступно, функция возвращает значение `null`.

## <a name="additional-resources"></a>Дополнительные ресурсы

[Пример действий OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
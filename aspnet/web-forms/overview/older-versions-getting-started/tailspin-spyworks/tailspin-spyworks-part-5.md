---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
title: "Часть 5: Бизнес-логику | Документы Microsoft"
author: JoeStagner
description: "Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks. Часть 5 добавляет некоторые бизнес-логики."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: eaef475a-ca91-47ea-a4a7-d074005ed80c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-5
msc.type: authoredcontent
ms.openlocfilehash: e205788e05a2ad94d86d4847c11c40898b1c3113
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="part-5-business-logic"></a>Часть 5: Бизнес-логику
====================
по [(Joe Stagner)](https://github.com/JoeStagner)

> Tailspin Spyworks показано, как чрезвычайно простой можно создавать мощные и масштабируемые приложения для платформы .NET. Он показывает как использовать новые возможности в ASP.NET 4 для создания Интернет-магазина, включая покупок, извлечения и администрирования.
> 
> Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks. Часть 5 добавляет некоторые бизнес-логики.


## <a id="_Toc260221671"></a>Добавление некоторых бизнес-логики

Мы хотим опыт покупок были доступны при посещении веб-узла. Посетители смогут просматривать и добавлять элементы в корзину покупок, даже если они не зарегистрированы или не вошел в систему. По мере готовности извлечение пользователь получает возможность проверки подлинности и если они не еще члены их можно будет создать учетную запись.

Это означает, что нам потребуется реализовать логику для преобразования в корзину для покупок из анонимного состояния в состояние «Зарегистрирован пользователь».

Давайте создайте каталог с именем «Классы» а затем правой кнопкой мыши папку и создать новый файл «Класс» с именем MyShoppingCart.cs

![](tailspin-spyworks-part-5/_static/image1.jpg)

![](tailspin-spyworks-part-5/_static/image1.png)

Как уже говорилось выше, мы расширение класса, реализующего MyShoppingCart.aspx страницы, и мы сделаем это с помощью. Конструкция мощный «разделяемого класса» NET элемента.

Созданный вызова для нашего файла MyShoppingCart.aspx.cf выглядит следующим образом.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample1.cs)]

Обратите внимание на использование ключевого слова «partial».

Файл класса, который мы только что созданную выглядит следующим образом.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample2.cs)]

Мы объединит нашей реализации путем добавления в этот файл, а также ключевое слово partial.

Наш новый файл класса теперь выглядит следующим образом.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample3.cs)]

Первый метод, который мы добавим наш класс имеет метод «AddItem». Это метод, который в конечном счете будет вызываться при нажатии ссылки «Добавить для иллюстрации» на страницах список продуктов и сведения о продукте.

Добавьте следующее в с помощью инструкций в верхней части страницы.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample4.cs)]

И добавьте этот метод в класс MyShoppingCart.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample5.cs)]

Мы используем LINQ to Entities для просмотра, если элемент уже существует в корзине. Если таким образом, мы обновить количество заказа товара, в противном случае мы создать новую запись для выбранного элемента

Для вызова этого метода мы реализуем страницу AddToCart.aspx, не только этот метод класса, но затем отображается текущий Корзина для покупок = после добавления элемента.

Щелкните правой кнопкой мыши имя решения в обозревателе решений и добавьте и новая страница с именем AddToCart.aspx, как мы сделали ранее.

Хотя эту страницу можно использовать для отображения промежуточные результаты как низкий стандартных проблем, и т.д., в нашей реализации, страницы фактически не отрисовки, но вместо вызывает логику «Добавить» и перенаправления.

Для этого мы добавим код к странице\_события Load.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample6.cs)]

Обратите внимание, что, получаемых продукта, чтобы добавить в корзину покупок из параметра строки запроса и вызова метода AddItem класса.

Если ошибки не обнаружены управление передается на страницу SHoppingCart.aspx, которая полностью мы реализуем рядом. Если ошибку следует мы создаем исключение.

В настоящее время мы еще не реализована глобального обработчика ошибок, это исключение будет исключен обработанное нашего приложения, но мы будет исправить это чуть ниже.

Также Обратите внимание на использование инструкции (доступно через Debug.Fail()`using System.Diagnostics;)`

— Приложение выполняется в отладчике, этот метод отображает подробные сведения о состоянии приложений, а также сообщение об ошибке, мы указываем диалоговое окно.

При выполнении в рабочей среде Debug.Fail(), оператор игнорируется.

Можно заметить в коде выше вызова метода в нашей корзину покупок класс имена «GetShoppingCartId».

Добавьте код для реализации метода, как показано ниже.

Обратите внимание, что мы также добавили кнопки обновления и извлечения и метки, где можно отобразить покупок «итог».

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample7.cs)]

Мы теперь можно добавить элементы в наш список покупок, но не были реализованы логику для отображения в корзину, после добавления продукта.

Таким образом на странице MyShoppingCart.aspx мы добавим элемент управления EntityDataSource и управления GridVire следующим образом.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample8.aspx)]

Вызовите форму в конструкторе, можно дважды нажмите кнопку Обновить корзину и создать обработчик событий click, указанный в объявлении в разметке.

Реализуется сведения позже, но это сообщите нам построения и запуска приложения без ошибок.

При запуске приложения и добавить элемент в корзину покупок появится следующее.

![](tailspin-spyworks-part-5/_static/image2.jpg)

Обратите внимание, что мы deviated в сетке отображается «default» путем реализации трех настраиваемых столбцов.

Во-первых, неизменяемое, поле «Связанный» для количества:

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample9.aspx)]

Далее — «вычисляемый» столбец, который отображает линейный итог (элемент затраты времени количество упорядоченный):

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample10.aspx)]

Наконец у нас есть настраиваемый столбец, содержащий элемент управления CheckBox, пользователь будет использовать для указания удаления элемента из покупательской диаграммы.

[!code-aspx[Main](tailspin-spyworks-part-5/samples/sample11.aspx)]

![](tailspin-spyworks-part-5/_static/image3.jpg)

Как видите, порядок, в итоговой строке является пустым, так что давайте добавить логику для вычисления итог заказа.

Сначала мы будет реализован метод «GetTotal», используемую для нашего MyShoppingCart класса.

В файле MyShoppingCart.cs добавьте следующий код.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample12.cs)]

Затем на странице\_мы будем можно вызвать метод наших GetTotal, используемую обработчик события загрузки. В то же время мы добавим тест пуста ли покупательской корзине и соответствующим образом настроить отображение, если он является.

Теперь если Корзина пуста мы получаем это:

![](tailspin-spyworks-part-5/_static/image4.jpg)

А если нет, мы увидим, нашей всего.

![](tailspin-spyworks-part-5/_static/image5.jpg)

Тем не менее эта страница еще не завершено.

Мы потребуется дополнительная логика пересчитать покупательской корзины, удаляя элементы, помеченные для удаления и определяя новые значения количества, как некоторые может быть изменено в сетке пользователя.

Позволяет добавить метод «RemoveItem» наш класс покупательской корзины в MyShoppingCart.cs для обработки случая, когда пользователь отмечает элемент для удаления.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample13.cs)]

Теперь давайте ad метод для обработки ситуации, когда пользователь изменяет просто качество располагаются в GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample14.cs)]

Основные возможности удаления и обновления на месте позволяет реализовать логику, которая фактически обновляет список покупок в базе данных. (В MyShoppingCart.cs)

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample15.cs)]

Можно заметить, что этот метод ожидает два параметра. Один покупательской корзине идентификатор, а другой — массив объектов, определяемых пользователем типа.

Таким образом, чтобы свести к минимуму зависимости логику на особенности интерфейса пользователя, мы определили структуру данных, который можно использовать для передачи покупок покупок в наш код без наш метод необходимости прямого доступа к элемента управления GridView.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample16.cs)]

В наш файл MyShoppingCart.aspx.cs можно использовать эту структуру в нашем обработчик события Click кнопки обновления следующим образом. Обратите внимание, что наряду с обновлением в корзину Корпорация Майкрософт будет обновлять все покупок.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample17.cs)]

Отметьте особый интерес представляет эту строку кода:

[!code-javascript[Main](tailspin-spyworks-part-5/samples/sample18.js)]

GetValues() является специальные вспомогательная функция, которая мы реализуем в MyShoppingCart.aspx.cs, как показано ниже.

[!code-csharp[Main](tailspin-spyworks-part-5/samples/sample19.cs)]

Это обеспечивает чистую способ доступа к значениям связанных элементов в наш элемент управления GridView. Поскольку наш элемент управления CheckBox «Удалить элемент» не привязан мы будем доступ к нему через метод FindControl().

На этом этапе разработки проекта мы получаем готовы реализовать процесс извлечения.

Прежде чем таким образом, давайте Visual Studio можно используйте для создания базы данных членства и Добавление пользователя в хранилище данных членства.

>[!div class="step-by-step"]
[Назад](tailspin-spyworks-part-4.md)
[Вперед](tailspin-spyworks-part-6.md)
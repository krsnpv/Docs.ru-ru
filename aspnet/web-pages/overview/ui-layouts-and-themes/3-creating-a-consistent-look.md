---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: "Создание согласованного макета в ASP.NET Web Pages (Razor) узлов | Документы Microsoft"
author: tfitzmac
description: "Чтобы сделать его более эффективно создавать веб-страницы для веб-узла, можно создать многократно используемых блоков содержимого (например, верхние и нижние колонтитулы) для веб-сайта и вы сможете..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/10/2014
ms.topic: article
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 2c7631017f7c0fb31f43320c2ab78baddd87b516
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Создание согласованного макета веб-страниц (Razor) узлов ASP.NET
====================
по [Tom FitzMacken](https://github.com/tfitzmac)

> В этой статье объясняется, как можно использовать страницы макета в на веб-сайт ASP.NET Web Pages (Razor) для создания многократно используемых блоков содержимого (например, верхние и нижние колонтитулы) и создание согласованного вида для всех страниц на сайте.
> 
> **Что вы узнаете следующее.** 
> 
> - Создание многократно используемых блоков содержимого, такого как верхние и нижние колонтитулы.
> - Инструкции по созданию согласованного вида для все страницы сайта с использованием макета.
> - Способ передачи данных во время выполнения к странице макета.
> 
> Существуют следующие функции ASP.NET, представленные в статье:
> 
> - Блоки содержимого, которые представляют собой файлы, содержащие содержимое в формате HTML для вставки на нескольких страницах.
> - Макет страницы, которые являются страницы, содержащие содержимое в формате HTML, который можно использовать совместно со страниц веб-сайта.
> - `RenderPage`, `RenderBody`, И `RenderSection` методы, которые определить место вставки элементов страницы ASP.NET.
> - `PageData` Словарь, который позволяет совместно использовать данные блоки содержимого и страницы макета.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Версии программного обеспечения, используемая в этом учебнике
> 
> 
> - Веб-страниц ASP.NET (Razor) 3
>   
> 
> Этот учебник также работает с веб-страницы ASP.NET 2.


## <a name="about-layout-pages"></a>Сведения о страницах макета

Содержимое, которое отображается на каждой странице, таких как заголовок и нижний колонтитул или поле, которое сообщает пользователям, что они вошли у многих веб-сайтов. ASP.NET позволяет создавать отдельный файл с блок содержимого, который может содержать текст, разметку и код, так же, как обычный веб-страницы. Затем можно вставить блок содержимого на других страницах на сайте место вставки. В этом случае не нужно скопировать и вставить содержимое в каждой странице. Создание общих содержимого, такого как это также упрощает для обновления веб-узла. Если вам нужно изменить содержимое, можно обновлять только один файл и затем созданной everywhere вставленного содержимого.

На следующей диаграмме показан как содержимое блокирует работы. Когда браузер запрашивает страницу с веб-сервера, ASP.NET вставляет блоки содержимого в точке, где `RenderPage` будет вызван на главной странице. Страница завершения (объединенные) затем отправляется в браузер.

![Концептуальная схема, показывающая, как метод RenderPage вставляет страницей, на которую указывает ссылка в текущей страницы.](3-creating-a-consistent-look/_static/image1.jpg)

В этой процедуре создается страница, которая ссылается на два блоки содержимого (заголовок и нижний колонтитул), расположенных в отдельных файлах. Эти же блоки содержимого можно использовать на любой странице веб-узла. Когда закончите, вы получите страницы следующим образом:

![Снимок экрана, показывающий страницу в браузере, получаемые в результате выполнения содержит вызовы метода RenderPage.](3-creating-a-consistent-look/_static/image2.jpg)

1. В корневой папке веб-сайта, создайте файл с именем *Index.cshtml*.
2. Замените существующую разметку следующей:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. В корневой папке создайте папку с именем *Shared*.

    > [!NOTE]
    > Часто для хранения файлов, которые являются общими для веб-страницы в папку с именем *Shared*.
4. В *Shared* папки, создайте файл с именем  *\_Header.cshtml*.
5. Замените все существующее содержимое следующим кодом:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Обратите внимание, что имя файла  *\_Header.cshtml*, с символа подчеркивания (\_) с префиксом. ASP.NET не будет отправлять страницы в браузере, если его имя начинается с символа подчеркивания. Это позволяет предотвратить людей запрос (случайно или иным способом) эти страницы напрямую. Рекомендуется использовать знаки подчеркивания на имя страницы, которые имеют блоки содержимого в них, так как вы не хотите действительно пользователи смогут запрашивать эти страницы &#8212; они существуют строго должны быть вставлены в другие страницы.
6. В *Shared* папки, создайте файл с именем  *\_Footer.cshtml* и замените содержимое следующим:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. В *Index.cshtml* добавьте два вызова `RenderPage` метода, как показано ниже:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Показано, как вставить блок содержимого на веб-странице. Вызывается `RenderPage` метода и передачи ей имени файла, содержимое которой нужно вставить в этот момент. Здесь вы вставляете содержимое  *\_Header.cshtml* и  *\_Footer.cshtml* файлы в *Index.cshtml* файла.
8. Запустите *Index.cshtml* страницы в браузере. (В WebMatrix в **файлы** рабочей области, щелкните правой кнопкой мыши файл, а затем выберите **запуск в браузере**.)
9. В браузере просмотрите исходный код страницы. (Например, в Internet Explorer, щелкните правой кнопкой мыши страницу и выберите **источник**.)

    Это позволяет увидеть разметки веб-страницы, который отправляется в браузер, объединяющего разметку страницы индекса блоки содержимого. В следующем примере показан исходный код страницы, отображаемый для *Index.cshtml*. Вызовы `RenderPage` , вставленных в *Index.cshtml* были заменены фактическое содержимое файлов верхнего и нижнего колонтитулов.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Создание согласованного вида с помощью страницы макета

Теперь вы знаете, можно легко включить содержимое на нескольких страницах. Более структурированный подход к созданию согласованного вида для сайта является использование макета страницы. Страница макета определяет структуру веб-страницы, но не содержит действительного содержимого. После создания макета страницы, можно создать веб-страницы с содержимым и затем связать их с макетом страницы. При отображении этих страниц, они будут отформатированное в соответствии со страницы макета. (В этом смысле страница макета действует в качестве шаблона для содержимого, которое определено в другие страницы.)

Страница макета — так же, как и любой HTML-страницы, за исключением того, что он содержит вызов `RenderBody` метод. Положение `RenderBody` метода на странице макета определяет, где будут включены сведения на странице содержимого.

На следующей схеме показано, как содержимое страницы и страницы макета объединяются во время выполнения для создания веб-странице завершения работы. Браузер запрашивает страницу содержимого. Страница содержимого содержит код с указанием страницы макета для страницы структуру. На странице «макет» вставляется содержимое в точке, где `RenderBody` вызывается метод. Содержимого блоков можно также вставлять в макет страницы путем вызова `RenderPage` метод способом, как и в предыдущем разделе. После завершения веб-страницы, он будет отправлен в браузере.

![Снимок экрана, показывающий страницу в браузере, получаемые в результате выполнения содержит вызовы метода RenderBody.](3-creating-a-consistent-look/_static/image3.jpg)

Ниже показано, как создать макет страницы содержимого страницы и ссылку на него.

1. В *Shared* папку веб-сайта, создайте файл с именем  *\_Layout1.cshtml*.
2. Замените все существующее содержимое следующим кодом:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Можно использовать `RenderPage` метода на странице макета для вставки содержимого блоков. Страница макета могут содержать только один вызов `RenderBody` метод.
3. В *Shared* папки, создайте файл с именем  *\_Header2.cshtml* и замените все существующее содержимое следующим:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. В корневой папке создайте новую папку с именем *стили*.
5. В *стили* папки, создайте файл с именем *Site.css* и добавьте следующие определения стиля:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Эти определения стиля здесь — только для того, чтобы показать, как можно использовать таблицы стилей со страницы макета. Если требуется, можно определить собственные стили для этих элементов.
6. В корневой папке создайте файл с именем *Content1.cshtml* и замените все существующее содержимое следующим:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Это страница, на которой будет использована страница макета. Блок кода в верхней части страницы указывает, какую страницу макета для форматирования этого содержимого.
7. Запустите *Content1.cshtml* в браузере. В формате отображаемой страницы и таблицы стилей, определенные в  *\_Layout1.cshtml* и текст (содержимое), определенные в *Content1.cshtml*.

    ![[image]](3-creating-a-consistent-look/_static/image4.jpg)

    Можно повторить шаг 6, чтобы создать дополнительные страницы содержимого, которые могут совместно использовать одну и ту же страницу макета.

    > [!NOTE]
    > Можно настроить веб-узла, чтобы автоматически, можно использовать одну и ту же страницу макета для всех страниц содержимого в папке. Дополнительные сведения см. в разделе [поведение Настройка сайта для веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Разработка макета страниц, имеющих несколько разделов содержимого

Это полезно, если вы хотите использовать макетов, имеющих несколько областей с заменяемое содержимое, страницы содержимого может иметь несколько разделов. На странице содержимого вы предоставляете каждого раздела уникальное имя. (Раздел по умолчанию остается без имени.) На странице «макет» добавляется `RenderBody` метод, чтобы указать, где должен отображаться раздел неименованные (по умолчанию). Затем можно добавить отдельный `RenderSection` методы для визуализации именованные разделы по отдельности.

На следующей диаграмме показан как ASP.NET обрабатывает содержимое, которое состоит из нескольких разделов. Каждый именованный раздел содержится в разделе блок на странице содержимого. (Их названия `Header` и `List` в примере.) Платформа вставляет раздела содержимого в страницы макета в точке, где `RenderSection` вызывается метод. Раздел неименованные (по умолчанию) вставляется в точке, где `RenderBody` вызывается метод, как показано выше.

![Концептуальная схема, показывающая, как метод RenderSection вставляет ссылки на разделы, в текущей страницы.](3-creating-a-consistent-look/_static/image5.jpg)

Эта процедура показывает, как создать страницу содержимого, несколько разделов содержимого и Подготовка к просмотру с помощью страницы макета, которая поддерживает несколько разделов содержимого.

1. В *Shared* папки, создайте файл с именем  *\_Layout2.cshtml*.
2. Замените все существующее содержимое следующим кодом:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Вы используете `RenderSection` метод для отображения заголовка и список разделов.
3. В корневой папке создайте файл с именем *Content2.cshtml* и замените все существующее содержимое следующим:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Эта страница содержимого содержит блок кода в верхней части страницы. Каждый именованный раздел заключается в блок раздела. Остальная часть страницы содержит раздел содержимого по умолчанию (без имени).
4. Запустите *Content2.cshtml* в браузере.

    ![Снимок экрана, показывающий страницу в браузере, получаемые в результате выполнения содержит вызовы метода RenderSection.](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>Делая дополнительные разделы содержимого

Как правило разделы, которые можно создать на странице содержимого должны совпадать разделы, определенные на странице макета. Возможно возникновение ошибок при возникновении любого из следующих:

- Страница содержимого содержит раздел с нет соответствующий раздел в странице макета.
- Страница макета содержит раздел, для которого нет содержимого.
- Страница макета включает вызовы методов, служащие для подготовки к просмотру несколько раз в одном разделе.

Однако можно переопределить это поведение для именованного раздела путем объявления раздела как необязательные в странице макета. Это позволяет определить несколько страниц, могут совместно использоваться страница макета, но, которые могут или не могут иметь содержимое для определенного раздела.

1. Откройте *Content2.cshtml* и удалите следующий раздел:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Сохраните страницу, а затем запустите его в браузере. Отображается сообщение об ошибке, так как содержимое страницы не предоставляет содержимое для раздела, определенных на странице макета, а именно: раздел заголовка.

    ![Снимок экрана, на которой отображается ошибка, возникающая при запуске страницы, который вызывает метод RenderSection, однако отсутствует соответствующий раздел.](3-creating-a-consistent-look/_static/image7.jpg)
3. В *Shared* откройте  *\_Layout2.cshtml* страницы и замените эту строку:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    с помощью следующего кода:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    В качестве альтернативы может замените следующий блок кода, который позволяет получить те же результаты предыдущей строке кода:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Запустите *Content2.cshtml* страницы в браузере еще раз. (При наличии этой странице откройте в браузере можно просто обновить его.) Это время страницы отображается без ошибок, даже при наличии отсутствует заголовок.

## <a name="passing-data-to-layout-pages"></a>Передача данных на макет страницы

Возможно, данные, определенные на странице содержимого, вам потребуется обратиться к странице макета. В этом случае нужно передать данные из содержимого страницы к странице макета. Например может потребоваться отображение состояния входа пользователя, или может потребоваться отображение или скрытие области содержимого, на основе ввода пользователя.

Для передачи данных из страницы содержимого страницы макета, можно разместить значения в `PageData` свойство страницы содержимого. `PageData` Свойство является коллекцией пар "имя значение", содержащих данные, передаваемые между страницами. На странице «макет» могут затем считывать значения из `PageData` свойство.

Вот другой схеме. Здесь показано, как можно использовать ASP.NET `PageData` свойство, чтобы передать значения от содержимого страницы к странице макета. Когда ASP.NET начинает создавать веб-страницы, он создает `PageData` коллекции. На странице содержимого написать код, чтобы поместить данные в `PageData` коллекции. Значения в `PageData` коллекции можно получить дополнительные блоки содержимого или других разделах на странице содержимого.

![Концептуальная схема, показывающая, как заполнить словарь PageData и передать эту информацию для страницы макета содержимого страницы.](3-creating-a-consistent-look/_static/image8.jpg)

Ниже показано, как передавать данные из содержимого страницы к странице макета. При запуске страницы отображаются кнопки, которая дает пользователю возможность скрывать или отображать список, который определен в макете страницы. При нажатии пользователем кнопки, устанавливает значение true или false (Boolean) `PageData` свойство. Страница макета считывает это значение и если это значение равно false, скрытие списка. Значение также используется для определения, следует ли отображать на странице содержимого **скрыть список** кнопку или **Показать список** кнопки.

![[image]](3-creating-a-consistent-look/_static/image9.jpg)

1. В корневой папке создайте файл с именем *Content3.cshtml* и замените все существующее содержимое следующим:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    Код сохраняет два блока данных в `PageData` свойство &#8212; заголовок веб-страницы и значение true или false, чтобы указать, следует ли отображать список.

    Обратите внимание, что ASP.NET позволяет разместить разметку HTML на странице, условно с помощью блока кода. Например `if/else` блок в основной области страницы определяет, какие формы для отображения в зависимости от того, следует ли `PageData["ShowList"]` задано значение true.
2. В *Shared* папки, создайте файл с именем  *\_Layout3.cshtml* и замените все существующее содержимое следующим:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    Страница макета включает выражение в `<title>` элемент, который возвращает значение заголовка из `PageData` свойство. Она также использует `ShowList` значение `PageData` свойства, чтобы определить, следует ли отображать блок содержимого списка.
3. В *Shared* папки, создайте файл с именем  *\_List.cshtml* и замените все существующее содержимое следующим:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Запустите *Content3.cshtml* страницы в браузере. Откроется страница со списком отображается в левой части страницы и **скрыть список** кнопку внизу.

    ![Снимок экрана со страницей, включающий список "и", «Скрыть список» кнопку.](3-creating-a-consistent-look/_static/image10.jpg)
5. Нажмите кнопку **скрыть список**. Список исчезает, а кнопка изменится на **Показать список**.

    ![Снимок экрана со страницей, не включает список и отобразить список, кнопку.](3-creating-a-consistent-look/_static/image11.jpg)
6. Нажмите кнопку **Показать список** кнопки, а также список отображается снова.

## <a name="additional-resources"></a>Дополнительные ресурсы


[Настройка поведения сайта для веб-страниц ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: "Используя шаблоны FormView (VB) | Документы Microsoft"
author: rick-anderson
description: "В отличие от DetailsView FormView не состоит из полей. Вместо этого FormView подготавливается к просмотру с помощью шаблонов. В этом учебнике мы рассмотрим е. с помощью..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: 05e97ce5efeaf72192ed294b946e2249c60007d1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-the-formviews-templates-vb"></a><span data-ttu-id="d0a01-105">Используя шаблоны FormView (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="d0a01-105">Using the FormView's Templates (VB)</span></span>
====================
<span data-ttu-id="d0a01-106">по [Скотт Митчелл](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="d0a01-106">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="d0a01-107">[Загрузить пример приложения](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe) или [скачать PDF](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)</span><span class="sxs-lookup"><span data-stu-id="d0a01-107">[Download Sample App](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe) or [Download PDF](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)</span></span>

> <span data-ttu-id="d0a01-108">В отличие от DetailsView FormView не состоит из полей.</span><span class="sxs-lookup"><span data-stu-id="d0a01-108">Unlike the DetailsView, the FormView is not composed of fields.</span></span> <span data-ttu-id="d0a01-109">Вместо этого FormView подготавливается к просмотру с помощью шаблонов.</span><span class="sxs-lookup"><span data-stu-id="d0a01-109">Instead, the FormView is rendered using templates.</span></span> <span data-ttu-id="d0a01-110">В данном руководстве, мы изучим с помощью элемента управления FormView для представления менее строгое представление данных.</span><span class="sxs-lookup"><span data-stu-id="d0a01-110">In this tutorial we'll examine using the FormView control to present a less rigid display of data.</span></span>


## <a name="introduction"></a><span data-ttu-id="d0a01-111">Вступление</span><span class="sxs-lookup"><span data-stu-id="d0a01-111">Introduction</span></span>

<span data-ttu-id="d0a01-112">В двух последних учебниках мы узнали, как для настройки выходов GridView и DetailsView элементов управления при помощи TemplateFields.</span><span class="sxs-lookup"><span data-stu-id="d0a01-112">In the last two tutorials we saw how to customize the GridView and DetailsView controls' outputs using TemplateFields.</span></span> <span data-ttu-id="d0a01-113">Разрешить TemplateFields к содержимому для конкретного поля высокой настраивать, но в конечном итоге GridView и DetailsView имеют вид вместо boxy, вид сетки.</span><span class="sxs-lookup"><span data-stu-id="d0a01-113">TemplateFields allow for the contents for a specific field to be highly customized, but in the end both the GridView and DetailsView have a rather boxy, grid-like appearance.</span></span> <span data-ttu-id="d0a01-114">Во многих сценариях макета сетки таких идеально, но в некоторых случаях требуется более гибкий, менее строгое отображения.</span><span class="sxs-lookup"><span data-stu-id="d0a01-114">For many scenarios such a grid-like layout is ideal, but at times a more fluid, less rigid display is needed.</span></span> <span data-ttu-id="d0a01-115">При отображении одной записи, возможно с помощью элемента управления FormView гибкий макет.</span><span class="sxs-lookup"><span data-stu-id="d0a01-115">When displaying a single record, such a fluid layout is possible using the FormView control.</span></span>

<span data-ttu-id="d0a01-116">В отличие от DetailsView FormView не состоит из полей.</span><span class="sxs-lookup"><span data-stu-id="d0a01-116">Unlike the DetailsView, the FormView is not composed of fields.</span></span> <span data-ttu-id="d0a01-117">Не удается добавить BoundField и TemplateField в FormView.</span><span class="sxs-lookup"><span data-stu-id="d0a01-117">You can't add a BoundField or TemplateField to a FormView.</span></span> <span data-ttu-id="d0a01-118">Вместо этого FormView подготавливается к просмотру с помощью шаблонов.</span><span class="sxs-lookup"><span data-stu-id="d0a01-118">Instead, the FormView is rendered using templates.</span></span> <span data-ttu-id="d0a01-119">Рассматривать как элемент управления DetailsView, содержащий один TemplateField FormView.</span><span class="sxs-lookup"><span data-stu-id="d0a01-119">Think of the FormView as a DetailsView control that contains a single TemplateField.</span></span> <span data-ttu-id="d0a01-120">FormView поддерживает следующие шаблоны:</span><span class="sxs-lookup"><span data-stu-id="d0a01-120">The FormView supports the following templates:</span></span>

- <span data-ttu-id="d0a01-121">`ItemTemplate`используется для отрисовки конкретной записи, отображаемых в FormView</span><span class="sxs-lookup"><span data-stu-id="d0a01-121">`ItemTemplate` used to render the particular record displayed in the FormView</span></span>
- <span data-ttu-id="d0a01-122">`HeaderTemplate`используется для указания строки необязательный заголовок</span><span class="sxs-lookup"><span data-stu-id="d0a01-122">`HeaderTemplate` used to specify an optional header row</span></span>
- <span data-ttu-id="d0a01-123">`FooterTemplate`используется для указания строка необязательно нижнего колонтитула</span><span class="sxs-lookup"><span data-stu-id="d0a01-123">`FooterTemplate` used to specify an optional footer row</span></span>
- <span data-ttu-id="d0a01-124">`EmptyDataTemplate`Когда FormView `DataSource` отсутствуют какие-либо записи `EmptyDataTemplate` используется вместо `ItemTemplate` для визуализации разметки для элемента управления</span><span class="sxs-lookup"><span data-stu-id="d0a01-124">`EmptyDataTemplate` when the FormView's `DataSource` lacks any records, the `EmptyDataTemplate` is used in place of the `ItemTemplate` for rendering the control's markup</span></span>
- <span data-ttu-id="d0a01-125">`PagerTemplate`можно использовать для настройки интерфейса постраничного просмотра для FormViews, имеющих включено разбиение на страницы</span><span class="sxs-lookup"><span data-stu-id="d0a01-125">`PagerTemplate` can be used to customize the paging interface for FormViews that have paging enabled</span></span>
- <span data-ttu-id="d0a01-126">`EditItemTemplate` / `InsertItemTemplate`позволяет настраивать интерфейс редактирования или интерфейс для вставки для FormViews, который поддерживает такие функциональные возможности</span><span class="sxs-lookup"><span data-stu-id="d0a01-126">`EditItemTemplate` / `InsertItemTemplate` used to customize the editing interface or inserting interface for FormViews that support such functionality</span></span>

<span data-ttu-id="d0a01-127">В данном руководстве, мы изучим с помощью элемента управления FormView для представления менее строгое отображения продуктов.</span><span class="sxs-lookup"><span data-stu-id="d0a01-127">In this tutorial we'll examine using the FormView control to present a less rigid display of products.</span></span> <span data-ttu-id="d0a01-128">Вместо того чтобы использовать поля имени, категории, поставщика и т. д., FormView `ItemTemplate` отобразит эти значения, используя сочетание элемент заголовка и `<table>` (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="d0a01-128">Rather than having fields for the name, category, supplier, and so on, the FormView's `ItemTemplate` will show these values using a combination of a header element and a `<table>` (see Figure 1).</span></span>


<span data-ttu-id="d0a01-129">[![FormView разрывов вид сетки макета в DetailsView](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d0a01-129">[![The FormView Breaks Out of the Grid-Like Layout Seen in the DetailsView](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)</span></span>

<span data-ttu-id="d0a01-130">**Рис. 1**: FormView разбивает Grid-Like макета, видны в DetailsView ([Просмотр полноразмерное изображение](using-the-formview-s-templates-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d0a01-130">**Figure 1**: The FormView Breaks Out of the Grid-Like Layout Seen in the DetailsView ([Click to view full-size image](using-the-formview-s-templates-vb/_static/image3.png))</span></span>


## <a name="step-1-binding-the-data-to-the-formview"></a><span data-ttu-id="d0a01-131">Шаг 1: Привязка данных к FormView</span><span class="sxs-lookup"><span data-stu-id="d0a01-131">Step 1: Binding the Data to the FormView</span></span>

<span data-ttu-id="d0a01-132">Откройте `FormView.aspx` страницы и перетащите FormView из области элементов в конструктор.</span><span class="sxs-lookup"><span data-stu-id="d0a01-132">Open the `FormView.aspx` page and drag a FormView from the Toolbox onto the Designer.</span></span> <span data-ttu-id="d0a01-133">При первом добавлении FormView он отображается как серый прямоугольник, предписывая нам, `ItemTemplate` необходим.</span><span class="sxs-lookup"><span data-stu-id="d0a01-133">When first adding the FormView it appears as a gray box, instructing us that an `ItemTemplate` is needed.</span></span>


<span data-ttu-id="d0a01-134">[![FormView невозможно отобразить в конструкторе, пока не предоставлены ItemTemplate](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="d0a01-134">[![The FormView Cannot be Rendered in the Designer Until an ItemTemplate is Provided](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)</span></span>

<span data-ttu-id="d0a01-135">**На рисунке 2**: FormView невозможно отобразить в конструкторе до `ItemTemplate` предоставляется ([Просмотр полноразмерное изображение](using-the-formview-s-templates-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d0a01-135">**Figure 2**: The FormView Cannot be Rendered in the Designer Until an `ItemTemplate` is Provided ([Click to view full-size image](using-the-formview-s-templates-vb/_static/image6.png))</span></span>


<span data-ttu-id="d0a01-136">`ItemTemplate` Могут быть созданы вручную (с помощью декларативного синтаксиса) или может быть создан автоматически путем привязки к элементу управления источником данных с помощью конструктора FormView.</span><span class="sxs-lookup"><span data-stu-id="d0a01-136">The `ItemTemplate` can be created by hand (through the declarative syntax) or can be auto-created by binding the FormView to a data source control through the Designer.</span></span> <span data-ttu-id="d0a01-137">Это автоматически созданной `ItemTemplate` содержит код HTML, что списки имя каждого поля и метки управления которого `Text` свойство привязано к значению поля.</span><span class="sxs-lookup"><span data-stu-id="d0a01-137">This auto-created `ItemTemplate` contains HTML that lists the name of each field and a Label control whose `Text` property is bound to the field's value.</span></span> <span data-ttu-id="d0a01-138">Такой подход также auto создает `InsertItemTemplate` и `EditItemTemplate`, которые заполняются элементы управления вводом для каждого поля данных, возвращаемых элементом управления источника данных.</span><span class="sxs-lookup"><span data-stu-id="d0a01-138">This approach also auto-creates an `InsertItemTemplate` and `EditItemTemplate`, both of which are populated with input controls for each of the data fields returned by the data source control.</span></span>

<span data-ttu-id="d0a01-139">Если вы хотите автоматически создать шаблон, с помощью смарт-тега FormView добавить новый элемент управления ObjectDataSource, который вызывает `ProductsBLL` класса `GetProducts()` метод.</span><span class="sxs-lookup"><span data-stu-id="d0a01-139">If you want to auto-create the template, from the FormView's smart tag add a new ObjectDataSource control that invokes the `ProductsBLL` class's `GetProducts()` method.</span></span> <span data-ttu-id="d0a01-140">Это создаст FormView с `ItemTemplate`, `InsertItemTemplate`, и `EditItemTemplate`.</span><span class="sxs-lookup"><span data-stu-id="d0a01-140">This will create a FormView with an `ItemTemplate`, `InsertItemTemplate`, and `EditItemTemplate`.</span></span> <span data-ttu-id="d0a01-141">Удалите из представления источника, `InsertItemTemplate` и `EditItemTemplate` поддерживает, так как мы не интересует создание элемента FormView, изменяемых или вставляемых еще.</span><span class="sxs-lookup"><span data-stu-id="d0a01-141">From the Source view, remove the `InsertItemTemplate` and `EditItemTemplate` since we're not interested in creating a FormView that supports editing or inserting yet.</span></span> <span data-ttu-id="d0a01-142">Далее очистить разметка в рамках `ItemTemplate` таким образом, что мы для работы с нуля.</span><span class="sxs-lookup"><span data-stu-id="d0a01-142">Next, clear out the markup within the `ItemTemplate` so that we have a clean slate to work from.</span></span>

<span data-ttu-id="d0a01-143">Если бы вместо надстраивать `ItemTemplate` вручную, можно добавить и настроить элемент управления ObjectDataSource, перетащив его из области элементов в конструктор.</span><span class="sxs-lookup"><span data-stu-id="d0a01-143">If you'd rather build up the `ItemTemplate` manually, you can add and configure the ObjectDataSource by dragging it from the Toolbox onto the Designer.</span></span> <span data-ttu-id="d0a01-144">Тем не менее не устанавливается FormView источника данных из конструктора.</span><span class="sxs-lookup"><span data-stu-id="d0a01-144">However, don't set the FormView's data source from the Designer.</span></span> <span data-ttu-id="d0a01-145">Вместо этого перейдите в представление источника и вручную задать FormView `DataSourceID` свойства `ID` значение ObjectDataSource.</span><span class="sxs-lookup"><span data-stu-id="d0a01-145">Instead, go to the Source view and manually set the FormView's `DataSourceID` property to the `ID` value of the ObjectDataSource.</span></span> <span data-ttu-id="d0a01-146">Вручную добавьте `ItemTemplate`.</span><span class="sxs-lookup"><span data-stu-id="d0a01-146">Next, manually add the `ItemTemplate`.</span></span>

<span data-ttu-id="d0a01-147">Независимо от того, какой подход решил воспользоваться, на этом этапе в FormView декларативная разметка должна выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d0a01-147">Regardless of what approach you decided to take, at this point your FormView's declarative markup should look like:</span></span>


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

<span data-ttu-id="d0a01-148">Поставьте флажок Включить постраничный просмотр в FormView смарт-тегов; При этом будет добавлено `AllowPaging="True"` атрибут декларативный синтаксис FormView.</span><span class="sxs-lookup"><span data-stu-id="d0a01-148">Take a moment to check the Enable Paging checkbox in the FormView's smart tag; this will add the `AllowPaging="True"` attribute to the FormView's declarative syntax.</span></span> <span data-ttu-id="d0a01-149">Кроме того, задать `EnableViewState` свойству значение False.</span><span class="sxs-lookup"><span data-stu-id="d0a01-149">Also, set the `EnableViewState` property to False.</span></span>

## <a name="step-2-defining-theitemtemplates-markup"></a><span data-ttu-id="d0a01-150">Шаг 2: Определение`ItemTemplate`элемента разметки</span><span class="sxs-lookup"><span data-stu-id="d0a01-150">Step 2: Defining the`ItemTemplate`'s Markup</span></span>

<span data-ttu-id="d0a01-151">FormView привязку к элементу управления ObjectDataSource и настроен для поддержки разбиения на страницы мы готовы укажите содержимое `ItemTemplate`.</span><span class="sxs-lookup"><span data-stu-id="d0a01-151">With the FormView bound to the ObjectDataSource control and configured to support paging we're ready to specify the content for the `ItemTemplate`.</span></span> <span data-ttu-id="d0a01-152">В этом учебнике будет имя продукта, отображаемое в `<h3>` заголовок.</span><span class="sxs-lookup"><span data-stu-id="d0a01-152">For this tutorial, let's have the product's name displayed in an `<h3>` heading.</span></span> <span data-ttu-id="d0a01-153">После этого воспользуемся HTML `<table>` для отображения оставшихся свойства продукта в таблице четыре столбца, где первый и третий столбцы список имен свойств, а вторая и четвертая перечислить их значения.</span><span class="sxs-lookup"><span data-stu-id="d0a01-153">Following that, let's use an HTML `<table>` to display the remaining product properties in a four-column table where the first and third columns list the property names and the second and fourth list their values.</span></span>

<span data-ttu-id="d0a01-154">Эта разметка может быть введено в через интерфейс редактирования шаблона FormView в конструкторе или введены вручную посредством декларативного синтаксиса.</span><span class="sxs-lookup"><span data-stu-id="d0a01-154">This markup can be entered in through the FormView's template editing interface in the Designer or entered manually through the declarative syntax.</span></span> <span data-ttu-id="d0a01-155">При работе с шаблонами обычно найти его проще работать непосредственно с декларативного синтаксиса, но вы можете использовать ту же методику удобно наиболее с.</span><span class="sxs-lookup"><span data-stu-id="d0a01-155">When working with templates I typically find it quicker to work directly with the declarative syntax, but feel free to use whatever technique you're most comfortable with.</span></span>

<span data-ttu-id="d0a01-156">В следующем примере показана декларативная разметка FormView после `ItemTemplate`его завершения структуры:</span><span class="sxs-lookup"><span data-stu-id="d0a01-156">The following markup shows the FormView declarative markup after the `ItemTemplate`'s structure has been completed:</span></span>


[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

<span data-ttu-id="d0a01-157">Обратите внимание, что синтаксис привязки данных — `<%# Eval("ProductName") %>`для примера могут быть добавлены непосредственно в выходные данные шаблона.</span><span class="sxs-lookup"><span data-stu-id="d0a01-157">Notice that the databinding syntax - `<%# Eval("ProductName") %>`, for example can be injected directly into the template's output.</span></span> <span data-ttu-id="d0a01-158">То есть их требуется не назначить элемент управления Label `Text` свойство.</span><span class="sxs-lookup"><span data-stu-id="d0a01-158">That is, it need not be assigned to a Label control's `Text` property.</span></span> <span data-ttu-id="d0a01-159">Например, у нас есть `ProductName` значению, отображаемому в `<h3>` элемента с помощью `<h3><%# Eval("ProductName") %></h3>`, что для продукта Chai будут отображаться как `<h3>Chai</h3>`.</span><span class="sxs-lookup"><span data-stu-id="d0a01-159">For example, we have the `ProductName` value displayed in an `<h3>` element using `<h3><%# Eval("ProductName") %></h3>`, which for the product Chai will render as `<h3>Chai</h3>`.</span></span>

<span data-ttu-id="d0a01-160">`ProductPropertyLabel` И `ProductPropertyValue` классы CSS, используются для указания стиль продукта имена и значения свойств в `<table>`.</span><span class="sxs-lookup"><span data-stu-id="d0a01-160">The `ProductPropertyLabel` and `ProductPropertyValue` CSS classes are used for specifying the style of the product property names and values in the `<table>`.</span></span> <span data-ttu-id="d0a01-161">Эти классы CSS определены в `Styles.css` и привести имена свойств полужирный и выравниванием по правому краю, добавьте право заполнение значений свойств.</span><span class="sxs-lookup"><span data-stu-id="d0a01-161">These CSS classes are defined in `Styles.css` and cause the property names to be bold and right-aligned and add a right padding to the property values.</span></span>

<span data-ttu-id="d0a01-162">Так как используются не CheckBoxFields с FormView, чтобы показать `Discontinued` значение как флажок необходимо добавить собственный элемент управления CheckBox.</span><span class="sxs-lookup"><span data-stu-id="d0a01-162">Since there are no CheckBoxFields available with the FormView, in order to show the `Discontinued` value as a checkbox we must add our own CheckBox control.</span></span> <span data-ttu-id="d0a01-163">`Enabled` Задано значение False, сделав его только для чтения и этот флажок `Checked` свойство привязано к значению `Discontinued` поля данных.</span><span class="sxs-lookup"><span data-stu-id="d0a01-163">The `Enabled` property is set to False, making it read-only, and the CheckBox's `Checked` property is bound to the value of the `Discontinued` data field.</span></span>

<span data-ttu-id="d0a01-164">С `ItemTemplate` завершения продукта информация отображается в виде гораздо более гибкая.</span><span class="sxs-lookup"><span data-stu-id="d0a01-164">With the `ItemTemplate` complete, the product information is displayed in a much more fluid manner.</span></span> <span data-ttu-id="d0a01-165">Сравните выходные данные DetailsView из последнего курса (рис. 3) с выходными данными, созданные в FormView в этом учебнике (рис. 4).</span><span class="sxs-lookup"><span data-stu-id="d0a01-165">Compare the DetailsView output from the last tutorial (Figure 3) with the output generated by the FormView in this tutorial (Figure 4).</span></span>


<span data-ttu-id="d0a01-166">[![Жесткая вывода DetailsView](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="d0a01-166">[![The Rigid DetailsView Output](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)</span></span>

<span data-ttu-id="d0a01-167">**Рис. 3**: жесткая DetailsView Output ([Просмотр полноразмерное изображение](using-the-formview-s-templates-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="d0a01-167">**Figure 3**: The Rigid DetailsView Output ([Click to view full-size image](using-the-formview-s-templates-vb/_static/image9.png))</span></span>


<span data-ttu-id="d0a01-168">[![Выходные данные жидкости FormView](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="d0a01-168">[![The Fluid FormView Output](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)</span></span>

<span data-ttu-id="d0a01-169">**Рис. 4**: FormView Output жидкости ([Просмотр полноразмерное изображение](using-the-formview-s-templates-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="d0a01-169">**Figure 4**: The Fluid FormView Output ([Click to view full-size image](using-the-formview-s-templates-vb/_static/image12.png))</span></span>


## <a name="summary"></a><span data-ttu-id="d0a01-170">Сводка</span><span class="sxs-lookup"><span data-stu-id="d0a01-170">Summary</span></span>

<span data-ttu-id="d0a01-171">Хотя элементы управления GridView и DetailsView могут быть настроены при помощи TemplateFields выходные данные, и по-прежнему представить свои данные в формате вид сетки, boxy.</span><span class="sxs-lookup"><span data-stu-id="d0a01-171">While the GridView and DetailsView controls can have their output customized using TemplateFields, both still present their data in a grid-like, boxy format.</span></span> <span data-ttu-id="d0a01-172">В тех случаях, когда должно быть показано одной записи с использованием менее строгое макета, FormView является идеальным выбором.</span><span class="sxs-lookup"><span data-stu-id="d0a01-172">For those times when a single record needs to be shown using a less rigid layout, the FormView is an ideal choice.</span></span> <span data-ttu-id="d0a01-173">Как и DetailsView FormView отображает одну запись из его `DataSource`, но в отличие от DetailsView состоит только из шаблонов и не поддерживает полей.</span><span class="sxs-lookup"><span data-stu-id="d0a01-173">Like the DetailsView, the FormView renders a single record from its `DataSource`, but unlike the DetailsView it is composed just of templates and does not support fields.</span></span>

<span data-ttu-id="d0a01-174">Как было показано в этом учебнике, FormView обеспечивает более гибкий макет при отображении одной записи.</span><span class="sxs-lookup"><span data-stu-id="d0a01-174">As we saw in this tutorial, the FormView allows for a more flexible layout when displaying a single record.</span></span> <span data-ttu-id="d0a01-175">В будущих учебниках мы изучим элементов управления DataList и повторителя, которые обеспечивают тот же уровень гибкости в качестве FormsView, однако, могут отображать несколько записей (такие как GridView).</span><span class="sxs-lookup"><span data-stu-id="d0a01-175">In future tutorials we'll examine the DataList and Repeater controls, which provide the same level of flexibility as the FormsView, but are able to display multiple records (like the GridView).</span></span>

<span data-ttu-id="d0a01-176">Программирование довольны!</span><span class="sxs-lookup"><span data-stu-id="d0a01-176">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="d0a01-177">Об авторе</span><span class="sxs-lookup"><span data-stu-id="d0a01-177">About the Author</span></span>

<span data-ttu-id="d0a01-178">[Скотт Митчелл](http://www.4guysfromrolla.com/ScottMitchell.shtml), автор семи ASP/ASP.NET и основателя из [4GuysFromRolla.com](http://www.4guysfromrolla.com), работает с веб-технологиями Майкрософт с 1998 года.</span><span class="sxs-lookup"><span data-stu-id="d0a01-178">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="d0a01-179">Скотт — независимый консультант, trainer и записи.</span><span class="sxs-lookup"><span data-stu-id="d0a01-179">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="d0a01-180">Его последняя книга — [ *диспетчерами учат самостоятельно ASP.NET 2.0 в течение 24 часов*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span><span class="sxs-lookup"><span data-stu-id="d0a01-180">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="d0a01-181">Он может быть достигнута по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) или через его блог, который можно найти в [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span><span class="sxs-lookup"><span data-stu-id="d0a01-181">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

## <a name="special-thanks-to"></a><span data-ttu-id="d0a01-182">Благодарности</span><span class="sxs-lookup"><span data-stu-id="d0a01-182">Special Thanks To</span></span>

<span data-ttu-id="d0a01-183">Этот учебник ряд прошел проверку многие полезные рецензентов.</span><span class="sxs-lookup"><span data-stu-id="d0a01-183">This tutorial series was reviewed by many helpful reviewers.</span></span> <span data-ttu-id="d0a01-184">Основной рецензент этого учебника было E.R.</span><span class="sxs-lookup"><span data-stu-id="d0a01-184">Lead reviewer for this tutorial was E.R.</span></span> <span data-ttu-id="d0a01-185">Gilmore.</span><span class="sxs-lookup"><span data-stu-id="d0a01-185">Gilmore.</span></span> <span data-ttu-id="d0a01-186">Объясняются моих последующих статей для MSDN?</span><span class="sxs-lookup"><span data-stu-id="d0a01-186">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="d0a01-187">Если Да, напишите мне по [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="d0a01-187">If so, drop me a line at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d0a01-188">[Назад](using-templatefields-in-the-detailsview-control-vb.md)
[Вперед](displaying-summary-information-in-the-gridview-s-footer-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d0a01-188">[Previous](using-templatefields-in-the-detailsview-control-vb.md)
[Next](displaying-summary-information-in-the-gridview-s-footer-vb.md)</span></span>
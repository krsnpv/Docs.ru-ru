---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: "Включение автоматического тестирования модулей | Документы Microsoft"
author: microsoft
description: "Шаг 12 показано, как разработать набор автоматических модульных тестов, проверьте нашей обновление NerdDinner функциональность и которое даст нам достоверности для внесения изменений..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 1a4258054d90b2d5bcc06a63fb6f3b4673a4837d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="enable-automated-unit-testing"></a>Включить автоматическое тестирование модулей
====================
по [Microsoft](https://github.com/microsoft)

[Скачать PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Это шаг 12 бесплатных [учебника «Обновление NerdDinner» приложения](introducing-the-nerddinner-tutorial.md) , обходов сквозной как построить небольшой, но завершается веб-приложения с помощью ASP.NET MVC 1.
> 
> Шаг 12 показано, как разработать набор автоматических модульных тестов, проверьте нашей обновление NerdDinner функциональность и которое даст нам уверенность в том, вносить изменения и усовершенствования для приложения в будущем.
> 
> При использовании ASP.NET MVC 3, которому рекомендуется следовать [Приступая к работе с MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) или [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) учебники.


## <a name="nerddinner-step-12-unit-testing"></a>Обновление NerdDinner шаг 12: Модульное тестирование

Давайте разработать набор автоматических модульных тестов, проверьте нашей обновление NerdDinner функциональность и которое даст нам уверенность в том, вносить изменения и усовершенствования для приложения в будущем.

### <a name="why-unit-test"></a>Почему модульный тест?

На диске в документе одного утром иметь внезапное flash вдохновения о приложении, над которым вы работаете. Необходимо учесть, что изменений, можно реализовать, значительно улучшить приложение. Возможно, рефакторинг, очищает код добавляет новый компонент и исправляет ошибку.

На вопрос, вы confronts, когда поступают на компьютер — «safe это делать это улучшение?» Что делать, если внесения изменений имеет побочные эффекты или что-то разрывы? Изменения могут быть простыми и занимает несколько минут для реализации, но что делать, если требуется часы, можно вручную проверить во всех сценариях приложения? Что делать, если забыт описать сценарий и неработающие приложение выходит в производство? Делает это улучшение действительно стоит все?

Автоматические модульные тесты обеспечивают средство подстраховки, которая позволяет непрерывно улучшения приложений и избежать опасаться кода, над которым вы работаете. Автоматических тестов, которые быстро убедитесь, что позволяет уверенно — код, а также расширяет возможности для внесения изменений, вы может в противном случае не испытывали удобнее делать. Они также помогут создать более простого в сопровождении решения и имеют больше времени жизни - какие интересы гораздо выше рентабельность инвестиций.

Платформа ASP.NET MVC позволяет легко и естественным функции приложения модульного тестирования. Она также позволяет проверить управляемую разработки (TDD) рабочий процесс, который позволяет осуществлять разработку на основе тестирования.

### <a name="nerddinnertests-project"></a>NerdDinner.Tests проекта

Если мы создали нашего приложения обновление NerdDinner в начале этого учебника, мы были будет появляться диалоговое окно подтверждения ли мы хотели создать проект модульных тестов направляются вместе с проектом приложения:

![](enable-automated-unit-testing/_static/image1.png)

Мы хранится этот «Да, создать проект модульного теста» переключатель — что привело к «NerdDinner.Tests» проекта, добавляемое в нашем решении:

![](enable-automated-unit-testing/_static/image2.png)

NerdDinner.Tests проект ссылается на сборку проекта приложения обновление NerdDinner и позволяет легко добавлять автоматические тесты, проверить работоспособность приложения его.

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Создание модульных тестов для нашей компании Dinner класс модели

Наш проект NerdDinner.Tests, проверить Dinner класс, созданный при уровень нашей модели мы добавим некоторые тесты.

Мы начнем с создания новой папки в нашем тестовый проект называется «Моделей», куда будет помещать наших тестов, связанных с моделью. Мы затем правой кнопкой мыши папку и выберите **Add -&gt;новый тест** команду меню. Откроется диалоговое окно «Добавление нового теста».

Мы выберем создание «Модульный тест» и присвойте ей имя «DinnerTest.cs»:

![](enable-automated-unit-testing/_static/image3.png)

При нажатии кнопки «ОК» Visual Studio Добавление (и откройте) DinnerTest.cs файл в проект.

![](enable-automated-unit-testing/_static/image4.png)

Шаблон по умолчанию Visual Studio модульного теста имеет множество код шаблона формы в ней, найти несколько запутанным. Давайте очистить просто содержать следующий код:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

Атрибут [TestClass] выше DinnerTest класса идентифицирует его как класс, который будет содержать тесты, а также дополнительный тест инициализации и уничтожения кода. Тесты в нем можно определить путем добавления открытые методы с атрибутом [TestMethod] на них.

Ниже приведены первое из двух тестов для, мы добавим наш класс Dinner. Первый тест проверяет, что нашей компании Dinner недопустим, если новый Dinner создается без правильно задать все свойства. Второй тест проверяет действительность нашей компании Dinner при Dinner имеет все свойства, заданные с допустимыми значениями:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

Можно заметить выше, нашей имен тестов явным образом указать (и немного verbose). Мы делаем, так как мы может оказаться, что создание сотен или тысяч небольших тестов, и мы хотели бы просто быстро определить назначение и поведение каждого из них (особенно если мы ищем по списку ошибок в test runner). Имена теста должна быть названа функциональные возможности, которые они предназначены для проверки. Выше мы используем «существительное\_следует\_команда «шаблон именования.

Мы структурирование тестов с помощью pattern — предназначенную для «Упорядочить, Act, Assert» тестирование «AAA»:

- Упорядочить: Настройка тестируемой единицы
- ACT: Именно модульного теста и записать результаты
- Assert: Проверки поведения

При написании тестов, мы хотим, чтобы избежать необходимости отдельных тестов делать слишком много. Вместо этого каждый тест следует проверить только один понятием (который будет значительно упростит для выявления причины сбоев). Рекомендуется является попробовать и иметь только один assert инструкции для каждого теста. Если имеется более одного оператора в методе теста контроля, убедитесь, что они все используются для тестирования то же самое. Если вы сомневаетесь, сделайте еще один тест.

### <a name="running-tests"></a>Запущенные тесты

Visual Studio 2008 Professional (и более поздних версий) включает встроенные тестов, которые можно использовать для выполнения модульных тестов Visual Studio проектов в среде IDE. Мы можем выбрать **Test -&gt;выполнения -&gt;все тесты в решении** меню команд (или типа Ctrl R A) для выполнения всех наших модульных тестов. Или в качестве альтернативы можно позиционировать наши курсор в методе класса или тестирования конкретного теста и использовать **Test -&gt;выполнения -&gt;тесты в текущем контексте** команду меню (или тип Ctrl R, T) для выполнения подмножества модульных тестов.

Давайте поместите нашей курсор в пределах класса DinnerTest и введите «Ctrl R, T» для выполнения двух тестов, который мы только что определили. Когда мы делаем это окне «Результаты теста» появится в среде Visual Studio, и мы рассмотрим результаты выполнения наших теста перечислены внутри его:

![](enable-automated-unit-testing/_static/image5.png)

*Примечание: В окне результатов теста VS не содержит столбца имя класса по умолчанию. Это можно добавить, щелкнув правой кнопкой мыши в окне результатов теста и с помощью команды меню Добавление и удаление столбцов.*

Наши тесты два заняло долю секунды для выполнения — и как можно оба они передаются см. Теперь можно перейти и расширить их путем создания другие тесты, которые можно проверить определенные правила проверки, а также рассматриваются два вспомогательных метода - IsUserHost() и IsUserRegisterd() —, мы добавили в класс Dinner. Наличие этих тестов для класса Dinner сделает намного легче и безопаснее добавить новые бизнес-правила, связанные с защитой к нему в будущем. Мы добавить новое правило логику на обед и затем за несколько секунд убедиться, что он еще не нарушают любой из наших прежние функции логики.

Обратите внимание на то, как с помощью имени описательные теста позволяет легко понять, что проверка каждого теста. Рекомендуется использовать **Сервис -&gt;параметры** команды меню, открыв тестирования Сервис -&gt;экран настройки выполнения тестов и проверки» дважды щелкните результат теста модульных отказавших или неопределенным результатом отображается флажок точки сбоя в тесте». Это позволит вам дважды щелкнуть при сбое в окне результатов теста и мгновенного перехода к предположение сбоя.

### <a name="creating-dinnerscontroller-unit-tests"></a>Создание DinnersController модульных тестов

Теперь создадим несколько модульных тестов, проверьте нашей DinnersController функциональность. Мы запустить, щелкнув папку «Контроллеры» в нашем тестовый проект и затем выберите **Add -&gt;новый тест** команду меню. Мы Создание «Модульного теста» и присвойте ей имя «DinnersControllerTest.cs».

Мы создадим двух методах теста, метод действия Details() на DinnersController проверки. Первый убедитесь, что представление возвращается при запросе существующие компании Dinner. Второй проверит, представление «NotFound» возвращается при запросе Dinner несуществующий:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

Приведенный выше код компилирует очистки. При выполнении тестов, однако их сбоев:

![](enable-automated-unit-testing/_static/image6.png)

Если взглянуть на сообщения об ошибках, мы рассмотрим, что причина не удалось выполнить тесты не поскольку наш класс DinnersRepository не удалось подключиться к базе данных. Обновление NerdDinner приложение использует строку подключения в локальный файл SQL Server Express, которая работает под \App\_каталог данных обновление NerdDinner проекта приложения. Поскольку наш NerdDinner.Tests проект компилируется и выполняется в другом каталоге, выберите проект приложения, неверное расположение относительный путь нашу строку подключения.

Мы *удалось* устранить эту проблему, скопировав файл базы данных SQL Express нашей тестовый проект, а затем добавьте соответствующий тест строки подключения к ней в файле App.config нашей тестового проекта. Это будет получена вышеуказанные тесты, разблокирован и запущена.

Модульное тестирование кода, с помощью реальной базы данных, однако, предлагает ряд проблем. В частности:

- Она значительно замедляет времени выполнения модульных тестов. Чем дольше необходимое для выполнения тестов, тем меньше вероятность будут часто выполнять их. В идеальном случае требуется модульные тесты, чтобы иметь возможность выполнения в секундах — и должны быть сделать как естественным образом, что компиляция проекта.
- Он усложняет логику установки и очистки в тесты. Требуется, чтобы каждый модульный тест для изолированной и независимым от остальных (с побочных эффектов или зависимостей). При работе в реальной базе данных необходимо учитывать состояние и выполнить его сброс между тестами.

Давайте взглянем на шаблон, который называется «инъекция зависимостей», которые могут помочь нам решить эти проблемы и избежать необходимости использования реальной базы данных с помощью наших тестов.

### <a name="dependency-injection"></a>Внедрение зависимостей

В данный момент DinnersController тесно» связана» в класс DinnerRepository. «Соединение» относится к ситуации, когда класс явным образом зависит от другого класса для работы:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

Поскольку класс DinnerRepository требуется доступ к базе данных, тесно связанные зависимости DinnersController класс имеет на концах DinnerRepository копии требует нам действующим DinnersController методы действий для тестирования базы данных.

Можно обойти это, создав шаблон, который называется «внедрения зависимостей» — это подход, где больше не неявного создания зависимости (например, репозиторий классы, обеспечивающие доступ к данным) в классах, которые их используют. Вместо этого зависимости можно явно передать класс, который использует их с помощью аргументов конструктора. Если зависимости определяются с помощью интерфейсов, мы получаем гибкость для передачи в реализациях «фиктивное» зависимостей для сценариев модульного тестирования. Это позволяет создать реализации зависимостей конкретного теста, которые фактически не требуется доступ к базе данных.

Чтобы увидеть это в действии, давайте реализовать внедрение зависимостей в нашем DinnersController.

#### <a name="extracting-an-idinnerrepository-interface"></a>Извлечение интерфейса IDinnerRepository

Наш первым шагом является создание новый интерфейс IDinnerRepository, инкапсулирующий нашей контроллеров, необходимых для извлечения и обновления ужинов контракт репозитория.

Этот интерфейс контракта можно определить вручную, щелкнув правой кнопкой мыши по папке \Models и выбрав **Add -&gt;новый элемент** команды меню и создать новый интерфейс с именем IDinnerRepository.cs.

Также мы извлечения рефакторинга средства встроены в Visual Studio Professional (и более поздних версий), чтобы автоматически использовать и создать интерфейс для нас из наших существующий класс DinnerRepository. Чтобы извлечь этот интерфейс, с помощью VS, просто поместите курсор в редакторе текста в классе DinnerRepository и затем щелкните правой кнопкой мыши и выберите **рефакторинга -&gt;извлечение интерфейса** команды меню:

![](enable-automated-unit-testing/_static/image7.png)

Будет запуска диалогового окна «Извлечение интерфейса» и нам запрашивать имя интерфейса, для создания. Он будет по умолчанию IDinnerRepository и автоматически выбрать все открытые методы в классе DinnerRepository Добавление интерфейса:

![](enable-automated-unit-testing/_static/image8.png)

При нажатии кнопки «ОК», Visual Studio добавит новый интерфейс IDinnerRepository наше приложение:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

И наши существующий класс DinnerRepository будет обновлен, чтобы он реализовал интерфейс:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>Обновление DinnersController для поддержки внедрение конструктора

Теперь мы сообщим DinnersController классом, который используется новый интерфейс.

В настоящее время DinnersController является жестко таким образом, что его поля «dinnerRepository» всегда является классом DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

Мы изменим ее так, чтобы в поле «dinnerRepository» IDinnerRepository вместо DinnerRepository типа. Затем мы добавим два открытых конструкторов DinnersController. Один из конструкторов позволяет IDinnerRepository передается как аргумент. Второе — конструктор по умолчанию, который использует реализацию существующего DinnerRepository:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

Так как ASP.NET MVC по умолчанию создает классы контроллера с помощью конструкторы по умолчанию, наши DinnersController во время выполнения будет продолжать класс DinnerRepository для осуществления доступа к данным.

Теперь мы можем обновить нашей модульные тесты, однако для передачи в реализации репозитория «фиктивное» компании dinner, с помощью параметра конструктора. Этот репозиторий «фиктивное» компании dinner не требуется доступ к реальной базы данных и вместо этого будет использовать данные в памяти.

#### <a name="creating-the-fakedinnerrepository-class"></a>Создание класса FakeDinnerRepository

Давайте создадим класс FakeDinnerRepository.

Мы начнем с создания каталога «Fakes» в данном проекте NerdDinner.Tests и затем добавить в нее новый класс FakeDinnerRepository (щелкните правой кнопкой мыши папку и выберите **Add -&gt;новый класс**):

![](enable-automated-unit-testing/_static/image9.png)

Код будет обновлена, чтобы класс FakeDinnerRepository реализует интерфейс IDinnerRepository. Мы затем правой кнопкой мыши и выберите из контекстного меню команду «Реализуйте интерфейс IDinnerRepository»:

![](enable-automated-unit-testing/_static/image10.png)

Это вызовет автоматическое добавление членов интерфейса IDinnerRepository в нашем FakeDinnerRepository класса реализации «заглушки» по умолчанию в Visual Studio:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

Затем можно обновить реализацию FakeDinnerRepository для работы из списка в памяти&lt;Dinner&gt; коллекции, переданного в качестве аргумента конструктора:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

Теперь у нас есть фиктивное реализацию IDinnerRepository, которая необходима база данных, а вместо этого может работать из списка в памяти объектов компании Dinner.

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>FakeDinnerRepository с помощью модульных тестов

Давайте вернемся к DinnersController модульные тесты, которые ранее сбой базы данных был недоступен. Можно изменить методы теста для использования FakeDinnerRepository, заполнены примерами данных компании Dinner в памяти для DinnersController, используя приведенный ниже код:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

И теперь при выполнении этих тестов они оба передачи:

![](enable-automated-unit-testing/_static/image11.png)

Лучше всего они принимают только доля секунды для выполнения и не требуют логики сложной настройки или очистки. Мы можем теперь модульного теста все наши код DinnersController действие метода (включая список, разбиение по страницам, сведения, создание, обновление и удаление) без необходимости никогда не подключаться к реальной базе данных.

| **Разделе стороны: Платформы внедрения зависимостей** |
| --- |
| Выполнение внедрения вручную зависимости (например, превышающие) работает отлично, но трудно поддерживать как общее число зависимостей, и повышает компонентов в приложении. Для .NET, которые помогают обеспечить еще большую гибкость управления зависимостей существует несколько инфраструктур внедрения зависимостей. Эти платформы также иногда называют «Инверсии элемента управления» (IoC) контейнеры, предоставляют механизмы, позволяющие дополнительный уровень поддержки конфигурации для указания и передаче объектов во время выполнения (чаще всего с помощью конструктора внедрения зависимости ). Некоторые из более широкое внедрение зависимостей OSS / include IOC платформы в .NET: AutoFac, Ninject, Spring.NET, StructureMap и Windsor. ASP.NET MVC предоставляет расширения API-интерфейсы, позволяющие разработчикам участвовать в решение и создание экземпляра контроллеры и позволяющий внедрения зависимостей и IoC платформы полностью интегрированы в рамках этого процесса. С помощью платформы DI и IOC также позволит нам для удаления из наших DinnersController — которого будет полностью удалить связь между ним и DinnerRepositorys конструктор по умолчанию. Мы не использовали внедрения зависимостей и IOC framework в приложении обновление NerdDinner. Но это то, что мы рассмотреть в будущем, если обновление NerdDinner базы кода и возможности роста. |

### <a name="creating-edit-action-unit-tests"></a>Создание модульных тестов для действий редактирования

Теперь создадим несколько модульных тестов, проверяющие DinnersController функциональные возможности редактирования. Мы начнем путем проверки версии HTTP-GET нашей действие изменения:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

Мы создадим тест, который проверяет, что визуализации представления, поддерживаемый объект DinnerFormViewModel обратно в том случае, когда запрашивается допустимым dinner:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

При выполнении теста, однако мы найдете, происходит сбой, поскольку при редактирования метода обращается к свойству User.Identity.Name выполнить проверку Dinner.IsHostedBy() исключение пустая ссылка.

Объект пользователя в базовом классе контроллера инкапсулирует сведения о вошедшего в систему пользователя и заполняется ASP.NET MVC, при создании контроллера во время выполнения. Так как мы тестируем DinnersController за пределами среды веб сервера, не задан объект пользователя (поэтому пустая ссылка на исключение).

### <a name="mocking-the-useridentityname-property"></a>Макетирование User.Identity.Name свойство

Платформы имитации упростить тестирование, позволяющих динамически создать фиктивный версии зависимых объектов, которые поддерживают наших тестов. Например мы используем платформа имитации в нашем тесте действие изменения для динамического создания объекта пользователя, наши DinnersController можно использовать для имитации username уточняющего запроса. Это позволит избежать пустая ссылка вызывается при запуске нашем тесте.

Существует много .NET платформы, которые могут использоваться с ASP.NET MVC имитации (вы увидите список их здесь: [http://www.mockframeworks.com/](http://www.mockframeworks.com/)). Для тестирования приложения обновление NerdDinner мы будем использовать с открытым исходным кодом имитации платформу, которая называется «Заказа», который можно загрузить бесплатно из [http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq).

После загрузки, мы добавим ссылку в данном проекте NerdDinner.Tests Moq.dll сборки:

![](enable-automated-unit-testing/_static/image12.png)

Затем мы добавим вспомогательный метод «CreateDinnersControllerAs(username)» к нашей тестового класса, который принимает в качестве параметра, а также что имя пользователя, а затем «фиктивных» свойство User.Identity.Name на экземпляре DinnersController:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

Выше мы используем заказа для создания макетов объектов, что fakes объект параметром ControllerContext (который передается с классами контроллера для предоставления объектов среды выполнения, как пользователь, запрос, ответ и сеанса ASP.NET MVC). Поступает вызов метода «SetupGet» на макетов для указания, что свойство HttpContext.User.Identity.Name параметром ControllerContext должна возвращать строку имени пользователя, которую мы передали вспомогательный метод.

Мы можете макета любое количество параметром ControllerContext свойства и методы. Чтобы проиллюстрировать это были также добавлены вызов SetupGet() для свойства Request.IsAuthenticated (которого не требуется фактически ниже — тестов, но которые помогают проиллюстрировать, как можно макета свойств запроса). После этого мы мы назначить экземпляр макетов параметром ControllerContext DinnersController, возвращает нашей вспомогательный метод.

Теперь можно написать модульные тесты, использующие этот вспомогательный метод для проверки редактирования сценариев с использованием разных пользователей:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

А теперь передачи при запуске тестов:

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>UpdateModel() сценариев тестирования

Мы создали тесты, которые покрывают версию HTTP-GET действие изменения. Теперь создадим некоторые тесты, проверьте версию HTTP POST действие изменения:

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

Интересные новые сценарии тестирования для поддержки с данным методом действия — его использование вспомогательного метода UpdateModel() в базовом классе контроллера. Мы используем этот вспомогательный метод для привязки значений формы post нашей компании Dinner экземпляр объекта.

Ниже приведены два теста, показано, как указать форму, передаваемую значения для вспомогательного метода UpdateModel() для использования. Мы будет это сделать, создание и заполнение объектов FormCollection, а затем назначить его свойству «ValueProvider» на контроллере.

Первый тест проверяет, что для успешного сохранения браузер перенаправляется на действие details. Второй тест проверяет, что при учете введено недопустимое действие повторно отображает представление изменения снова с сообщением об ошибке.


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>Тестирование Подведение итогов

Мы рассмотрели основные понятия, связанные с модульного тестирования классов контроллеров. Эти методы можно использовать для простого создания сотни простых тестов, которые проверяют поведение приложения.

Поскольку наш контроллера и тесты модели не требуется реальной базы данных, они являются очень быстро и легко выполняется. Мы будем возможность выполнения сотни автоматических тестов в секундах и сразу же получить обратную связь относительно ли что-нибудь нарушала мы внесенных изменений. Это поможет вам получить нам достоверности постоянно улучшить рефакторинга и уточнения нашего приложения.

Мы узнали тестирование в последнем разделе этой главы — но не так, как тестирование стоит необходимо делать в конце процесса разработки! Напротив необходимо создавать автоматические тесты как можно раньше в процессе разработки. Таким образом это позволяет получить оперативные сведения при разработке, помогает продуманным подумать о сценариев использования приложения и помогает разрабатывать приложение с чистой иерархическое представление и взаимозависимости в виду.

Главы в книге обсудим теста управляемую разработки (TDD) и способ его использования с ASP.NET MVC. Управляемой тестами разработки пишется итеративный типизации сначала тесты, которые удовлетворяют полученный код. С помощью управляемой тестами разработки каждого компонента, начните с создания тест, который проверяет функциональные возможности, которые вы собираетесь реализовать. Запись модульного теста сначала гарантирует четко понимать, компонент и как он должен работать. Только в том случае, когда записывается теста (и вы проверили, что он возвращает ошибку) вы, а затем реализовать фактическое функциональные возможности, которые можно проверить. Так как уже Потратив время обдумывания как компонент может работать в случае использования, необходимо будет лучше понять требования и как оптимальным способом для их реализации. Закончив с реализацией, можно повторно выполнить тест — и немедленный результат в виде ли компонент работает правильно. Мы обсудим TDD более в главе 10.

### <a name="next-step"></a>Следующий шаг

Некоторые последний переноса комментариев.

>[!div class="step-by-step"]
[Назад](use-ajax-to-implement-mapping-scenarios.md)
[Вперед](nerddinner-wrap-up.md)
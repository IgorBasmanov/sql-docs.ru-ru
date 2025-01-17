---
title: Определение свойств неизвестного элемента и обработки значений NULL | Документация Майкрософт
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: analysis-services
ms.topic: conceptual
ms.assetid: d9abb09c-9bfa-4e32-b530-8590e4383566
author: minewiskan
ms.author: owend
manager: craigg
ms.openlocfilehash: d0d97b7fea9557e1ce462fcc540e51a1ee4b0228
ms.sourcegitcommit: f5807ced6df55dfa78ccf402217551a7a3b44764
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/15/2019
ms.locfileid: "69493918"
---
# <a name="defining-the-unknown-member-and-null-processing-properties"></a>Определение свойств Unknown Member и Null Processing
  В процессе обработки измерения службами [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] его атрибуты заполняются всеми уникальными значениями, полученными из базовых столбцов представлений и таблиц в представлении источника данных. Если при обработке службы [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] обнаруживают значение NULL, по умолчанию оно преобразуется в нулевое значение для числовых столбцов или в пустую строку — для строковых. Можно изменить значения по умолчанию или преобразовывать значения NULL в процессе извлечения, преобразования или загрузки данных (если они выполняются) из базового реляционного хранилища данных. Кроме того, службы [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] можно настроить для преобразования значения NULL в указанное значение настройкой трех свойств: **UnknownMember** и **UnknownMemberName** для измерения и **NullProcessing** для ключевого атрибута измерения.  
  
 Мастер измерений и мастер кубов включают эти свойства в том случае, если ключевой атрибут измерения допускает значения NULL или корневой атрибут измерения, связанного по схеме «снежинка», основан на столбце, который допускает значения NULL. В этих случаях свойству **NullProcessing** ключевого атрибута будет присвоено значение **UnknownMember** , а свойству **UnknownMember** — значение **Visible**.  
  
 Однако при добавочной сборке измерений, связанных по схеме "снежинка" (как в измерении Product на занятиях учебника), или при определении измерений с помощью конструктора измерений и их последующей интеграции в куб может потребоваться ручная установка свойств **UnknownMember** и **NullProcessing** .  
  
 В задачах этого раздела будут добавлены атрибуты категории и подкатегории товара в измерение Product из таблиц, связанных по схеме «снежинка», которые, в свою очередь, будут добавлены в представление источника данных [!INCLUDE[ssSampleDBCoShort](../includes/sssampledbcoshort-md.md)] DW. Затем включите свойство **UnknownMember** для измерения `Assembly Components` Product, укажите в качестве значения свойства `Subcategory` UnknownMemberName, свяжите атрибуты и `Category` с атрибутом Product Name. Затем определите пользовательскую обработку ошибок для ключевого атрибута элемента, связывающего таблицы по схеме «снежинка».  
  
> [!NOTE]  
>  Если изначально куб [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] Tutorial был определен с помощью мастера кубов, при добавлении атрибутов Subcategory и Category эти шаги будут выполнены автоматически.  
  
## <a name="reviewing-error-handling-and-unknown-member-properties-in-the-product-dimension"></a>Просмотр свойств обработки ошибок и неизвестного элемента в измерении Product  
  
1.  Переключитесь в конструкторе измерений на измерение **Product** , перейдите на вкладку **Структура измерения** и выберите на панели **Атрибуты** элемент **Product** .  
  
     Теперь можно просматривать и изменять свойства самого измерения.  
  
2.  В окне "Свойства" просмотрите свойства **UnknownMember** и **UnknownMemberName** .  
  
     Обратите внимание, что свойство **UnknownMember** отключено, так как для него указано значение **Нет** вместо **Видимый** или **Скрытый**, и что для свойства **UnknownMemberName** имя не задано.  
  
3.  В окне свойств в ячейке свойств **ErrorConfiguration** выберите **(пользовательский)** и раскройте коллекцию свойств **ErrorConfiguration** .  
  
     Выбор значения **(пользовательский)** для свойства **ErrorConfiguration** позволяет просмотреть используемые по умолчанию настройки конфигурации обработки ошибок; настройки при этом не изменяются.  
  
4.  Просмотрите свойства конфигурации ошибок ключа и ошибок ключа NULL, однако не вносите изменения.  
  
     Обратите внимание, что по умолчанию при преобразовании ключа NULL в неизвестный элемент ошибка обработки, связанная с этим преобразованием, пропускается.  
  
     На рисунке ниже показаны параметры свойств в коллекции свойств **ErrorConfiguration** .  
  
     ![Коллекция свойств ErrorConfiguration](../../2014/tutorials/media/l4-productdimensionerrorconfig-1.gif "Коллекция свойств ErrorConfiguration")  
  
5.  Перейдите на вкладку **браузер** , убедитесь в том, что в списке Иерархия выбран пункт **линии модели продукта** , `All Products`а затем разверните.  
  
     Обратите внимание на пять элементов уровня Product Line.  
  
6.  Разверните узел **Components**, а затем разверните немаркированный элемент уровня **Model Name** .  
  
     Этот уровень содержит компоненты сборки, используемые при построении других компонентов, начиная с продукта **Adjustable Race** , как показано на рисунке ниже.  
  
     ![Компоненты сборки, используемые для создания других компонентов](../../2014/tutorials/media/l4-productdimensionerrorconfig-2.gif "Компоненты сборки, используемые для создания других компонентов")  
  
## <a name="defining-attributes-from-snowflaked-tables-and-a-product-category-user-defined-hierarchy"></a>Определение атрибутов из связанных по схеме «снежинка» таблиц и пользовательской иерархии Product Category  
  
1.  Откройте в конструкторе представлений источников данных представление источника данных [!INCLUDE[ssSampleDBCoShort](../includes/sssampledbcoshort-md.md)] DW, выберите **Reseller Sales** на панели **Организатор диаграмм** , а затем выберите команду **Добавить или удалить объекты** в меню **Представление источников данных** среды [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)].  
  
     Откроется диалоговое окно **Добавление или удаление таблиц** .  
  
2.  В списке **Включенные объекты** выберите **DimProduct (dbo)** , а затем нажмите кнопку **Добавить связанные таблицы**.  
  
     Будут добавлены объекты **DimProductSubcategory (dbo)** и **FactProductInventory (dbo)** . Удалите **FactProductInventory (dbo)** , чтобы в список **Включенные объекты** была добавлена только таблица **DimProductSubcategory (dbo)** .  
  
3.  Повторно нажмите кнопку **Добавить связанные таблицы** при выбранной по умолчанию (как последняя добавленная) таблице **DimProductSubcategory (dbo)** .  
  
     Таблица **DimProductCategory (dbo)** будет добавлена в список **Включенные объекты** .  
  
4.  Нажмите кнопку **ОК**.  
  
5.  В меню **Формат** среды [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]последовательно выберите команды **Автоматический макет**и **Диаграмма**.  
  
     Обратите внимание, что таблицы **DimProductSubcategory (dbo)** и **DimProductCategory (dbo)** связаны друг с другом, а также с таблицей **ResellerSales** через таблицу **Product** .  
  
6.  Перейдите в конструкторе измерений к измерению **Product** и откройте вкладку **Структура измерения** .  
  
7.  Щелкните правой кнопкой мыши панель **Представление источника данных** и выберите команду **Показать все таблицы**.  
  
8.  На панели **Представление источника данных** найдите таблицу **DimProductCategory** , щелкните в ней правой кнопкой мыши столбец **ProductCategoryKey** и выберите команду **Создать атрибут из столбца**.  
  
9. На панели **атрибуты** измените имя этого нового атрибута на `Category`.  
  
10. В окно свойств щелкните поле свойства **NameColumn** , а затем нажмите кнопку обзора ( **...** ), чтобы открыть диалоговое окно **Столбец имени** .  
  
11. В списке **Исходный столбец** выберите **EnglishProductCategoryName** и нажмите кнопку **ОК**.  
  
12. На панели **Представление источника данных** найдите таблицу **DimProductSubcategory** , щелкните в ней правой кнопкой мыши столбец **ProductSubcategoryKey** и выберите команду **Создать атрибут из столбца**.  
  
13. На панели **атрибуты** измените имя этого нового атрибута на `Subcategory`.  
  
14. В окно свойств щелкните поле свойства **NameColumn** , а затем нажмите кнопку обзора **(...)** , чтобы открыть диалоговое окно **Столбец имени** .  
  
15. В списке **Исходный столбец** выберите **EnglishProductSubcategoryName** и нажмите кнопку **ОК**.  
  
16. Создайте новую пользовательскую иерархию с названием **категории продуктов** со следующими уровнями в порядке сверху вниз: `Category`, `Subcategory`и **Название продукта**.  
  
17. Укажите `All Products` в качестве значения свойства **AllMemberName** пользовательской иерархии категории продуктов.  
  
## <a name="browsing-the-user-defined-hierarchies-in-the-product-dimension"></a>Просмотр пользовательских иерархий в измерении Product  
  
1.  На панели инструментов вкладки **Структура измерения** в **конструкторе измерений** для измерения **Product** нажмите кнопку **Обработка**.  
  
2.  Нажмите кнопку **Да** , чтобы собрать и развернуть проект, а затем кнопку **Выполнить** , чтобы выполнить обработку измерения **Product** .  
  
3.  По завершении обработки разверните узел **Обработка измерения "Product" завершена успешно** в диалоговом окне **Ход обработки** , разверните узел **Обработка атрибута измерения "Product Name" завершена**, а затем разверните узел **Запросы SQL 1**.  
  
4.  Щелкните запрос SELECT DISTINCT, а затем **Просмотр подробностей**.  
  
     Обратите внимание, что к предложению SELECT DISTINCT было добавлено предложение WHERE, удаляющее продукты, для которых не задано значение в столбце ProductSubcategoryKey, как показано на следующем рисунке.  
  
     ![SELECT DISTINCT предложение, содержащее предложение WHERE](../../2014/tutorials/media/l4-productnametraceline-1.gif "SELECT DISTINCT предложение, содержащее предложение WHERE")  
  
5.  Три раза нажмите кнопку **Закрыть** , чтобы закрыть все диалоговые окна.  
  
6.  Перейдите на вкладку **Браузер** конструктора измерений для измерения **Product** и нажмите кнопку **Повтор соединения**.  
  
7.  Убедитесь, что в `All Products`списке **Иерархия** отображаются **линии модели продукта** , а затем разверните узел **компоненты**.  
  
8.  В списке Иерархия выберите **категории продуктов** , разверните `All Products`, а затем — **компоненты**.  
  
     Обратите внимание, что не отображается ни один из компонентов сборки.  
  
 Чтобы изменить поведение, упомянутое в предыдущей задаче, необходимо включить свойство **UnknownMember** измерения Products, задать значение для свойства **UnknownMemberName** , задать свойство `Subcategory` **NullProcessing** для свойств и Атрибуты **имени модели** для **UnknownMember** `Category` , определение атрибута в `Subcategory` качестве связанного атрибута атрибута, а затем определение атрибута «линейка **продукта** » в качестве связанного атрибута **имени модели** версию. В результате выполнения этих действий службы [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] станут использовать значение имени неизвестного элемента для товаров, не имеющих значений в столбце **SubcategoryKey** , как будет показано в следующей задаче.  
  
## <a name="enabling-the-unknown-member-defining-attribute-relationships-and-specifying-custom-processing-properties-for-nulls"></a>Включение неизвестного элемента, определение связей атрибутов и указание свойств пользовательской обработки для значений NULL  
  
1.  В конструкторе измерений для измерения **Product** перейдите на вкладку **Структура измерения** , а затем на панели **Атрибуты** выберите атрибут **Product** .  
  
2.  В окне **Свойства** измените свойство **UnknownMember** на **Visible**, а затем измените значение свойства **UnknownMemberName** на `Assembly Components`.  
  
     Смена значения свойства **UnknownMember** на **Видимый** или **Скрытый** включит свойство **UnknownMember** для этого измерения.  
  
3.  Перейдите на вкладку **Связи атрибутов** .  
  
4.  На диаграмме щелкните правой кнопкой мыши `Subcategory` атрибут и выберите пункт **создать связь атрибутов**.  
  
5.  В диалоговом окне **Создание связи атрибутов** в поле **Исходный атрибут** имеет `Subcategory`значение. Задайте для `Category`параметра **связанный атрибут** значение. Оставьте для типа связи значение **Гибкая**.  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
7.  На панели **Атрибуты** выберите элемент **Subcategory**.  
  
8.  В окне "Свойства" разверните свойство **KeyColumns** , а затем свойство **DimProductSubcategory.ProductSubcategoryKey (Integer)** .  
  
9. Установите для свойства **NullProcessing** значение **UnknownMember**.  
  
10. На панели **Атрибуты** выберите элемент **Model Name**.  
  
11. В окне "Свойства" разверните свойство **KeyColumns** , а затем свойство **Product.ModelName (WChar)** .  
  
12. Установите для свойства **NullProcessing** значение **UnknownMember**.  
  
     Из-за этих изменений при [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] обнаружении значения NULL `Subcategory` для атрибута или атрибута **имени модели** во время обработки значение неизвестного элемента будет заменено значением ключа, а пользовательские иерархии будут сформирован правильно.  
  
## <a name="browsing-the-product-dimension-again"></a>Повторный просмотр измерения Product  
  
1.  В меню **Сборка** выберите команду **Развернуть Analysis Services Tutorial**.  
  
2.  После успешного развертывания перейдите на вкладку **Браузер** в конструкторе измерений для измерения **Product** и нажмите кнопку **Повтор соединения**.  
  
3.  Убедитесь, что в списке Иерархия выбраны **категории продуктов** , а затем `All Products`разверните.  
  
     Обратите внимание, что элемент Assembly Components отображается в качестве нового элемента на уровне категории.  
  
4.  `Assembly Components` Разверните элемент `Category` уровня,`Subcategory` а затем разверните элементуровня.`Assembly Components`  
  
     Обратите внимание, что все компоненты сборки отображаются на уровне **Product Name** , как показано на рисунке ниже.  
  
     ![Уровень имени продукта, иллюстрирующий компоненты сборки](../../2014/tutorials/media/l4-assemblycomponents-1.gif "Уровень имени продукта, иллюстрирующий компоненты сборки")  
  
## <a name="next-lesson"></a>Следующее занятие  
 [Занятие 5. Определение связей между измерениями и группами мер](lesson-5-defining-relationships-between-dimensions-and-measure-groups.md)  
  
  

---
title: Тип соединения служб Analysis Services для многомерных выражений (службы SSRS) | Документы Майкрософт
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: bd2e7148-3124-4e07-9734-22333127c3be
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c108b2eaaf8aa0182b8192ab2d7868db84d531a6
ms.sourcegitcommit: a1adc6906ccc0a57d187e1ce35ab7a7a951ebff8
ms.translationtype: MTE75
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68892487"
---
# <a name="analysis-services-connection-type-for-mdx-ssrs"></a>Тип соединения служб Analysis Services для многомерных выражений (службы SSRS)
  Чтобы включить в отчет данные из куба [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , необходимо иметь набор данных, основанный на источнике данных [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Этот встроенный тип источника данных основан на модуле обработки данных служб [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Из куба служб [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] можно получить метаданные измерений, иерархий, уровней, ключевых показателей эффективности, мер и атрибутов и использовать их в качестве данных отчета.  
  
 Этот модуль обработки данных поддерживает многозначные параметры, серверные агрегатные вычисления и учетные данные, управляемые независимо с помощью строки подключения.  
  
 Используйте сведения в этом разделе для создания источника данных. Пошаговые инструкции см. в разделе [Добавление и проверка подключения к данным (построитель отчетов и службы SSRS)](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md).  
  
##  <a name="Connection"></a> Строка подключения  
 При соединении с кубом служб [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] выполняется соединение с объектом базы данных в экземпляре служб Analysis Services на сервере. База данных может содержать несколько кубов. При построении запроса необходимо указать куб в конструкторе запросов. В следующем примере приведена строка соединения:  
  
```  
data source=<server name>;initial catalog=<database name>  
```  
  
 Дополнительные примеры строк соединения см. в разделе [Подключения к данным, источники данных и строки подключения в построителе отчетов](data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md).  
  
  
##  <a name="Credentials"></a> Учетные данные  
 Учетные данные необходимы для запуска запросов, локального предварительного просмотра отчетов, а также для предварительного просмотра отчетов на сервере отчетов.  
  
 После публикации отчета может понадобиться изменить учетные данные источника данных, чтобы разрешения, необходимые для получения данных при запуске отчета на сервере отчетов, были допустимыми.  
  
 Клиент, используемый для создания отчетов, имеет следующие параметры для указания учетных данных.  
  
-   Текущий пользователь Windows (встроенная безопасность).  
  
-   Использовать сохраненные имя пользователя и пароль.  
  
-   Выдавать приглашение пользователю на ввод учетных данных. Этот параметр поддерживает только схему встроенной безопасности Windows.  
  
-   Учетные данные не требуются. Чтобы использовать этот параметр, необходима учетная запись автоматического выполнения, настроенная на сервере отчетов. Дополнительные сведения см. в разделе [Настройка учетной записи автоматического выполнения (диспетчер конфигурации служб SSRS)](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md) в [документации по службам Reporting Services](https://go.microsoft.com/fwlink/?linkid=121312) на сайте msdn.microsoft.com.  
  
 Дополнительные сведения см. в разделе [подключения к данным, источники данных и строки &#40;подключения построитель отчетов и&#41; службы SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) , а также [Указание учетных данных и сведений о соединении для источников данных отчета](specify-credential-and-connection-information-for-report-data-sources.md).  
  
  
##  <a name="Query"></a> Запросы  
 После подключения к источнику данных служб [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] создается набор данных и определяется запрос многомерных выражений, указывающий данные для получения из куба. С помощью графического конструктора запросов многомерных выражений можно просматривать и выбирать базовые структуры данных в источнике данных.  
  
 Запрос можно задавать следующими способами.  
  
-   Интерактивное построение отчета. Конструктор запросов многомерных выражений в службах Analysis Services поддерживает следующие представления.  
  
    -   **Представление конструктора.** Перетащите измерения, элементы, свойства элементов, меры и ключевые показатели эффективности из браузера метаданных на панель **Данные** для создания запроса многомерных выражений. Перетащите вычисляемые элементы с панели "Вычисляемые элементы" на панель "Данные", чтобы определить дополнительные поля наборов данных.  
  
    -   **Представление запроса.** Перетащите измерения, элементы, свойства элементов, меры и ключевые показатели эффективности из браузера метаданных на панель «Запрос» для создания запроса многомерных выражений. Текст многомерного выражения можно изменять непосредственно на панели запроса. Перетащите вычисляемые элементы из панели "Вычисляемые элементы" в панель "Запрос" для определения дополнительных полей наборов данных.  
  
     Дополнительные сведения см. в статье [Пользовательский интерфейс конструктора запросов многомерных выражений служб Analysis Services (построитель отчетов)](https://msdn.microsoft.com/library/7e288eee-2d37-485e-a6a0-dbba5e041e26).  
  
-   Импорт существующего запроса многомерных выражений из отчета. Воспользуйтесь кнопкой **Импорт** , чтобы указать RDL-файл и импортировать запрос. Можно импортировать запрос из отчета, содержащего внедренный набор данных, основанный на источнике данных служб [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Импорт запроса многомерных выражений непосредственно из MDX-файла не поддерживается.  
  
 Во время разработки выполните запрос, чтобы просмотреть результирующий набор. Результаты запроса возвращаются автоматически в виде плоского набора строк. Столбцы результирующего набора запроса заполняют коллекцию полей набора данных. После создания запроса просмотрите коллекцию полей набора данных, созданную из метаданных в области данных отчета. При запуске отчета фактические данные возвращаются из внешнего источника данных.  
  
 Модуль обработки данных служб [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] поддерживает расширенные свойства полей набора данных. Эти значения доступны из внешнего источника данных, но они не отображаются в области данных отчета. В отчете с помощью встроенной коллекции [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Fields **можно использовать расширенные свойства полей, поддерживаемые модулем обработки данных служб** . Для свойств, значения которых содержатся в источнике данных, можно обращаться к значениям таких стандартных свойств, как **FormattedValue**, **Color**или **UniqueName**. Дополнительные сведения см. в разделе [Расширенные свойства поля для базы данных служб Analysis Services &#40;службы SSRS&#41;](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md).  
  
  
##  <a name="Parameters"></a> Параметры  
 Чтобы включить параметры запроса, необходимо создать фильтр в области фильтра конструктора запросов и пометить фильтр как параметр. Для каждого фильтра будет автоматически создан набор данных, предоставляющий доступные значения. По умолчанию эти наборы данных не отображаются в области данных отчета. Дополнительные сведения см. в разделе [Определение параметров в конструкторе запросов многомерных выражений для служб Analysis Services (построитель отчетов)](../../reporting-services/report-data/define-parameters-in-the-mdx-query-designer-for-analysis-services.md) и [Отображение скрытых наборов данных для значений параметра в многомерных данных (построитель отчетов и службы SSRS)](../../reporting-services/report-data/show-hidden-datasets-for-parameter-values-multidimensional-data.md).  
  
 По умолчанию каждый параметр отчета имеет тип данных **Текст**. После создания параметров отчета можно изменить значения по умолчанию. Дополнительные сведения см. в разделе [Параметры отчета (построитель отчетов и конструктор отчетов)](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md).  
  
  
##  <a name="Remarks"></a> Замечания  
 Модуль обработки данных служб Analysis Services работает на основе протокола XMLA (XML для аналитики). Результирующие наборы из кубов извлекаются через протокол XMLA в виде плоского набора строк. Неоднородные иерархии не поддерживаются. Дополнительные сведения об иерархиях см. в разделе [Неоднородные иерархии](https://docs.microsoft.com/analysis-services/multidimensional-models/user-defined-hierarchies-ragged-hierarchies).  
  
 Данные из куба служб [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] можно также получать с помощью источника данных OLE DB. Дополнительные сведения см. в разделе [Тип соединения OLE DB (службы SSRS)](../../reporting-services/report-data/ole-db-connection-type-ssrs.md).  
  
 Дополнительные сведения о поддержке версий см. в разделе [Источники данных, поддерживаемые службами Reporting Services ( службы SSRS)](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md) документации по [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в [электронной документации](https://go.microsoft.com/fwlink/?linkid=121312) по [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
  
##  <a name="Related"></a> См. также  
 В этих разделах документации содержатся подробные сведения о данных отчетов, а также методические сведения об определении, настройке и использовании элементов отчетов, связанных с данными.  
  
 [Наборы данных отчетов (службы SSRS)](../../reporting-services/report-data/report-datasets-ssrs.md)  
 Предоставляет общие сведения о доступе к данным отчета.  
  
 [Подключения к данным, источники данных и строки подключения в построителе отчетов](data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)  
 Предоставляет сведения о подключениях к данным и источникам данных.  
  
 [Внедренные и общие наборы данных отчета (построитель отчетов и службы SSRS)](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
 Предоставляет сведения об общих и внедренных наборах данных.  
  
 [Коллекция полей набора данных (построитель отчетов и службы SSRS)](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
 Предоставляет сведения о коллекции полей набора данных, создаваемой запросом.  
  
 [Расширенные свойства поля для базы данных служб Analysis Services (службы SSRS)](../../reporting-services/report-data/extended-field-properties-for-an-analysis-services-database-ssrs.md)  
 Предоставляет сведения о расширенных полях, доступных через поставщик данных XMLA.  
  
 [Источники данных, поддерживаемые службами Reporting Services &#40;SSRS&#41;](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md), см. в документации [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в [электронной документации](https://go.microsoft.com/fwlink/?linkid=121312) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
 Предоставляет подробные сведения о поддержке платформ и версий для каждого модуля обработки данных.  
  
  
## <a name="see-also"></a>См. также:  
 [Параметры отчета (построитель отчетов и конструктор отчетов)](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [Фильтрация, группирование и сортировка данных (построитель отчетов и службы SSRS)](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [Выражения (построитель отчетов и службы SSRS)](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
  

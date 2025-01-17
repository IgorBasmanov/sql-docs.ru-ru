---
title: Как хранилище запросов собирает данные | Документация Майкрософт
ms.custom: ''
ms.date: 11/29/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Query Store, data collection
ms.assetid: 8d5eec36-0013-480a-9c11-183e162e4c8e
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 83fbc6c183216cedcbb664a0c3a2e3a9337e1513
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67946772"
---
# <a name="how-query-store-collects-data"></a>Сбор данных в хранилище запросов
[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

  Хранилище запросов работает подобно **бортовому регистратору данных в самолете** , непрерывно собирая сведения о компиляции и времени выполнения, связанные с запросами и планами. Связанные с запросами данные сохраняются во внутренних таблицах и предоставляются пользователям с помощью набора представлений.  
  
## <a name="views"></a>Представления  
 На схеме ниже показаны представления хранилища запросов и их логические связи. Сведения о времени компиляции показаны в виде элементов синего цвета.  
  
 ![query-store-process-2views](../../relational-databases/performance/media/query-store-process-2views.png "query-store-process-2views")  
**Описания представлений**  
  
|Представление|Описание|  
|----------|-----------------|  
|**sys.query_store_query_text**|Показывает тексты уникальных запросов к базе данных. Комментарии и пробелы до и после текста запроса игнорируются. Комментарии и пробелы внутри текста учитываются. Каждая инструкция в пакете создает текстовую запись отдельного запроса.|  
|**sys.query_context_settings**|Показывает уникальные комбинации влияющих на план параметров, при которых выполняются запросы. Тот же текст запроса, выполненного с другими влияющими на план параметрами, создает запись отдельного запроса в хранилище запросов, так как элемент `context_settings_id` является частью ключа запроса.|  
|**sys.query_store_query**|Записи запросов, которые отслеживаются и принудительно выполняются отдельно в хранилище запросов. Текст одного запроса может создать несколько записей, если он выполняется при других контекстных параметрах или вне одних модулей [!INCLUDE[tsql](../../includes/tsql-md.md)] и внутри других (хранимые процедуры, триггеры и т. д.).|  
|**sys.query_store_plan**|Показывает предполагаемый план запроса со статистикой времени компиляции. Хранимый план эквивалентен плану, получаемому при использовании `SET SHOWPLAN_XML ON`.|  
|**sys.query_store_runtime_stats_interval**|Хранилище запросов делит время на автоматически создаваемые интервалы времени и сохраняет сводные статистические данные в таких интервалах для каждого выполненного плана. Размер интервала регулируется параметром конфигурации "Интервал сбора статистики" (в [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]) или параметром `INTERVAL_LENGTH_MINUTES` с помощью [параметров ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).|  
|**sys.query_store_runtime_stats**|Сводная статистика времени выполнения для выполненных планов. Все собранные показатели выражаются в виде четырех статистических функций: Average (среднее), Maximum (максимум), Minimum (минимум), Standard Deviation (стандартное отклонение).|  
  
 Дополнительные сведения о представлениях хранилища запросов см. в разделе **Связанные представления, функции и процедуры** статьи [Мониторинг производительности с использованием хранилища запросов](monitoring-performance-by-using-the-query-store.md).  
  
## <a name="query-processing"></a>Обработка запросов  
 Хранилище запросов взаимодействует с конвейером обработки запросов в следующих ключевых моментах:  
  
1.  При первой компиляции запросов текст запроса и исходный план отправляются в хранилище запросов.  
  
2.  При перекомпиляции запроса план обновляется в хранилище запросов. Если создается новый план, хранилище запросов добавляет новую запись плана для запроса, сохраняя предыдущие записи вместе со статистикой их выполнения.  
  
3.  При выполнении запроса статистика времени выполнения отправляется в хранилище запросов. Хранилище запросов поддерживает точность сводной статистики для каждого плана, выполненного в течение активного на данный момент интервала.  
  
4.  Во время этапов компиляции и проверки на перекомпиляцию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] определяет, есть ли в хранилище запросов план, который должен применяться к выполняемому в данный момент запросу. Если существует принудительный план и план в кэше процедур отличается от принудительного плана, то выполняется перекомпиляция запроса именно так, как если бы указание PLAN HINT было применено к этому запросу. Этот процесс происходит прозрачно для пользовательского приложения.  
  
 На следующей схеме показаны точки интеграции, описанные выше:  
  
 ![query-store-process-2processor](../../relational-databases/performance/media/query-store-process-2processor.png "query-store-process-2processor")  
  
 Чтобы свести к минимуму издержки ввода-вывода, новые данные записываются в память. Операции записи ставятся в очередь и записываются на диск позже. Сведения о запросах и планах (на схеме ниже это Plan Store) записываются на диск с минимальной задержкой. Статистика времени выполнения (Runtime Stats) хранится в памяти в течение времени, заданного параметром `DATA_FLUSH_INTERVAL_SECONDS` инструкции `SET QUERY_STORE` . Диалоговое окно хранилища запросов SSMS позволяет ввести **Интервал записи данных на диск (в минутах)** , которое преобразуется в секунды.  
  
 ![query-store-process-3plan](../../relational-databases/performance/media/query-store-process-3.png "query-store-process-3plan")  
  
 Если происходит сбой системы, хранилище запросов может потерять данные среды выполнения в объеме, определенном параметром `DATA_FLUSH_INTERVAL_SECONDS`. Значение по умолчанию — 900 секунд (15 минут) — представляет собой оптимальное соотношение между эффективностью отслеживания запросов и доступностью данных.  
Если система испытывает недостаток памяти, статистика времени выполнения может быть сохранена на диске до наступления времени, определенного параметром `DATA_FLUSH_INTERVAL_SECONDS`.  
Во время чтения данных хранилища запросов данные из памяти и с диска прозрачно объединяются.
Если сеанс прерывается или клиентское приложение перезапускается или аварийно завершает работу, статистика запросов не будет записана.  
  
 ![query-store-process-4planinfo](../../relational-databases/performance/media/query-store-process-4planinfo.png "query-store-process-4planinfo")    

  
## <a name="see-also"></a>См. также:  
 [Мониторинг производительности с использованием хранилища запросов](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Рекомендации по хранилищу запросов](../../relational-databases/performance/best-practice-with-the-query-store.md)   
 [Представления каталога хранилища запросов (Transact-SQL)](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)  
  
  

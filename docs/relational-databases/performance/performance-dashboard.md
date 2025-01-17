---
title: Панель мониторинга производительности | Документация Майкрософт
ms.custom: ''
ms.date: 01/10/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- performance dashboard [SQL Server]
- performance dashboard reports
- perf dashboard
ms.assetid: 07f8f594-75b4-4591-8c29-d63811d7753e
author: pelopes
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 4b5e2a9dbb6963b63d6dfac8d76d9c01e839a8fe
ms.sourcegitcommit: 869d4de6c807a37873b66e5479d2c5ceff9efb85
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67559478"
---
# <a name="performance-dashboard"></a>Панель мониторинга производительности
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

В [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] версии 17.2 и более поздних есть панель мониторинга производительности. Она позволяет быстро получать наглядное представление о состоянии производительности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с версии [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]) и [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)]. 

Панель мониторинга производительности помогает быстро выявлять наличие узких мест производительности в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)]. Если узкое место существует, вы можете легко собрать дополнительные диагностические сведения, необходимые для устранения проблемы. Вот некоторые распространенные проблемы производительности, которые можно выявлять с помощью панели мониторинга производительности:
-  Узкие места ЦП (и какие запросы занимают больше всего ресурсов процессора)
-  узкие места ввода-вывода (и какие запросы выполняют больше всего операций ввода-вывода);
-  рекомендации по индексам, сформированные оптимизатором запросов (отсутствующие индексы);
-  Блокировка
-  состязание за ресурсы (включая состязание кратковременной блокировки).

Панель мониторинга производительности также помогает выявлять ресурсоемкие запросы, которые могли выполняться раньше. Для определения уровня затрат доступно несколько метрик: ЦП, число логических операций записи, число логических операций чтения, продолжительность, число физических операций чтения и время CLR.

Панель мониторинга производительности состоит из следующих разделов и вложенных отчетов:
-  Загрузка ЦП системы
-  Текущие ожидающие запросы
-  Текущее действие
   -  Запросы пользователей
   -  Сеансы пользователей
   -  Коэффициент попадания в кэш
-  Исторические сведения
   -  Случаи ожидания
   -  Кратковременные блокировки
   -  Статистика ввода-вывода
   -  Ресурсоемкие запросы
- Прочие сведения
  -  Активные трассировки
  -  Активные сеансы xEvent
  -  Базы данных
  -  Отсутствующие индексы

> [!NOTE] 
> На внутреннем уровне панель мониторинга производительности использует динамические административные представления и функции динамического управления, связанные с [выполнением](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md), [индексом](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md) и [вводом-выводом](../../relational-databases/system-dynamic-management-views/i-o-related-dynamic-management-views-and-functions-transact-sql.md).

## <a name="to-view-the-performance-dashboard"></a>Открытие панели мониторинга производительности 
  
Чтобы открыть панель мониторинга производительности, щелкните правой кнопкой мыши имя экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в обозревателе объектов и последовательно выберите пункты **Отчеты**, **Стандартные отчеты** и **Панель мониторинга производительности**.  
  
![Пункт "Панель мониторинга производительности" в меню](../../relational-databases/performance/media/perf_dashboard_ssms.png "Пункт \"Панель мониторинга производительности\" в меню")  
  
Панель мониторинга производительности откроется на новой вкладке. Ниже представлен пример очевидного узкого места ЦП.  
  
![Главный экран панели мониторинга производительности](../../relational-databases/performance/media/perf_dashboard.png "Главный экран панели мониторинга производительности")  
  
## <a name="remarks"></a>Remarks
В отчете **Отсутствующие индексы** приводятся потенциально отсутствующие индексы, которые оптимизатор запросов обнаружил во время компиляции запросов. Однако этим рекомендациям не нужно следовать в обязательном порядке. Для создания корпорация Майкрософт рекомендует оценивать только индексы с оценкой выше 100 000, так как у них самые высокие ожидаемые улучшения в отношении запросов пользователей. 

> [!TIP]
> Всегда следует учитывать, сравним ли новый индекс с существующим в той же таблице и можно ли достичь тех же практических результатов путем изменения существующего индекса, а не создания нового. Например, если предлагается новый индекс для столбцов C1, C2 и C3, сначала оцените, существует ли индекс в столбцах C1 и C2. Если да, может быть предпочтительнее просто добавить столбец C3 в существующий индекс (сохраняя порядок существующих столбцов), чтобы не создавать новый.
> Дополнительные сведения см. в [руководстве по архитектуре и разработке индексов](../../relational-databases/sql-server-index-design-guide.md).

В отчете **Ожидание** приводятся ожидания в режиме простоя или спящем режиме. Дополнительные сведения об ожидании см. в статье [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) и документе [Настройка производительности SQL Server 2005 с помощью ожиданий и очередей](http://download.microsoft.com/download/4/7/a/47a548b9-249e-484c-abd7-29f31282b04d/performance_tuning_waits_queues.doc).

Отчеты **Ресурсоемкие запросы** сбрасываются при перезапуске [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], так как данные в базовых динамических административных представлениях очищаются. Начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] подробные сведения о ресурсоемких запросах можно найти в хранилище запросов. 

> [!NOTE]
> Панель мониторинга производительности была впервые выпущена в качестве отдельного скачиваемого компонента для [SQL Server 2005](http://www.microsoft.com/download/details.aspx?id=22602), а позже была обновлена для [SQL Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=29063).

## <a name="permissions"></a>Разрешения  
В [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] требуются разрешения `VIEW SERVER STATE` и `ALTER TRACE`. В [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)] необходимо разрешение `VIEW DATABASE STATE` для базы данных.

## <a name="see-also"></a>См. также:  
 [Monitor and Tune for Performance](../../relational-databases/performance/monitor-and-tune-for-performance.md)     
 [Средства контроля и настройки производительности](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)     
 [Открытие монитора активности (среда SQL Server Management Studio)](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)     
 [Монитор активности](../../relational-databases/performance-monitor/activity-monitor.md)     
 [Мониторинг производительности с использованием хранилища запросов](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)     

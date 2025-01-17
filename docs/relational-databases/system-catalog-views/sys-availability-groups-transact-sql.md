---
title: sys.availability_groups (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.availability_groups_TSQL
- availability_groups_TSQL
- sys.availability_groups
- availability_groups
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_groups catalog view
ms.assetid: da7fa55f-c008-45d9-bcfc-3513b02d9e71
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b6992cceb19d0fe39c1e53e36535949a94907295
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67942658"
---
# <a name="sysavailabilitygroups-transact-sql"></a>sys.availability_groups (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Возвращает по строке для каждой группы доступности, для которой в локальном экземпляре SQL Server размещена реплика доступности. Каждая строка содержит кэшированную копию метаданных группы доступности.  
   
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Уникальный идентификатор (GUID) группы доступности.|  
|**name**|**sysname**|Имя группы доступности. Определяемое пользователем имя, которое должно быть уникальным в отказоустойчивом кластере Windows Server (WSFC).|  
|**resource_id**|**nvarchar(40)**|Идентификатор ресурса для ресурса кластера WSFC.|  
|**resource_group_id**|**nvarchar(40)**|Идентификатор группы ресурсов кластера WSFC, принадлежащей к группе доступности.|  
|**failure_condition_level**|**int**|Определяемые пользователем уровень условий сбоя от имени которой должны активироваться автоматическая отработка отказа, один из целочисленных значений, приведенных в таблице сразу под этой таблицей.<br /><br /> Уровни условий сбоя (1–5) варьируются от наименее ограничительного уровня 1 до наиболее ограничительного уровня 5. Заданный уровень условий включает в себя ограничения всех предыдущих уровней. Таким образом, наиболее строгий уровень 5 включает в себя ограничения уровней с 1 по 4, уровень 4 содержит ограничения уровней с 1 по 3 и т. д.<br /><br /> Чтобы изменить это значение, используйте параметр FAILURE_CONDITION_LEVEL [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] инструкции.|  
|**health_check_timeout**|**int**|Время ожидания (в миллисекундах для) [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) системные хранимые процедуры возвращают сведения о состоянии сервера, прежде чем на экземпляре сервера предполагается, что медленно или завис. Значение по умолчанию — 30 000 миллисекунд (30 секунд).<br /><br /> Чтобы изменить это значение, используйте параметр HEALTH_CHECK_TIMEOUT [ALTER AVAILABILITY GROUP](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] инструкции.|  
|**automated_backup_preference**|**tinyint**|Предпочитаемое расположение для выполнения резервного копирования баз данных доступности в этой группе доступности. Ниже приведены возможные значения и их описания.<br /><br /> <br /><br /> 0: Основной. Резервное копирование должно всегда выполняться в первичной реплике.<br /><br /> 1\. Только Вторичная. Создание резервных копий во вторичной реплике является предпочтительным.<br /><br /> 2\. Предпочтение вторичной. Создание резервных копий во вторичной реплике является предпочтительным, но создание резервных копий в первичной реплике также является допустимым при отсутствии вторичных реплик для операций резервного копирования. Это поведение по умолчанию.<br /><br /> 3\. Любая реплика. Приоритет места выполнения резервного копирования отсутствует.<br /><br /> <br /><br /> Дополнительные сведения см. в статье [Активные вторичные реплики: резервное копирование во вторичных репликах (группы доступности Always On)](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).|  
|**automated_backup_preference_desc**|**nvarchar(60)**|Описание **automated_backup_preference**, используя один из:<br /><br /> PRIMARY<br /><br /> SECONDARY_ONLY<br /><br /> SECONDARY<br /><br /> None|  
|**version**|**smallint**|Версия метаданные группы доступности, хранящиеся в отказоустойчивом кластере Windows. Этот номер версии увеличивается, когда добавляются новые функции.|  
|**basic_features**|**bit**|Указывает, является ли это основной группы доступности. Дополнительные сведения см. в разделе [Базовые группы доступности (группы доступности AlwaysOn)](../../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).|  
|**dtc_support**|**bit**|Указывает, включена ли поддержка DTC для этой группы доступности. **DTC_SUPPORT** параметр **CREATE AVAILABILITY GROUP** управляет этот параметр.|  
|**db_failover**|**bit**|Указывает, поддерживает ли группы доступности перехода на другой ресурс для условий работоспособности базы данных. **DB_FAILOVER** параметр **CREATE AVAILABILITY GROUP** управляет этот параметр.|  
|**is_distributed**|**bit**|Указывает, является ли это распределенной группы доступности. Дополнительные сведения см. в разделе [Распределенные группы доступности (группы доступности AlwaysOn)](../../database-engine/availability-groups/windows/distributed-availability-groups-always-on-availability-groups.md).|  
  
## <a name="failure-condition-level--values"></a>Значения уровня условия сбоя  
 В следующей таблице описаны уровни условие возможного сбоя для **failure_condition_level** столбца.  
  
|Значение|Условие сбоя|  
|-----------|-----------------------|  
|1|Указывает, что следует запустить автоматический переход на другой ресурс при возникновении любой из следующих ситуаций.<br /><br /> <br /><br /> - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Служба не работает.<br /><br /> -Аренда группы доступности для подключения к отказоустойчивому кластеру WSFC истекла, поскольку подтверждение не получено от экземпляра сервера. Дополнительные сведения см. в разделе [Как это работает: время ожидания аренды Always On в SQL Server](https://blogs.msdn.com/b/psssql/archive/2012/09/07/how-it-works-sql-server-Always%20On-lease-timeout.aspx).|  
|2|Указывает, что следует запустить автоматический переход на другой ресурс при возникновении любой из следующих ситуаций.<br /><br /> <br /><br /> -Экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не подключается к кластеру и определяемый пользователем **health_check_timeout** группы доступности порогового значения.<br /><br /> -Реплика доступности находится в состоянии сбоя.|  
|3|Указывает, что следует запустить автоматический переход на другой ресурс в случае появления критических внутренних ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], таких как потерянные спин-блокировки, серьезные нарушения доступа для записи или формирование слишком больших дампов.<br /><br /> Это значение по умолчанию.|  
|4|Указывает, что следует запустить автоматический переход на другой ресурс в случае появления не столь серьезных внутренних ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], например устойчивое состояние нехватки памяти в пуле внутренних ресурсов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|5|Указывает, что следует запустить автоматический переход на другой ресурс при любом удовлетворяющим условиям состоянии сбоя, включая:<br /><br /> <br /><br /> -Исчерпание рабочих потоков SQL Engine.<br /><br /> -Обнаружение неразрешимой взаимоблокировки.|  
  
## <a name="security"></a>Безопасность  
  
### <a name="permissions"></a>Разрешения  
 Требует разрешения VIEW ANY DEFINITION на экземпляре сервера.  
  
## <a name="see-also"></a>См. также  
 [sys.availability_replicas (Transact-SQL)](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [Группы доступности AlwaysOn (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Отслеживание групп доступности (Transact-SQL)](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Отслеживание групп доступности (Transact-SQL)](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  

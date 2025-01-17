---
title: Использование WQL с поставщиком WMI для событий сервера | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: wmi
ms.topic: reference
helpviewer_keywords:
- queries [WMI]
- query language [WMI]
- WMI Query Language [WMI]
- WQL [WMI]
- WMI Provider for Server Events, WQL
ms.assetid: 58b67426-1e66-4445-8e2c-03182e94c4be
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 17d28b2d8d2da467fa07bd2c2ddc2de8db207cca
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68139381"
---
# <a name="using-wql-with-the-wmi-provider-for-server-events"></a>Использование WQL с поставщиком WMI для событий сервера
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Приложения управления могут получать доступ к событиям [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью поставщика WMI для событий сервера путем выполнения инструкций WMI Query Language (WQL). WQL является упрощенным подмножеством языка SQL с некоторыми расширениями, специфичными для WMI. При использовании WQL приложение извлекает тип события из определенного экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], базы данных или объекта базы данных (в настоящее время поддерживаются только объекты очереди). Поставщик WMI для событий сервера преобразует запрос в уведомление о событии, созданный в целевую базу данных для уведомлений о событиях уровня базы данных или уровня объекта, или в **master** событиях уровня сервера базы данных уведомления.  
  
 Например, рассмотрим следующий WQL-запрос.  
  
```  
SELECT * FROM DDL_DATABASE_LEVEL_EVENTS WHERE DatabaseName = 'AdventureWorks'  
```  
  
 На основе этого запроса поставщик WMI совершает попытку создать эквивалент этого уведомления о событии на целевом сервере.  
  
```  
USE AdventureWorks ;  
GO  
  
CREATE EVENT NOTIFICATION SQLWEP_76CF38C1_18BB_42DD_A7DC_C8820155B0E9  
    ON DATABASE  
    WITH FAN_IN  
    FOR DDL_DATABASE_LEVEL_EVENTS  
    TO SERVICE   
        'SQL/Notifications/ProcessWMIEventProviderNotification/v1.0',  
        'A7E5521A-1CA6-4741-865D-826F804E5135';  
GO  
```  
  
 Аргумент в предложении `FROM` WQL-запроса (`DDL_DATABASE_LEVEL_EVENTS`) может представлять любое допустимое событие, о котором может быть создано уведомление. Аргументы в предложениях `SELECT` и `WHERE` могут указывать любые свойства событий, связанные с событием или его родительским событием. Список допустимых событий и свойств событий, см. в разделе [уведомления о событиях (компонент Database Engine)](https://technet.microsoft.com/library/ms182602.aspx).  
  
 Следующий синтаксис языка WQL явно поддерживается поставщиком WMI для событий сервера. Может быть указан дополнительный синтаксис WQL, но он не зависит от этого поставщика и вместо этого анализируется службой узлов WMI. Дополнительные сведения о языке запросов WMI см. в документации по WQL на веб-узле MSDN.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
SELECT { event_property [ ,...n ] | * }  
FROM event_type   
WHERE where_condition   
```  
  
## <a name="arguments"></a>Аргументы  
 *event_property*  
 Свойство события. К ним относятся **PostTime**, **SPID**, и **LoginName**. Просмотрите каждое событие, перечисленные в [поставщик WMI для событий классов и свойств сервера](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-classes-and-properties.md) чтобы определить, какие свойства оно имеет. Например, событие ddl_database_level_events имеет содержит **DatabaseName** и **UserName** свойства. Также наследует **SQLInstance**, **LoginName**, **PostTime**, **SPID**, и **ComputerName** свойства от родительских событий.  
  
 **,** *.. .n*  
 Указывает, что *event_property* могут запрашиваться несколько раз, разделенных запятыми.  
  
 \*  
 Указывает, что запрашиваются все свойства, связанные с событием.  
  
 *event_type*  
 Событие, для которого может быть создано уведомление. Список доступных событий, см. в разделе [поставщик WMI для событий классов и свойств сервера](https://technet.microsoft.com/library/ms186449.aspx). Обратите внимание, что *тип события* имена соответствуют тому же *event_type* | *event_group* , могут быть указаны при создании вручную уведомления о событии с помощью CREATE EVENT NOTIFICATION. Примеры *тип события* включают CREATE_TABLE, LOCK_DEADLOCK, DDL_USER_EVENTS и TRC_DATABASE.  
  
> [!NOTE]  
>  Определенные системные хранимые процедуры, выполняющие DDL-подобные операции, могут также вызывать формирование уведомления о событиях. Протестируйте свои уведомления о событиях, чтобы определить их реакцию на системные хранимые процедуры. Например, инструкция CREATE TYPE и **sp_addtype** хранимой процедуры вызывают срабатывание уведомления о событии, созданного на событии CREATE_TYPE. Тем не менее **sp_rename** хранимой процедуры не запускает уведомления о событиях. Дополнительные сведения см. в разделе[DDL-события](../../relational-databases/triggers/ddl-events.md).  
  
 *where_condition*  
 Является предикат запроса в предложении WHERE, состоящий из *event_property* имена и логические операторы и операторы сравнения. *Where_condition* определяет область, в котором соответствующее уведомление о событии регистрируется в целевую базу данных. Он также может действовать как фильтр для определения конкретной схемы или объект, из которого запрос *event_type.* Дополнительные сведения см. в подразделе «Примечания» далее в этом разделе.  
  
 Только `=` операнд может использоваться вместе с **DatabaseName**, **SchemaName**, и **ObjectName**. Другие выражения нельзя использовать с этими свойствами событий.  
  
## <a name="remarks"></a>Примечания  
 *Where_condition* поставщика WMI для событий сервера синтаксис аргумента:  
  
-   Область, в которой поставщик пытается получить указанный *event_type*: уровень сервера, уровень базы данных или уровень объекта (в настоящее время поддерживаются только объекты очереди). В конечном счете эта область определяет тип уведомления о событии, создаваемого в базе данных-получателе. Этот процесс называется регистрацией уведомления о событии.  
  
-   База данных, схема и объект (если применимо), в которых выполняется регистрация.  
  
 Поставщик WMI для событий сервера использует восходящий алгоритм «первый подходящий» для создания наиболее узкой области для базового уведомления EVENT NOTIFICATION. Алгоритм пытается минимизировать внутреннюю активность на сервере и сетевой трафик между экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и процессом на узле WMI. Поставщик исследует *event_type* указанное в предложении FROM и условия в предложении WHERE и пытается зарегистрировать базовое уведомление EVENT NOTIFICATION в наиболее узкой возможной области. Если поставщику не удается выполнить регистрацию в наиболее узкой области, он пытается выполнить ее в областях более высокого уровня, пока регистрация не пройдет успешно. Если достигнут самый высокий уровень (уровень сервера), а регистрация завершается сбоем, возвращается сообщение об ошибке.  
  
 Например если имя базы данных = **"** AdventureWorks **"** указан в предложении WHERE, то поставщик пытается зарегистрировать уведомление о событии в [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] базы данных. Если база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] существует, а у вызывающего клиента достаточно разрешений для создания уведомления в [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], регистрация выполняется успешно. В противном случае выполняется попытка зарегистрировать уведомление о событии на уровне сервера. Регистрация выполняется успешно, если клиент WMI имеет необходимые разрешения. Однако в этом сценарии события не возвращаются клиенту, пока не будет создана база данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
 *Where_condition* также может действовать как фильтр для дополнительного ограничения запроса для конкретной базы данных, schema или object. Например, рассмотрим следующий WQL-запрос.  
  
```  
SELECT * FROM ALTER_TABLE   
WHERE DatabaseName = 'AdventureWorks' AND SchemaName = 'Sales'   
    AND ObjectType='Table' AND ObjectName = 'SalesOrderDetail'  
```  
  
 В зависимости от результатов процесса регистрации WQL-запрос может быть зарегистрирован либо на уровне базы данных, либо на уровне сервера. Однако даже если он зарегистрирован на уровне сервера, поставщик в конечном итоге отфильтровывает любые события `ALTER_TABLE`, неприменимые к таблице `AdventureWorks.Sales.SalesOrderDetail`. Иными словами, поставщик возвращает только свойства событий `ALTER_TABLE`, возникших в конкретной таблице.  
  
 Если указано составное выражение (такое как `DatabaseName='AW1'` OR `DatabaseName='AW2'`), выполняется попытка зарегистрировать одиночное уведомление о событии на уровне сервера вместо регистрации двух отдельных уведомлений о событии. Регистрация выполняется успешно, если вызывающий клиент имеет необходимые разрешения.  
  
 Если `SchemaName='X' AND ObjectType='Y' AND ObjectName='Z'` являются все указанные в `WHERE` предложения, будет предпринята попытка зарегистрировать уведомление о событии непосредственно в объекте `Z` в схеме `X`. Регистрация выполняется успешно, если клиент имеет необходимые разрешения. Обратите внимание, что, в настоящее время события на уровне объектов поддерживаются только для очередей, а также только для QUEUE_ACTIVATION *event_type*.  
  
 Следует иметь в виду, что не все события могут запрашиваться во всех областях действия. Например, WQL-запрос о событии трассировки, таком как Lock_Deadlock, или группе событий трассировки, такой как TRC_LOCKS, может быть зарегистрирован только на уровне сервера. Аналогичным образом событие CREATE_ENDPOINT и группа событий DDL_ENDPOINT_EVENTS также могут быть зарегистрированы только на уровне сервера. Дополнительные сведения о соответствующей области для регистрации событий, см. в разделе [Проектирование уведомлений о событиях](https://technet.microsoft.com/library/ms175854\(v=sql.105\).aspx). Попытка регистрации WQL-запроса с типом *event_type* может быть зарегистрирован только на сервере уровень, всегда выполняется на уровне сервера. Регистрация выполняется успешно, если клиент WMI имеет необходимые разрешения. Иначе клиенту возвращается ошибка. Однако в некоторых случаях можно использовать предложение WHERE в качестве фильтра для событий на уровне сервера на основе свойств, соответствующих событию. Например, многие события трассировки имеют **DatabaseName** свойство, которое может использоваться в предложении WHERE в качестве фильтра.  
  
 Уведомления о событиях уровня сервера создаются в **master** базы данных и может запрашиваться для метаданных с помощью [sys.server_event_notifications](../../relational-databases/system-catalog-views/sys-server-event-notifications-transact-sql.md) представления каталога.  
  
 Уровня базы данных или уведомления о событиях уровня объекта создаются в указанной базе данных и могут запрашиваться для метаданных с помощью [sys.event_notifications](../../relational-databases/system-catalog-views/sys-event-notifications-transact-sql.md) представления каталога. Представление каталога должно иметь префикс с именем соответствующей базы данных.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-querying-for-events-at-the-server-scope"></a>A. Запросы событий на уровне сервера  
 Следующий WQL-запрос получает все свойства событий для любого события трассировки `SERVER_MEMORY_CHANGE`, возникшего в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
SELECT * FROM SERVER_MEMORY_CHANGE  
```  
  
### <a name="b-querying-for-events-at-the-database-scope"></a>Б. Запросы событий на уровне базы данных  
 Следующий WQL-запрос получает конкретные свойства событий для любых событий, возникающих в базе данных `AdventureWorks` и существующих в группе событий `DDL_DATABASE_LEVEL_EVENTS`.  
  
```  
SELECT SPID, SQLInstance, DatabaseName FROM DDL_DATABASE_LEVEL_EVENTS   
WHERE DatabaseName = 'AdventureWorks'   
```  
  
### <a name="c-querying-for-events-at-the-database-scope-filtering-by-schema-and-object"></a>В. Запросы событий на уровне базы данных, фильтрация по схеме и объекту  
 Следующий запрос получает все свойства событий для любого события `ALTER_TABLE`, возникшего в таблице `AdventureWorks.Sales.SalesOrderDetail`.  
  
```  
SELECT * FROM ALTER_TABLE   
WHERE DatabaseName = 'AdventureWorks' AND SchemaName = 'Sales'   
    AND ObjectType='Table' AND ObjectName = 'SalesOrderDetail'  
```  
  
## <a name="see-also"></a>См. также  
 [Поставщик WMI для событий сервера основные понятия](https://technet.microsoft.com/library/ms180560.aspx)   
 [Уведомления о событиях (компонент Database Engine)](https://technet.microsoft.com/library/ms182602.aspx)  
  
  

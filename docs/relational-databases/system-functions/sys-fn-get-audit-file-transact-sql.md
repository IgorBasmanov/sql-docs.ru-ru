---
title: sys.fn_get_audit_file (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 05/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- fn_get_audit_file_TSQL
- sys.fn_get_audit_file_TSQL
- fn_get_audit_file
- sys.fn_get_audit_file
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_get_audit_file function
- fn_get_audit_file function
ms.assetid: d6a78d14-bb1f-4987-b7b6-579ddd4167f5
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 350b1eca94f8041a0a38105c650e1c0ec7e1f617
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68046284"
---
# <a name="sysfngetauditfile-transact-sql"></a>sys.fn_get_audit_file (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Возвращает сведения из файла аудита, созданного аудитом сервера в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в статье [Подсистема аудита SQL Server (ядро СУБД)](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
fn_get_audit_file ( file_pattern,   
    { default | initial_file_name | NULL },   
    { default | audit_record_offset | NULL } )  
```  
  
## <a name="arguments"></a>Аргументы  
 *file_pattern*  
 Указывает каталог или путь и имя файла для файла аудита, который предстоит прочитать. Тип — **nvarchar(260)** . 
 
 - **SQL Server**:
    
    Этот аргумент должен содержать как путь (букву диска или сетевой ресурс), так и имя файла, которое может включать символ-шаблон. Одна звездочка (*) используется для выбора нескольких файлов из набора файлов аудита. Пример:  
  
    -   **\<путь >\\ \***  - собрать все файлы аудита в указанном расположении.  
  
    -   **\<путь > \LoginsAudit_{GUID}** - собрать все файлы, которые имеют указанное имя и идентификатор GUID аудита.  
  
    -   **\<путь > \LoginsAudit_{GUID}_00_29384.sqlaudit** -собирать указанный файл аудита.  
  
 - **База данных Azure SQL**:
 
    Этот аргумент используется для указания URL-адрес большого двоичного объекта (в том числе конечную точку хранилища и контейнер). Хотя он не поддерживает шаблон «звездочка», можно использовать префикс имени частичного файла (blob) (вместо имени весь большой двоичный объект) для выбора нескольких файлов (BLOB), которые начинаются с этого префикса. Пример:
 
      - **\<Storage_endpoint\>/\<контейнера\>/\<ServerName\>/\<DatabaseName\> /**  -собирает все файлы аудита (BLOB) для конкретной базы данных.    
      
      - **\<Storage_endpoint\>/\<контейнера\>/\<ServerName\>/\<DatabaseName\> / \< AuditName\>/\<CreationDate\>/\<FileName\>.xel** -собирает указанный файл аудита (blob).
  
> [!NOTE]  
>  При передаче пути без имени файла сформируется ошибка.  
  
 *initial_file_name*  
 Указывает путь и имя файла для определенного файла в наборе файлов аудита, из которого предстоит начать чтение записей аудита. Тип — **nvarchar(260)** .  
  
> [!NOTE]  
>  *Параметров initial_file_name* аргумент должен содержать допустимые записи или должен содержать значение по умолчанию | Значение NULL.  
  
 *audit_record_offset*  
 Указывает известное местоположение с файлом, указанным для initial_file_name. Когда используется этот аргумент, функция начнет чтение с первой записи буфера, следующей немедленно после указанного смещения.  
  
> [!NOTE]  
>  *Audit_record_offset* аргумент должен содержать допустимые записи или должен содержать значение по умолчанию | Значение NULL. Тип — **bigint**.  
  
## <a name="tables-returned"></a>Возвращаемые таблицы  
 В следующей таблице описано содержимое файла аудита, которое может возвращаться этой функцией.  
  
| Имя столбца | Type | Описание |  
|-------------|------|-------------|  
| action_id | **varchar(4)** | Идентификатор действия. Не допускает значения NULL. |  
| additional_information | **nvarchar(4000)** | Уникальные сведения, применимые только к одиночному событию, возвращаются в формате XML. Немногие действия, доступные для аудита, содержат сведения этого вида.<br /><br /> Для действий, имеющих связанный с ними стек TSQL, отображается один уровень стека TSQL в формате XML. Используется следующий формат XML:<br /><br /> `<tsql_stack><frame nest_level = '%u' database_name = '%.*s' schema_name = '%.*s' object_name = '%.*s' /></tsql_stack>`<br /><br /> Параметр «frame nest_level» указывает текущий уровень вложенности кадра. Имя модуля состоит из трех частей (имя базы данных database_name, имя схемы schema_name и имя объекта object_name).  Имя модуля будут анализироваться для экранирования недопустимый xml символы, такие как `'\<'`, `'>'`, `'/'`, `'_x'`. Они будут экранируются как `_xHHHH\_`. Здесь HHHH обозначает четырехразрядный шестнадцатеричный код символа в UCS-2.<br /><br /> Допускает значение NULL. Возвращает NULL, если событие не сообщает дополнительных сведений. |
| affected_rows | **bigint** | **Область применения**: Только база данных Azure SQL<br /><br /> Число строк, затронутых при выполненной инструкции. |  
| application_name | **nvarchar(128)** | **Область применения**: Azure SQL DB и экземпляр SQL Server (начиная с 2017)<br /><br /> Имя клиентского приложения, для которого выполнена инструкцию, которая вызвала событие аудита |  
| audit_file_offset | **bigint** | **Aplies для**: Только SQL Server<br /><br /> Смещение буфера в файле, который содержит запись аудита. Не допускает значение NULL. |  
| audit_schema_version | **int** | Всегда имеет значение 1 |  
| class_type | **varchar(2)** | Тип доступной для аудита сущности, для которой проводится аудит. Не допускает значение NULL. |  
| client_ip | **nvarchar(128)** | **Область применения**: Azure SQL DB и экземпляр SQL Server (начиная с 2017)<br /><br />    Исходный IP-адрес клиентского приложения |  
| connection_id | GUID | **Область применения**: Azure SQL DB и управляемый экземпляр<br /><br /> Идентификатор соединения на сервере |
| data_sensitivity_information | nvarchar(4000) | **Область применения**: Только база данных Azure SQL<br /><br /> Типы информации и меток конфиденциальности, возвращаемых запросом подвергшиеся аудиту, на основе классифицированных столбцов в базе данных. Дополнительные сведения о [базы данных SQL Azure, данные обнаружения и классификации](https://docs.microsoft.com/azure/sql-database/sql-database-data-discovery-and-classification) |
| database_name | **sysname** | Контекст базы данных, в котором выполнялось действие. Допускает значение NULL. Возвращает значение NULL для аудитов, выполняемых на уровне сервера. |  
| database_principal_id | **int** |Идентификатор контекста пользователя базы данных, в котором выполнено действие. Не допускает значение NULL. Поддерживает 0, если это неприменимо. Например, операция сервера.|
| database_principal_name | **sysname** | Текущий пользователь. Допускает значение NULL. Возвращает значение NULL, если недоступно. |  
| duration_milliseconds | **bigint** | **Область применения**: Azure SQL DB и управляемый экземпляр<br /><br /> Длительность выполнения запроса в миллисекундах |
| event_time | **datetime2** | Дата и время срабатывания действия, доступного для аудита. Не допускает значение NULL. |  
| file_name | **varchar(260)** | Путь и имя файла журнала аудита, из которого получена запись. Не допускает значение NULL. |
| is_column_permission | **bit** | Флаг, обозначающий разрешение уровня столбца. Не допускает значение NULL. Возвращает 0, если permission_bitmask = 0.<br /> 1 = true;<br /> 0 = false. |
| object_id | **int** | Идентификатор сущности, над которой выполнен аудит. Это включает следующие действия.<br /> Объекты сервера.<br /> Базы данных<br /> Объекты базы данных<br /> Объекты схемы.<br /> Не допускает значение NULL. Возвращает 0, если сущностью является сам сервер или если аудит не выполняется на уровне объекта. Например, проверка подлинности. |  
| object_name | **sysname** | Имя сущности, для которой проводился аудит. Это включает следующие действия.<br /> Объекты сервера.<br /> Базы данных<br /> Объекты базы данных<br /> Объекты схемы.<br /> Допускает значение NULL. Возвращает NULL, если сущностью является сам сервер или если аудит не выполняется на уровне объекта. Например, проверка подлинности. |
| permission_bitmask | **varbinary(16)** | В некоторых действиях это предоставленные, запрещенные или отмененные разрешения. |
| response_rows | **bigint** | **Область применения**: Azure SQL DB и управляемый экземпляр<br /><br /> Количество строк, возвращаемых в результирующем наборе. |  
| schema_name | **sysname** | Контекст схемы, в котором выполнялось действие. Допускает значение NULL. Возвращает значение NULL для аудитов, выполняемых вне схемы. |  
| sequence_group_id | **varbinary** | **Область применения**: Только SQL Server (начиная с 2016)<br /><br />  Уникальный идентификатор |  
| sequence_number | **int** | Отслеживает последовательность записей в одной записи аудита, слишком большой, чтобы уместиться в буфере записи для аудитов. Не допускает значение NULL. |  
| server_instance_name | **sysname** | Имя экземпляра сервера, где проводился аудит. Используется стандартный формат «сервер\экземпляр». |  
| server_principal_id | **int** | Идентификатор контекста имени входа, в котором выполнено действие. Не допускает значение NULL. |  
| server_principal_name | **sysname** | Текущее имя входа. Допускает значение NULL. |  
| server_principal_sid | **varbinary** | Идентификатор безопасности текущего имени входа. Допускает значение NULL. |  
| session_id | **smallint** | Идентификатор сеанса, в котором произошло событие. Не допускает значение NULL. |  
| session_server_principal_name | **sysname** | Участник на уровне сервера для сеанса. Допускает значение NULL. |  
| инструкция | **nvarchar(4000)** | Инструкция TSQL (если существует). Допускает значение NULL. Если неприменимо, возвращается значение NULL. |  
| succeeded | **bit** | Показывает, было ли успешным действие, запустившее событие. Не допускает значение NULL. Для всех событий, отличных от событий имени входа, при этом формируются только сообщения о том, была ли проверка разрешения выполнена успешно или окончилась неудачей, а не сообщения о самой операции.<br /> 1 = успешное завершение;<br /> 0 = неуспешное завершение. |
| target_database_principal_id | **int** | Участник базы данных, над которым выполняется операция GRANT/DENY/REVOKE. Не допускает значение NULL. Если неприменимо, возвращается значение 0. |  
| target_database_principal_name | **sysname** | Целевой пользователь действия. Допускает значение NULL. Если неприменимо, возвращается значение NULL. |  
| target_server_principal_id | **int** | Участник на уровне сервера, над которым выполняется операция GRANT/DENY/REVOKE. Не допускает значение NULL. Если неприменимо, возвращается значение 0. |  
| target_server_principal_name | **sysname** | Целевое имя входа действия. Допускает значение NULL. Если неприменимо, возвращается значение NULL. |  
| target_server_principal_sid | **varbinary** | Идентификатор безопасности целевого имени входа. Допускает значение NULL. Если неприменимо, возвращается значение NULL. |  
| transaction_id | **bigint** | **Область применения**: Только SQL Server (начиная с 2016)<br /><br /> Уникальный идентификатор для определения нескольких событий аудита в одну транзакцию |  
| user_defined_event_id | **smallint** | **Применяется к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] через [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], экземпляр базы данных SQL Azure и управление ими<br /><br /> Определенный пользователем идентификатор события передается в качестве аргумента для **sp_audit_write**. **NULL** для системных событий (по умолчанию) и ненулевой идентификатор для определяемого пользователем события. Дополнительные сведения см. в разделе [sp_audit_write &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-audit-write-transact-sql.md). |  
| user_defined_information | **nvarchar(4000)** | **Применяется к**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] через [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], экземпляр базы данных SQL Azure и управление ими<br /><br /> Используется для записи любой дополнительной информации, пользователю необходимо записать в журнал аудита с помощью **sp_audit_write** хранимой процедуры. |  

  
## <a name="remarks"></a>Примечания  
 Если *file_pattern* аргумент, передаваемый **fn_get_audit_file** ссылается на путь или файл, который не существует, или если файл не является файлом аудита, **MSG_INVALID_AUDIT_FILE**возвращается сообщение об ошибке.  
  
## <a name="permissions"></a>Разрешения

- **SQL Server**: Требуется разрешение **CONTROL SERVER** .  
- **База данных Azure SQL**: Требуется **базы данных системы УПРАВЛЕНИЯ** разрешение.     
  - Администраторы серверов могут обращаться к журналам аудита всех баз данных на сервере.
  - Администраторы серверов не имеет доступ только к журналам аудита из текущей базы данных.
  - Большие двоичные объекты, которые не соответствуют приведенным выше критериям будут пропущены (список пропущенных больших двоичных объектов отображается в выходном сообщении запроса), и функция возвратит журналы только из больших двоичных объектов, для которых разрешен доступ.  
  
## <a name="examples"></a>Примеры

- **SQL Server**

  В следующем примере выполняется чтение из файла, который имеет имя `\\serverName\Audit\HIPAA_AUDIT.sqlaudit`.  
  
  ```  
  SELECT * FROM sys.fn_get_audit_file ('\\serverName\Audit\HIPAA_AUDIT.sqlaudit',default,default);  
  GO  
  ```  

- **База данных SQL Azure**

  В этом примере выполняется чтение из файла с именем `ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel`:  
  
  ```  
  SELECT * FROM sys.fn_get_audit_file ('https://mystorage.blob.core.windows.net/sqldbauditlogs/ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel',default,default);
  GO  
  ```  

  Этот пример считывает из тот же файл, как описано выше, но с помощью дополнительных предложений T-SQL (**ВЕРХНЕЙ**, **ORDER BY**, и **ГДЕ** предложение для фильтрации возвращенных записей аудита функция):
  
  ```  
  SELECT TOP 10 * FROM sys.fn_get_audit_file ('https://mystorage.blob.core.windows.net/sqldbauditlogs/ShiraServer/MayaDB/SqlDbAuditing_Audit/2017-07-14/10_45_22_173_1.xel',default,default)
  WHERE server_principal_name = 'admin1'
  ORDER BY event_time
  GO
  ```  

  Этот пример считывает все журналы с серверов, которые начинаются с аудита `Sh`: 
  
  ```  
  SELECT * FROM sys.fn_get_audit_file ('https://mystorage.blob.core.windows.net/sqldbauditlogs/Sh',default,default);
  GO  
  ```

Полный пример создания аудита см. в разделе [Аудит SQL Server (ядро СУБД)](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).

Сведения о настройке аудита базы данных SQL Azure, см. в разделе [приступить к работе с аудитом базы данных SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing).
  
## <a name="see-also"></a>См. также  
 [CREATE SERVER AUDIT (Transact-SQL)](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT (Transact-SQL)](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT (Transact-SQL)](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.server_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Создание аудита сервера и спецификации аудита сервера](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  

---
title: Хранимые процедуры (ядро СУБД) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- storing programs as stored procedures
- stored procedures [SQL Server], about stored procedures
ms.assetid: cc6daf62-9663-4c3e-950a-ab42e2830427
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f41eb44b026c78a3d99814b231f52b518c18a177
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68136581"
---
# <a name="stored-procedures-database-engine"></a>Хранимые процедуры (компонент Database Engine)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Хранимая процедура в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] — это группа из одной или нескольких инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)] или ссылка на метод [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] среды CLR. Процедуры аналогичны конструкциям в других языках программирования, поскольку обеспечивают следующее:  
  
-   обрабатывают входные параметры и возвращают вызывающей программе значения в виде выходных параметров;  
  
-   содержат программные инструкции, которые выполняют операции в базе данных, включая вызов других процедур;  
  
-   возвращают значение состояния вызывающей программе, таким образом передавая сведения об успешном или неуспешном завершении (и причины последнего).  
  
## <a name="benefits-of-using-stored-procedures"></a>Преимущества хранимых процедур  
 В следующем списке описываются преимущества использования процедур.  
  
 Снижение сетевого трафика между клиентами и сервером  
 Команды в процедуре выполняются как один пакет кода. Это позволяет существенно сократить сетевой трафик между сервером и клиентом, поскольку по сети отправляется только вызов на выполнение процедуры. Без инкапсуляции кода, предоставляемой процедурой, по сети бы пришлось пересылать все отдельные строки кода.  
  
 Большая безопасность  
 Многие пользователи и клиентские программы могут выполнять операции с базовыми объектами базы данных посредством процедур, даже если у них нет прямых разрешений на доступ к базовым объектам. Процедура проверяет, какие из процессов и действий могут выполняться, и защищает базовые объекты базы данных. Это устраняет необходимость предоставлять разрешения на уровне индивидуальных объектов и упрощает формирование уровней безопасности.  
  
 Предложение [EXECUTE AS](../../t-sql/statements/execute-as-clause-transact-sql.md) может быть указано в инструкции CREATE PROCEDURE, чтобы разрешить олицетворение других пользователей или разрешить пользователям или приложениям выполнять определенные действия баз данных без необходимости иметь прямые разрешения на базовые объекты и команды. Например, для некоторых действий, таких как TRUNCATE TABLE, предоставить разрешения нельзя. Чтобы выполнить инструкцию TRUNCATE TABLE, у пользователя должны быть разрешения ALTER на нужную таблицу. Предоставление разрешений ALTER не всегда подходит, так как фактические разрешения пользователя выходят за пределы возможности усечения таблицы. Заключив инструкцию TRUNCATE TABLE в модуль и указав, что этот модуль должен выполняться от имени пользователя, у которого есть разрешения на изменение таблицы, можно предоставить разрешение на усечение таблицы пользователю с разрешением EXECUTE для этого модуля.  
  
 При вызове процедуры через сеть виден только вызов на выполнение процедуры. Следовательно, злоумышленники не смогут видеть имена объектов таблиц и баз данных, внедрять свои инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] или выполнять поиск важных данных.  
  
 Использование параметров в процедурах помогает предотвратить атаки типа «инъекция SQL». Поскольку входные данные параметра обрабатываются как литеральные значения, а не как исполняемый код, злоумышленнику будет труднее вставить команду в инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)] в процедуре и создать угрозу безопасности.  
  
 Процедуры могут быть зашифрованы, что позволяет замаскировать исходный код. Дополнительные сведения см. в статье [SQL Server Encryption](../../relational-databases/security/encryption/sql-server-encryption.md).  
  
 Повторное использование кода  
 Если какой-то код многократно используется в операции базы данных, то отличным решением будет произвести его инкапсуляцию в процедуры. Это устранит необходимость излишнего копирования того же кода, снизит уровень несогласованности кода и позволит осуществлять доступ к коду любым пользователям или приложениям, имеющим необходимые разрешения.  
  
 Более легкое обслуживание  
 Если клиентские приложения вызывают процедуры, а операции баз данных остаются на уровне данных, то для внесения изменений в основную базу данных будет достаточно обновить только процедуры. Уровень приложения остается незатронутым изменениями в схемах баз данных, связях или процессах.  
  
 Повышенная производительность  
 По умолчанию компиляция процедуры и создание плана выполнения, используемого для последующих выполнений, производится при ее первом запуске Поскольку обработчику запросов не нужно создавать новый план, обычно обработка процедуры занимает меньше времени.  
  
 Если в таблицах или данных, на которые ссылается процедура, произошли значительные изменения, то наличие предварительно скомпилированного плана может вызвать замедление работы процедуры. В этом случае перекомпиляция процедуры и принудительное создание нового плана выполнения может улучшить производительность.  
  
## <a name="types-of-stored-procedures"></a>Типы хранимых процедур  
 Пользовательские процедуры  
 Пользовательские процедуры могут быть созданы в пользовательской базе данных или любых системных базах данных, за исключением базы данных **Resource** . Процедура может быть разработана либо на языке [!INCLUDE[tsql](../../includes/tsql-md.md)] , либо как ссылка на метод [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] среды CLR.  
  
 Временные процедуры  
 Временные процедуры — это один из видов пользовательских процедур. Временные процедуры схожи с постоянными процедурами, за исключением того, что они хранятся в базе данных **tempdb**. Существует два вида временных процедур: локальные и глобальные. Они отличаются друг от друга именами, видимостью и доступностью. Имена локальных временных процедур начинаются с одного знака диеза (#); они видны только текущему соединению пользователя и удаляются, когда закрывается соединение. Имена глобальных временных процедур начинаются с двух знаков диеза (##); они видны любому пользователю и удаляются после окончания последнего сеанса, использующего процедуру.  
  
 Система  
 Системные процедуры включены в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Физически они хранятся во внутренней скрытой базе данных **Resource** . Логически они отображаются в схеме **sys** каждой системной и пользовательской базы данных. В дополнение к этому, база данных **msdb** также содержит системные хранимые процедуры в схеме **dbo** . Эти процедуры используются для планирования предупреждений и заданий. Поскольку названия системных процедур начинаются с префикса **sp_** , этот префикс не рекомендуется использовать при создании пользовательских процедур. Полный список системных хранимых процедур см. в статье [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддерживает системные процедуры, обеспечивающие интерфейс между [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и внешними программами для выполнения различных действий по обслуживанию системы. Эти расширенные процедуры имеют префикс xp_. Полный список расширенных хранимых процедур см. в статье [Основные расширенные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql.md).  
  
 Расширенные пользовательские процедуры  
 Расширенные процедуры позволяют создавать внешние подпрограммы на языках программирования, таких как С. Они представляют собой библиотеки DLL, которые могут динамически загружаться и выполняться экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!NOTE]  
>  Расширенные хранимые процедуры в будущих версиях [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]будут удалены. Не используйте его при работе над новыми приложениями и как можно быстрее измените приложения, в которых он в настоящее время используется. Вместо них рекомендуется создавать процедуры CLR. Этот метод более надежен и безопасен, чем использование расширенных хранимых процедур.  
  
## <a name="related-tasks"></a>Связанные задачи  
  
|||  
|-|-|  
|**Описание задачи**|**Раздел**|  
|Описывает создание хранимой процедуры.|[Создание хранимой процедуры](../../relational-databases/stored-procedures/create-a-stored-procedure.md)|  
|Описывает изменение хранимой процедуры.|[Изменение хранимой процедуры](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)|  
|Описывает удаление хранимой процедуры.|[Удаление хранимой процедуры](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)|  
|Описывает выполнение хранимой процедуры.|[Выполнение хранимой процедуры](../../relational-databases/stored-procedures/execute-a-stored-procedure.md)|  
|Описывает предоставление разрешений на хранимую процедуру.|[Предоставление разрешений на хранимую процедуру](../../relational-databases/stored-procedures/grant-permissions-on-a-stored-procedure.md)|  
|Описывает возврат данных из хранимой процедуры в приложение.|[Возврат данных из хранимой процедуры](../../relational-databases/stored-procedures/return-data-from-a-stored-procedure.md)|  
|Описывает перекомпиляцию хранимой процедуры.|[Перекомпиляция хранимой процедуры](../../relational-databases/stored-procedures/recompile-a-stored-procedure.md)|  
|Описывает переименование хранимой процедуры.|[Изменение имени хранимой процедуры](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)|  
|Описывает просмотр определения хранимой процедуры.|[Просмотр определения хранимой процедуры](../../relational-databases/stored-procedures/view-the-definition-of-a-stored-procedure.md)|  
|Описывает просмотр зависимостей хранимой процедуры.|[Просмотр зависимостей хранимой процедуры](../../relational-databases/stored-procedures/view-the-dependencies-of-a-stored-procedure.md)|  
|Описывается использование параметров в хранимой процедуре.|[Параметры](../../relational-databases/stored-procedures/parameters.md)|  
  
## <a name="related-content"></a>См. также  
 [Хранимые процедуры CLR](https://msdn.microsoft.com/library/bbdd51b2-a9b4-4916-ba6f-7957ac6c3f33)  
  
  

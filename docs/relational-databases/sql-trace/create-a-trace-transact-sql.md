---
title: Создание трассировки (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- traces [SQL Server], example
- traces [SQL Server], creating
ms.assetid: 79dd4254-e3c6-467a-bb6f-f99e51757e99
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 22e393c2758342c0d4a48b8b03ed757d8f95d38c
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68073014"
---
# <a name="create-a-trace-transact-sql"></a>создать трассировку (Transact-SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  В этом подразделе описано, как использовать хранимые процедуры для создания трассировки.  
  
### <a name="to-create-a-trace"></a>Создание трассировки  
  
1.  Выполните хранимую процедуру **sp_trace_create** с необходимыми аргументами для создания новой трассировки. Новая трассировка будет находиться в остановленном состоянии (*status* имеет значение **0**).  
  
2.  Выполните хранимую процедуру **sp_trace_setevent** с необходимыми аргументами, чтобы выбрать события и столбцы для трассировки.  
  
3.  Дополнительно выполните хранимую процедуру **sp_trace_setfilter** , чтобы установить любой фильтр или комбинацию фильтров.  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

     **sp_trace_setevent** and **sp_trace_setfilter** can be executed only on existing traces that are stopped.  
  
    > [!IMPORTANT]  
    >  Unlike regular stored procedures, parameters of all SQL Server Profiler stored procedures (<strong>sp_trace_*xx*</strong>) are strictly typed and do not support automatic data type conversion. If these parameters are not called with the correct input parameter data types, as specified in the argument description, the stored procedure returns an error.  
  
## <a name="example"></a>Пример  
 В следующем примере кода с помощью [!INCLUDE[tsql](../../includes/tsql-md.md)]создается трассировка. Он состоит из трех разделов: создание трассировки, заполнение файла трассировки и остановка трассировки. Настройте трассировку, добавив события, которые необходимо отслеживать. Список событий и столбцов см. в статье [Хранимая процедура sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)создается трассировка.  
  
 Следующий код создает трассировку, добавляет в нее события и затем запускает ее:  
  
```  
DECLARE @RC int, @TraceID int, @on BIT  
EXEC @rc = sp_trace_create @TraceID output, 0, N'C:\SampleTrace'  
  
-- Select the return code to see if the trace creation was successful.  
SELECT RC = @RC, TraceID = @TraceID  
  
-- Set the events and data columns you need to capture.  
SELECT @on = 1  
  
-- 10 is RPC:Completed event. 1 is TextData column.   
EXEC sp_trace_setevent @TraceID, 10, 1, @on   
-- 13 is SQL:BatchStarting, 11 is LoginName  
EXEC sp_trace_setevent @TraceID, 13, 11, @on   
-- 13 is SQL:BatchStarting, 14 is StartTime  
EXEC sp_trace_setevent @TraceID, 13, 14, @on   
-- 12 is SQL:BatchCompleted, 15 is EndTime  
EXEC sp_trace_setevent @TraceID, 12, 15, @on   
-- 13 is SQL:BatchStarting, 1 is TextData  
EXEC sp_trace_setevent @TraceID, 13, 1, @on   
  
-- Set any filter. Not provided in this example  
--EXEC sp_trace_setfilter 1, 10, 0, 6, N'%Profiler%'  
  
-- Start Trace (status 1 = start)  
EXEC @RC = sp_trace_setstatus @TraceID, 1  
GO  
  
```  
  
## <a name="example"></a>Пример  
 Теперь, когда трассировка создана и запущена, выполните следующий код, чтобы заполнить трассировку активностью.  
  
```  
SELECT * FROM master.sys.databases  
GO  
SELECT * FROM ::fn_trace_getinfo(default)  
GO  
  
```  
  
## <a name="example"></a>Пример  
 Трассировку можно в любое время остановить и перезапустить. В этом примере выполнение следующего кода позволит остановить трассировку, закрыть ее и удалить ее определение.  
  
```  
DECLARE @TraceID int  
-- Populate a variable with the trace_id of the current trace  
SELECT  @TraceID = TraceID FROM ::fn_trace_getinfo(default) WHERE VALUE = N'C:\SampleTrace.trc'  
  
-- First stop the trace.   
EXEC sp_trace_setstatus @TraceID, 0  
  
-- Close and then delete its definition from SQL Server.   
EXEC sp_trace_setstatus @TraceID, 2  
  
```  
  
## <a name="example"></a>Пример  
 Чтобы изучить файл трассировки, откройте файл SampleTrace.trc в приложении [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
  
## <a name="see-also"></a>См. также:  
 [Хранимые процедуры приложения SQL Server Profiler (Transact-SQL)](../../relational-databases/system-stored-procedures/sql-server-profiler-stored-procedures-transact-sql.md)   
 [Хранимая процедура sp_trace_create (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-create-transact-sql.md)   
 [Хранимая процедура sp_trace_setevent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)   
 [sp_trace_setfilter (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-trace-setfilter-transact-sql.md)  
  
  

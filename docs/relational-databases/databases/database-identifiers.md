---
title: Идентификаторы баз данных | Документация Майкрософт
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- regular identifiers [SQL Server]
- identifiers [SQL Server]
- names [SQL Server], identifiers
- identifiers [SQL Server], about identifiers
- SQL Server identifiers
- Transact-SQL identifiers
- database objects [SQL Server], names
ms.assetid: 171291bb-f57f-4ad1-8cea-0b092d5d150c
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d71cd82e5fe9af99bb42a6a8971f4d2157612407
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68037559"
---
# <a name="database-identifiers"></a>Идентификаторы баз данных
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  Имя объекта базы данных называется его идентификатором. Идентификаторы в [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] могут присваиваться любым сущностям: серверам, базам данных и их объектам, например таблицам, представлениям, столбцам, индексам, триггерам, процедурам, ограничениям и правилам. Для большинства объектов идентификаторы необходимы, а для некоторых, например ограничений, необязательны.  
  
 Идентификатор объекта создается при определении объекта. Затем идентификатор используется для обращения к объекту. Например, следующая инструкция создает таблицу с идентификатором `TableX`и двумя столбцами с идентификаторами `KeyCol` и `Description`:  
  
```  
CREATE TABLE TableX  
(KeyCol INT PRIMARY KEY, Description nvarchar(80))  
```  
  
 Эта таблица также содержит безымянное ограничение. Ограничение `PRIMARY KEY` не имеет идентификатора.  
  
 Параметры сортировки идентификатора зависят от уровня, для которого определен этот идентификатор. К идентификаторам объектов на уровне экземпляров, таких как имена входа и имена базы данных, применяются параметры сортировки по умолчанию для экземпляра. Идентификаторам объектов в пределах базы данных, например таблиц, представлений или имен столбцов, назначаются параметры сортировки, установленные по умолчанию для базы данных. Например, две таблицы с именами, отличающимися только регистром, могут быть созданы в базе данных с параметрами сортировки c учетом регистра, но не могут быть созданы в базе данных с параметрами сортировки без учета регистра.  
  
> [!NOTE]  
>  Имена переменных или параметров функций и хранимых процедур должны соответствовать правилам для идентификаторов [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
## <a name="classes-of-identifiers"></a>Классы идентификаторов  
 Существует два класса идентификаторов.  
  
 Обычные идентификаторы  
 Соответствуют правилам форматирования идентификаторов. Обычные идентификаторы не разделяются при использовании в инструкциях языка [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
```  
SELECT *  
FROM TableX  
WHERE KeyCol = 124  
```  
  
 Идентификаторы с разделителем  
 Заключаются в двойные кавычки (") или квадратные скобки ([ ]). Идентификаторы, которые соответствуют правилам форматирования идентификаторов, могут быть не разделяемыми. Пример:  
  
```  
SELECT *  
FROM [TableX]         --Delimiter is optional.  
WHERE [KeyCol] = 124  --Delimiter is optional.  
```  
  
 Идентификаторы, которые не соответствуют всем правилам для идентификаторов, в инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)] должны быть разделены. Пример:  
  
```  
SELECT *  
FROM [My Table]      --Identifier contains a space and uses a reserved keyword.  
WHERE [order] = 10   --Identifier is a reserved keyword.  
```  
  
 И обычные идентификаторы, и идентификаторы с разделителями должны содержать от 1 до 128 символов. Для локальных временных таблиц идентификатор может содержать не более 116 символов.  
  
## <a name="rules-for-regular-identifiers"></a>Правила для обычных идентификаторов  
 Имена переменных, функций и хранимых процедур должны соответствовать этим правилам для идентификаторов [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
1.  Первым символом должен быть один из следующих.  
  
    -   Буква в соответствии со стандартом Unicode Standard 3,2. Определения букв в стандарте Юникод включают латинские символы от «a» до «z», от «A» до «Z», а также буквенные символы других языков;  
  
    -   подчеркивание (_), символ @ или решетка (#).  
  
         Определенные символы в начале идентификатора в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]имеют особое значение. Обычный идентификатор, начинающийся символом @, означает локальную переменную или параметр и не может использоваться в качестве имени объекта какого-либо иного типа. Идентификатор, начинающийся символом решетки (#), означает временную таблицу или процедуру. Идентификатор, начинающийся двойным символом решетки (##), означает глобальный временный объект. Хотя символы решетки и двойной решетки могут использоваться в начале имен объектов других типов, мы не рекомендуем такой способ именования.  
  
         Некоторые функции языка [!INCLUDE[tsql](../../includes/tsql-md.md)] имеют имена, начинающиеся с двойного символа «@» (@@). Во избежание путаницы с этими функциями не следует использовать имена, начинающиеся символами @@.  
  
2.  Последующие символы могут включать:  
  
    -   Буквы в соответствии со стандартом Unicode Standard 3,2.  
  
    -   Десятичные цифры из набора символов Basic Latin или другого набора символов национального языка.  
  
    -   символ @, знак доллара ($), решетка или подчеркивание.  
  
3.  Идентификатор не должен быть зарезервированным словом [!INCLUDE[tsql](../../includes/tsql-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] резервирует версии зарезервированных слов как в верхнем, так и в нижнем регистре. Если идентификаторы используются в инструкциях языка [!INCLUDE[tsql](../../includes/tsql-md.md)] , идентификаторы, которые не соответствуют этим правилам, должны быть заключены в двойные кавычки или квадратные скобки. Состав зарезервированных слов зависит от уровня совместимости базы данных. Этот уровень можно установить с помощью инструкции [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md) .  
  
4.  Внутри идентификаторов запрещается использовать символы пробела или специальные символы.  
  
5.  Дополнительные символы недопустимы.  
  
 Если идентификаторы используются в инструкциях языка [!INCLUDE[tsql](../../includes/tsql-md.md)] , идентификаторы, которые не соответствуют этим правилам, должны быть заключены в двойные кавычки или квадратные скобки.  
  
> [!NOTE]  
>  Некоторые правила форматирования обычных идентификаторов зависят от уровня совместимости базы данных. Этот уровень можно установить с помощью процедуры [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
## <a name="see-also"></a>См. также:  
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE DATABASE (SQL Server Transact-SQL)](../../t-sql/statements/create-database-sql-server-transact-sql.md)   
 [CREATE DEFAULT (Transact-SQL)](../../t-sql/statements/create-default-transact-sql.md)   
 [CREATE PROCEDURE (Transact-SQL)](../../t-sql/statements/create-procedure-transact-sql.md)   
 [CREATE RULE (Transact-SQL)](../../t-sql/statements/create-rule-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md)   
 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [Зарезервированные ключевые слова (Transact-SQL)](../../t-sql/language-elements/reserved-keywords-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)  
  
  

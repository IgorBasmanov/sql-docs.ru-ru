---
title: IF...ELSE (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/11/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IF_TSQL
- IF
dev_langs:
- TSQL
helpviewer_keywords:
- IF...ELSE keyword
- ELSE (IF...ELSE) keyword
- ELSE keyword
- IF keyword
ms.assetid: 676c881f-dee1-417a-bc51-55da62398e81
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b0b19216ce352256355da9b2f0c458f49c036773
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68075075"
---
# <a name="ifelse-transact-sql"></a>IF...ELSE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Задает условия для выполнения инструкции [!INCLUDE[tsql](../../includes/tsql-md.md)]. Инструкция языка [!INCLUDE[tsql](../../includes/tsql-md.md)], следующая за ключевым словом IF и его условием, выполняется только в том случае, если логическое выражение возвращает TRUE. Необязательное ключевое слово ELSE представляет другую инструкцию языка [!INCLUDE[tsql](../../includes/tsql-md.md)], которая выполняется, если условие IF не удовлетворяется и логическое выражение возвращает FALSE.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
IF Boolean_expression   
     { sql_statement | statement_block }   
[ ELSE   
     { sql_statement | statement_block } ]   
```  
  
## <a name="arguments"></a>Аргументы  
 *Boolean_expression*  
 Выражение, возвращающее значение TRUE или FALSE. Если логическое выражение содержит инструкцию SELECT, инструкция SELECT должна быть заключена в скобки.  
  
 { *sql_statement*| *statement_block* }  
 Любая инструкция или группа инструкций языка [!INCLUDE[tsql](../../includes/tsql-md.md)], указанная с помощью блока инструкций. Без использования блока инструкций условия IF и ELSE могут повлиять на выполнение только одной инструкции языка [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Для определения блока инструкций используйте ключевые слова потока управления BEGIN и END.  
  
## <a name="remarks"></a>Remarks  
 Конструкция IF...ELSE может быть использована в пакетах, хранимых процедурах и нерегламентированных запросах. При использовании в хранимой процедуре эта конструкция часто применяется для проверки существования некоторого параметра.  
  
 Проверки IF могут находиться внутри другого IF или следующего ELSE. Ограничение количества вложенных уровней зависит от свободной памяти.  
  
## <a name="example"></a>Пример  
  
```  
IF DATENAME(weekday, GETDATE()) IN (N'Saturday', N'Sunday')
       SELECT 'Weekend';
ELSE 
       SELECT 'Weekday';
```  
  
 Дополнительные сведения см. в разделе [ELSE (IF...ELSE) (Transact-SQL)](../../t-sql/language-elements/else-if-else-transact-sql.md).  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 В следующем примере используется `IF...ELSE` для определения того, какой из двух ответов показать пользователю, на основе веса элемента в таблице `DimProduct`.  
  
```  
-- Uses AdventureWorksDW  
  
DECLARE @maxWeight float, @productKey integer  
SET @maxWeight = 100.00  
SET @productKey = 424  
IF @maxWeight <= (SELECT Weight from DimProduct 
                  WHERE ProductKey = @productKey)   
    (SELECT @productKey AS ProductKey, EnglishDescription, Weight, 
    'This product is too heavy to ship and is only available for pickup.' 
        AS ShippingStatus
    FROM DimProduct WHERE ProductKey = @productKey);  
ELSE  
    (SELECT @productKey AS ProductKey, EnglishDescription, Weight, 
    'This product is available for shipping or pickup.' 
        AS ShippingStatus
    FROM DimProduct WHERE ProductKey = @productKey);  
```  
  
## <a name="see-also"></a>См. также:  
 [BEGIN...END &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-end-transact-sql.md)   
 [END (BEGIN...END) (Transact-SQL)](../../t-sql/language-elements/end-begin-end-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [WHILE (Transact-SQL)](../../t-sql/language-elements/while-transact-sql.md)   
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)   
 [Язык управления потоком (Transact-SQL)](~/t-sql/language-elements/control-of-flow.md) [ELSE (IF...ELSE) (Transact-SQL)](../../t-sql/language-elements/else-if-else-transact-sql.md) 
  
  




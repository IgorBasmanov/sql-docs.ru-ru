---
title: Предложение OVER (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- OVER_TSQL
- OVER
dev_langs:
- TSQL
helpviewer_keywords:
- order of rowsets [SQL Server]
- rowsets [SQL Server], windowing
- window function
- partitions [SQL Server], rowsets
- clauses [SQL Server], OVER
- rowsets [SQL Server], partitioning
- rowsets [SQL Server], ordering
- OVER clause
ms.assetid: ddcef3a6-0341-43e0-ae73-630484b7b398
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4ac5c9ff7d2fbb5d32b559e6225dba4e7bbb7a48
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67948327"
---
# <a name="select---over-clause-transact-sql"></a>SELECT — предложение OVER (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Определяет секционирование и упорядочение набора строк до применения соответствующей оконной функции. То есть предложение OVER определяет окно или определяемый пользователем набор строк внутри результирующего набора запроса. Затем оконная функция вычисляет значение для каждой строки в окне. Вы можете использовать предложение OVER вместе с функциями для вычисления статистических значений, например для вычисления скользящих средних, суммарных статистических выражений, промежуточных итогов или первых N результатов в группе.  
  
-   [Ранжирующие функции](../../t-sql/functions/ranking-functions-transact-sql.md)  
  
-   [Агрегатные функции](../../t-sql/functions/aggregate-functions-transact-sql.md)  
  
-   [Аналитические функции](../../t-sql/functions/analytic-functions-transact-sql.md)  
  
-   [Функция NEXT VALUE FOR](../../t-sql/functions/next-value-for-transact-sql.md)  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
-- Syntax for SQL Server, Azure SQL Database, and Azure SQL Data Warehouse  
  
OVER (   
       [ <PARTITION BY clause> ]  
       [ <ORDER BY clause> ]   
       [ <ROW or RANGE clause> ]  
      )  
  
<PARTITION BY clause> ::=  
PARTITION BY value_expression , ... [ n ]  
  
<ORDER BY clause> ::=  
ORDER BY order_by_expression  
    [ COLLATE collation_name ]   
    [ ASC | DESC ]   
    [ ,...n ]  
  
<ROW or RANGE clause> ::=  
{ ROWS | RANGE } <window frame extent>  
  
<window frame extent> ::=   
{   <window frame preceding>  
  | <window frame between>  
}  
<window frame between> ::=   
  BETWEEN <window frame bound> AND <window frame bound>  
  
<window frame bound> ::=   
{   <window frame preceding>  
  | <window frame following>  
}  
  
<window frame preceding> ::=   
{  
    UNBOUNDED PRECEDING  
  | <unsigned_value_specification> PRECEDING  
  | CURRENT ROW  
}  
  
<window frame following> ::=   
{  
    UNBOUNDED FOLLOWING  
  | <unsigned_value_specification> FOLLOWING  
  | CURRENT ROW  
}  
  
<unsigned value specification> ::=   
{  <unsigned integer literal> }  
  
```  
  
```  
-- Syntax for Parallel Data Warehouse  
  
OVER ( [ PARTITION BY value_expression ] [ order_by_clause ] )  
```  
  
## <a name="arguments"></a>Аргументы  
 PARTITION BY  
 Разделяет результирующий набор запроса на секции. Оконная функция применяется к каждой секции отдельно, и вычисление начинается заново для каждой секции.  
  
 *value_expression*  
 Определяет столбец, по которому секционируется набор строк. Аргумент *value_expression* может ссылаться только на столбцы, сделанные доступными с помощью предложения FROM. Аргумент *value_expression* не может ссылаться на выражения или псевдонимы в списке выбора. Выражение *value_expression* может быть выражением столбца, скалярным вложенным запросом, скалярной функцией или пользовательской переменной.  
  
 \<Предложение ORDER BY>  
 Определяет логический порядок строк в каждой секции результирующего набора. То есть он указывает логический порядок, в котором выполняется вычисление оконной функции.  
  
 *order_by_expression*  
 Указывает столбец или выражение, по которому производится сортировка. Аргумент *order_by_expression* может ссылаться только на столбцы, сделанные доступными с помощью предложения FROM. Нельзя указывать целое число для обозначения имени или псевдонима столбца.  
  
 COLLATE *collation_name*  
 Указывает, что операция ORDER BY должна выполняться в соответствии с параметрами сортировки, указанными в аргументе *collation_name*. Аргументом *collation_name* может быть либо имя параметров сортировки Windows, либо имя параметров сортировки SQL. Дополнительные сведения см. в статье [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md). Аргумент COLLATE применяется только к столбцам типа **char**, **varchar**, **nchar** и **nvarchar**.  
  
 **ASC** | DESC  
 Указывает порядок сортировки значений в указанном столбце — по возрастанию или по убыванию. Порядок сортировки по умолчанию — ASC. Значения NULL рассматриваются как минимально возможные значения.  
  
 ROWS | RANGE  
**Применимо к**: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. 
  
 Еще больше ограничивает строки в пределах секции, указывая начальную и конечную точки. Это достигается путем указания диапазона строк в отношении текущей строки с помощью логических или физических взаимосвязей. Физическая взаимосвязь достигается с помощью предложения ROWS.  
  
 Предложение ROWS ограничивает строки внутри секции путем указания фиксированного числа строк, предшествующих или следующих после текущей строки. В качестве альтернативы предложение RANGE логически ограничивает строки внутри секции путем указания диапазона значений в отношении к значению текущей строки. Предшествующие и последующие строки определяются на основании порядка, заданного в предложении ORDER BY. Рамка окна "RANGE … CURRENT ROW ..." содержит все строки, которые имеют те же значения в выражении ORDER BY, что и в текущей строке. Например, ROWS BETWEEN 2 PRECEDING AND CURRENT ROW означает, что окно строк, с которым работает функция, содержит всего три строки, при этом текущей строке предшествуют 2 строки (включая текущую).  
  
> [!NOTE]  
>  Предложения ROWS и RANGE требуют, чтобы было указано предложение ORDER BY. Если предложение ORDER BY содержит несколько выражений порядка, то CURRENT ROW FOR RANGE при определении текущей строки учитывает все столбцы в списке ORDER BY.  
  
 UNBOUNDED PRECEDING  
**Применимо к**: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Указывает, что окно начинается с первой строки секции. UNBOUNDED PRECEDING может быть указано только как начальная точка окна.  
  
 \<спецификация неподписанного значения> PRECEDING  
 Указывается со \<спецификацией неподписанного значения> для обозначения числа строк или значений перед текущей строкой. Эта спецификация не допускается в предложении RANGE.  
  
 CURRENT ROW  
**Применимо к**: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. 
  
 Указывает, что окно начинается или заканчивается на текущей строке при использовании совместно с предложением ROWS или что окно заканчивается на текущем значении при использовании с предложением RANGE. CURRENT ROW может быть задана и как начальная, и как конечная точка.  
  
 BETWEEN \<граница рамки окна > AND \<граница рамки окна >  
**Применимо к**: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. 
  
 Используется совместно с предложением ROWS или RANGE для указания нижней (начальной) или верхней (конечной) граничной точки окна. \<граница рамки окна> определяет граничную начальную точку, а \<граница рамки окна> определяет граничную конечную точку. Верхняя граница не может быть меньше нижней границы.  
  
 UNBOUNDED FOLLOWING  
**Применимо к**: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. 
  
 Указывает, что окно заканчивается на последней строке секции. UNBOUNDED FOLLOWING может быть указано только как конечная точка окна. Например, RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING определяет, что окно начинается на текущей строке и заканчивается на последней строке секции.  
  
 \<спецификация неподписанного значения> FOLLOWING  
 Указывается со \<спецификацией неподписанного значения> для обозначения числа строк или значений после текущей строки. При указании \<спецификации неподписанного значения> FOLLOWING как начальной точки окна конечная точка должна быть \<спецификацией неподписанного значения>FOLLOWING. Например, ROWS BETWEEN 2 FOLLOWING AND 10 FOLLOWING определяет, что окно начинается на второй строке после текущей и заканчивается на десятой строке после текущей строки. Эта спецификация не допускается в предложении RANGE.  
  
 неподписанный целочисленный литерал  
**Применимо к**: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Положительный целочисленный литерал (включая 0), который указывает число строк или значений перед или после текущей строки или значения. Эта спецификация является допустимой только в предложении ROWS.  
  
## <a name="general-remarks"></a>Общие замечания  
 В одном запросе с одним предложением FROM может использоваться несколько оконных функций. Предложение OVER для каждой функции может отличаться в части секционирования и упорядочения.  
  
 Если PARTITION BY не указан, функция обрабатывает все строки результирующего набора запроса как одну группу. 
 
### <a name="important"></a>Важно!

Если указано предложение ROWS или RANGE и используется \<предшествующая рамка окна> для \<экстента рамки окна> (короткий синтаксис), то данная спецификация используется в качестве начальной точки границы рамки окна, а CURRENT ROW — в качестве конечной точки границы окна. Например "ROWS 5 PRECEDING" равно "ROWS BETWEEN 5 PRECEDING AND CURRENT ROW".  
  
> [!NOTE]
> Если предложение ORDER BY не указано, то для рамки окна используется весь раздел. Это относится только к тем функциям, которым не требуется предложение ORDER BY. Если предложение ROWS или RANGE не указаны, а указано предложение ORDER BY, то в качестве значения по умолчанию для рамки окна используется RANGE UNBOUNDED PRECEDING AND CURRENT ROW. Это относится только к тем функциям, которые могут принимать дополнительную спецификацию ROWS или RANGE. Например, ранжирующая функция не может принимать предложение ROWS или RANGE, поэтому данная рамка окна не может использоваться, даже несмотря на наличие предложения ORDER BY, а предложение ROWS или RANGE отсутствует.  
    
## <a name="limitations-and-restrictions"></a>Ограничения  
 Предложение OVER не может использоваться с агрегатной функцией CHECKSUM.  
  
 Предложение RANGE не может использоваться со \<спецификацией неподписанного значения> PRECEDING или со \<спецификацией неподписанного значения> FOLLOWING.  
  
 В зависимости от используемой функции (ранжирующая, агрегатная или аналитическая) с предложением OVER, \<предложение ORDER BY> и (или) \<предложения ROWS и RANGE> могут не поддерживаться.  
  
## <a name="examples"></a>Примеры  
  
### <a name="a-using-the-over-clause-with-the-rownumber-function"></a>A. Использование предложения OVER с функцией ROW_NUMBER  
 Следующий пример демонстрирует использование предложения OVER с функцией ROW_NUMBER для отображения номера каждой строки в секции. Предложение ORDER BY, указанное в предложении OVER упорядочивает строки каждой секции по столбцу `SalesYTD`. Предложение ORDER BY в инструкции SELECT определяет порядок, в котором возвращается весь результирующий набор запроса.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ROW_NUMBER() OVER(PARTITION BY PostalCode ORDER BY SalesYTD DESC) AS "Row Number",   
    p.LastName, s.SalesYTD, a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0  
ORDER BY PostalCode;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Row Number      LastName                SalesYTD              PostalCode 
 --------------- ----------------------- --------------------- ---------- 
 1               Mitchell                4251368.5497          98027 
 2               Blythe                  3763178.1787          98027 
 3               Carson                  3189418.3662          98027 
 4               Reiter                  2315185.611           98027 
 5               Vargas                  1453719.4653          98027  
 6               Ansman-Wolfe            1352577.1325          98027  
 1               Pak                     4116871.2277          98055  
 2               Varkey Chudukatil       3121616.3202          98055  
 3               Saraiva                 2604540.7172          98055  
 4               Ito                     2458535.6169          98055  
 5               Valdez                  1827066.7118          98055  
 6               Mensa-Annan             1576562.1966          98055  
 7               Campbell                1573012.9383          98055  
 8               Tsoflias                1421810.9242          98055
 ```  
  
### <a name="b-using-the-over-clause-with-aggregate-functions"></a>Б. Использование предложения OVER с агрегатными функциями  
 В следующем примере предложение `OVER` используется с агрегатной функцией для всех возвращаемых запросом строк. В данном примере использование предложения `OVER` является более эффективным, чем использование вложенных запросов для получения статистических значений.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT SalesOrderID, ProductID, OrderQty  
    ,SUM(OrderQty) OVER(PARTITION BY SalesOrderID) AS Total  
    ,AVG(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Avg"  
    ,COUNT(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Count"  
    ,MIN(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Min"  
    ,MAX(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Max"  
FROM Sales.SalesOrderDetail   
WHERE SalesOrderID IN(43659,43664);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
SalesOrderID ProductID   OrderQty Total       Avg         Count       Min    Max  
------------ ----------- -------- ----------- ----------- ----------- ------ ------  
43659        776         1        26          2           12          1      6  
43659        777         3        26          2           12          1      6  
43659        778         1        26          2           12          1      6  
43659        771         1        26          2           12          1      6  
43659        772         1        26          2           12          1      6  
43659        773         2        26          2           12          1      6  
43659        774         1        26          2           12          1      6  
43659        714         3        26          2           12          1      6  
43659        716         1        26          2           12          1      6  
43659        709         6        26          2           12          1      6  
43659        712         2        26          2           12          1      6  
43659        711         4        26          2           12          1      6  
43664        772         1        14          1           8           1      4  
43664        775         4        14          1           8           1      4  
43664        714         1        14          1           8           1      4  
43664        716         1        14          1           8           1      4  
43664        777         2        14          1           8           1      4  
43664        771         3        14          1           8           1      4  
43664        773         1        14          1           8           1      4  
43664        778         1        14          1           8           1      4  
```  
  
 Следующий пример демонстрирует использование предложения `OVER` с агрегатной функцией в вычисляемом значении.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT SalesOrderID, ProductID, OrderQty  
    ,SUM(OrderQty) OVER(PARTITION BY SalesOrderID) AS Total  
    ,CAST(1. * OrderQty / SUM(OrderQty) OVER(PARTITION BY SalesOrderID)   
        *100 AS DECIMAL(5,2))AS "Percent by ProductID"  
FROM Sales.SalesOrderDetail   
WHERE SalesOrderID IN(43659,43664);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Обратите внимание, что статистические функции вычисляются в столбце `SalesOrderID`, а столбец `Percent by ProductID` вычисляется для каждой строки каждого `SalesOrderID`.  
  
```  
SalesOrderID ProductID   OrderQty Total       Percent by ProductID  
------------ ----------- -------- ----------- ---------------------------------------  
43659        776         1        26          3.85  
43659        777         3        26          11.54  
43659        778         1        26          3.85  
43659        771         1        26          3.85  
43659        772         1        26          3.85  
43659        773         2        26          7.69  
43659        774         1        26          3.85  
43659        714         3        26          11.54  
43659        716         1        26          3.85  
43659        709         6        26          23.08  
43659        712         2        26          7.69  
43659        711         4        26          15.38  
43664        772         1        14          7.14  
43664        775         4        14          28.57  
43664        714         1        14          7.14  
43664        716         1        14          7.14  
43664        777         2        14          14.29  
43664        771         3        14          21.4  
43664        773         1        14          7.14  
43664        778         1        14          7.14  
  
 (20 row(s) affected)  
```  
  
### <a name="c-producing-a-moving-average-and-cumulative-total"></a>В. Нахождение скользящей средней и кумулятивной суммы  
 В следующем примере показано использование функций AVG и SUM с предложением OVER для вычисления скользящей средней и кумулятивной суммы годовых продаж по каждой территории, указанной в таблице `Sales.SalesPerson`. Данные секционируются по `TerritoryID` и логически сортируются по `SalesYTD`. Это означает, что функция AVG вычисляется для каждой территории на основании объема продаж за год. Обратите внимание, что в `TerritoryID` 1 для продаж за 2005 год используются две строки, в которых представлены два менеджера по продажам с показателями за этот год. После расчета среднего значения продаж для двух данных строк в вычисление включается третья строка, представляющая продажи за 2006 год.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT BusinessEntityID, TerritoryID   
   ,DATEPART(yy,ModifiedDate) AS SalesYear  
   ,CONVERT(varchar(20),SalesYTD,1) AS  SalesYTD  
   ,CONVERT(varchar(20),AVG(SalesYTD) OVER (PARTITION BY TerritoryID   
                                            ORDER BY DATEPART(yy,ModifiedDate)   
                                           ),1) AS MovingAvg  
   ,CONVERT(varchar(20),SUM(SalesYTD) OVER (PARTITION BY TerritoryID   
                                            ORDER BY DATEPART(yy,ModifiedDate)   
                                            ),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5  
ORDER BY TerritoryID,SalesYear;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYear   SalesYTD             MovingAvg            CumulativeTotal  
---------------- ----------- ----------- -------------------- -------------------- --------------------  
274              NULL        2005        559,697.56           559,697.56           559,697.56  
287              NULL        2006        519,905.93           539,801.75           1,079,603.50  
285              NULL        2007        172,524.45           417,375.98           1,252,127.95  
283              1           2005        1,573,012.94         1,462,795.04         2,925,590.07  
280              1           2005        1,352,577.13         1,462,795.04         2,925,590.07  
284              1           2006        1,576,562.20         1,500,717.42         4,502,152.27  
275              2           2005        3,763,178.18         3,763,178.18         3,763,178.18  
277              3           2005        3,189,418.37         3,189,418.37         3,189,418.37  
276              4           2005        4,251,368.55         3,354,952.08         6,709,904.17  
281              4           2005        2,458,535.62         3,354,952.08         6,709,904.17  
  
(10 row(s) affected)  
  
```  
  
 В этом примере предложение OVER не включает в себя предложение PARTITION BY. Это означает, что функция будет применяться для всех строк, возвращаемых запросом. Предложение ORDER BY, указанное в предложении OVER, определяет логический порядок применения функции AVG. Запрос возвращает скользящее среднее значение продаж за год для всех территорий, указанных в предложении WHERE. Предложение ORDER BY, указанное в инструкции SELECT, определяет порядок отображения строк запроса.  
  
```sql  
SELECT BusinessEntityID, TerritoryID   
   ,DATEPART(yy,ModifiedDate) AS SalesYear  
   ,CONVERT(varchar(20),SalesYTD,1) AS  SalesYTD  
   ,CONVERT(varchar(20),AVG(SalesYTD) OVER (ORDER BY DATEPART(yy,ModifiedDate)   
                                            ),1) AS MovingAvg  
   ,CONVERT(varchar(20),SUM(SalesYTD) OVER (ORDER BY DATEPART(yy,ModifiedDate)   
                                            ),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5  
ORDER BY SalesYear;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYear   SalesYTD             MovingAvg            CumulativeTotal  
---------------- ----------- ----------- -------------------- -------------------- --------------------  
274              NULL        2005        559,697.56           2,449,684.05         17,147,788.35  
275              2           2005        3,763,178.18         2,449,684.05         17,147,788.35  
276              4           2005        4,251,368.55         2,449,684.05         17,147,788.35  
277              3           2005        3,189,418.37         2,449,684.05         17,147,788.35  
280              1           2005        1,352,577.13         2,449,684.05         17,147,788.35  
281              4           2005        2,458,535.62         2,449,684.05         17,147,788.35  
283              1           2005        1,573,012.94         2,449,684.05         17,147,788.35  
284              1           2006        1,576,562.20         2,138,250.72         19,244,256.47  
287              NULL        2006        519,905.93           2,138,250.72         19,244,256.47  
285              NULL        2007        172,524.45           1,941,678.09         19,416,780.93  
(10 row(s) affected)  
```  
  
### <a name="d-specifying-the-rows-clause"></a>Г. Указание предложения ROWS  
  
**Применимо к**: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 В следующем примере с помощью предложения ROWS определяется окно, в рамках которого вычисляется текущая строка, а также *N* последующих строк (1 строка в данном примере).  
  
```sql  
SELECT BusinessEntityID, TerritoryID   
    ,CONVERT(varchar(20),SalesYTD,1) AS  SalesYTD  
    ,DATEPART(yy,ModifiedDate) AS SalesYear  
    ,CONVERT(varchar(20),SUM(SalesYTD) OVER (PARTITION BY TerritoryID   
                                             ORDER BY DATEPART(yy,ModifiedDate)   
                                             ROWS BETWEEN CURRENT ROW AND 1 FOLLOWING ),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYTD             SalesYear   CumulativeTotal  
---------------- ----------- -------------------- ----------- --------------------  
274              NULL        559,697.56           2005        1,079,603.50  
287              NULL        519,905.93           2006        692,430.38  
285              NULL        172,524.45           2007        172,524.45  
283              1           1,573,012.94         2005        2,925,590.07  
280              1           1,352,577.13         2005        2,929,139.33  
284              1           1,576,562.20         2006        1,576,562.20  
275              2           3,763,178.18         2005        3,763,178.18  
277              3           3,189,418.37         2005        3,189,418.37  
276              4           4,251,368.55         2005        6,709,904.17  
281              4           2,458,535.62         2005        2,458,535.62  
```  
  
 В следующем примере предложение ROWS указывается с UNBOUNDED PRECEDING. В результате окно начинается с первой строки секции.  
  
```sql  
SELECT BusinessEntityID, TerritoryID   
    ,CONVERT(varchar(20),SalesYTD,1) AS  SalesYTD  
    ,DATEPART(yy,ModifiedDate) AS SalesYear  
    ,CONVERT(varchar(20),SUM(SalesYTD) OVER (PARTITION BY TerritoryID   
                                             ORDER BY DATEPART(yy,ModifiedDate)   
                                             ROWS UNBOUNDED PRECEDING),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYTD             SalesYear   CumulativeTotal  
---------------- ----------- -------------------- ----------- --------------------  
274              NULL        559,697.56           2005        559,697.56  
287              NULL        519,905.93           2006        1,079,603.50  
285              NULL        172,524.45           2007        1,252,127.95  
283              1           1,573,012.94         2005        1,573,012.94  
280              1           1,352,577.13         2005        2,925,590.07  
284              1           1,576,562.20         2006        4,502,152.27  
275              2           3,763,178.18         2005        3,763,178.18  
277              3           3,189,418.37         2005        3,189,418.37  
276              4           4,251,368.55         2005        4,251,368.55  
281              4           2,458,535.62         2005        6,709,904.17  
  
```  
  
## <a name="examples-includesspdwincludessspdw-mdmd"></a>Примеры: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-the-over-clause-with-the-rownumber-function"></a>Д. Использование предложения OVER с функцией ROW_NUMBER  
 В следующем примере возвращается ROW_NUMBER для торговых представителей в зависимости от установленной для них квоты продаж.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ROW_NUMBER() OVER(ORDER BY SUM(SalesAmountQuota) DESC) AS RowNumber,  
    FirstName, LastName,   
CONVERT(varchar(13), SUM(SalesAmountQuota),1) AS SalesQuota   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq  
    ON e.EmployeeKey = sq.EmployeeKey  
WHERE e.SalesPersonFlag = 1  
GROUP BY LastName, FirstName;  
```  
  
 Здесь приводится частичный результирующий набор.  
  
 ```
 RowNumber  FirstName  LastName            SalesQuota  
 ---------  ---------  ------------------  -------------  
 1          Jillian    Carson              12,198,000.00  
 2          Linda      Mitchell            11,786,000.00  
 3          Michael    Blythe              11,162,000.00  
 4          Jae        Pak                 10,514,000.00  
 ```
 
### <a name="f-using-the-over-clause-with-aggregate-functions"></a>Е. Использование предложения OVER с агрегатными функциями  
 Следующие примеры демонстрируют использование предложения OVER с агрегатными функциями. В данном примере использование предложения OVER является более эффективным, чем использование вложенных запросов.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT SalesOrderNumber AS OrderNumber, ProductKey,   
       OrderQuantity AS Qty,   
       SUM(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Total,  
       AVG(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Avg,  
       COUNT(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Count,  
       MIN(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Min,  
       MAX(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Max  
FROM dbo.FactResellerSales   
WHERE SalesOrderNumber IN(N'SO43659',N'SO43664') AND  
      ProductKey LIKE '2%'  
ORDER BY SalesOrderNumber,ProductKey;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 OrderNumber  Product  Qty  Total  Avg  Count  Min  Max  
 -----------  -------  ---  -----  ---  -----  ---  ---  
 SO43659      218      6    16     3    5      1    6  
 SO43659      220      4    16     3    5      1    6  
 SO43659      223      2    16     3    5      1    6  
 SO43659      229      3    16     3    5      1    6  
 SO43659      235      1    16     3    5      1    6  
 SO43664      229      1     2     1    2      1    1  
 SO43664      235      1     2     1    2      1    1  
 ```
 
 Следующий пример демонстрирует использование предложения OVER с агрегатной функцией в вычисляемом значении. Обратите внимание, что статистические выражения вычисляются в столбце `SalesOrderNumber`, а процент от общего числа заказов на продажу вычисляется для каждой строки каждого `SalesOrderNumber`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT SalesOrderNumber AS OrderNumber, ProductKey AS Product,   
       OrderQuantity AS Qty,   
       SUM(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Total,  
       CAST(1. * OrderQuantity / SUM(OrderQuantity)   
        OVER(PARTITION BY SalesOrderNumber)   
            *100 AS DECIMAL(5,2)) AS PctByProduct  
FROM dbo.FactResellerSales   
WHERE SalesOrderNumber IN(N'SO43659',N'SO43664') AND  
      ProductKey LIKE '2%'  
ORDER BY SalesOrderNumber,ProductKey;  
```  
  
 Первый запуск этого результирующего набора:  
  
 ```
 OrderNumber  Product  Qty  Total  PctByProduct  
 -----------  -------  ---  -----  ------------  
 SO43659      218      6    16     37.50  
 SO43659      220      4    16     25.00  
 SO43659      223      2    16     12.50  
 SO43659      229      2    16     18.75  
 ```
 
## <a name="see-also"></a>См. также:  
 [Агрегатные функции (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md)   
 [Аналитические функции (Transact-SQL)](../../t-sql/functions/analytic-functions-transact-sql.md)   
 [Полезная запись блога о функциях окна и предложении OVER на сайте sqlmag.com. Автор: Ицик Бег-Ган (Itzik Ben-Gan)](https://www.itprotoday.com/sql-server/how-use-microsoft-sql-server-2012s-window-functions-part-1)  
  
  

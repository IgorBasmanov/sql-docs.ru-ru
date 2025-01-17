---
title: CREATE FUNCTION (хранилище данных SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 8cad1b2c-5ea0-4001-9060-2f6832ccd057
author: CarlRabeler
ms.author: carlrab
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 23949aec32acce44cd139ab8505cd1ffc743e64d
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67912707"
---
# <a name="create-function-sql-data-warehouse"></a>CREATE FUNCTION (хранилище данных SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Создает определяемую пользователем функцию в [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Определяемая пользователем функция представляет собой подпрограмму [!INCLUDE[tsql](../../includes/tsql-md.md)], которая принимает параметры, выполняет действие, например сложное вычисление, а затем возвращает результат этого действия в виде значения. Возвращаемое значение должно быть скалярным (единичным). При помощи этой инструкции можно создать подпрограмму, которую можно повторно использовать следующими способами.  
  
-   В инструкциях [!INCLUDE[tsql](../../includes/tsql-md.md)], например SELECT.  
  
-   В приложениях, вызывающих функцию.  
  
-   В определении другой пользовательской функции.  
  
-   Для определения ограничения CHECK на столбец.  
  
-   Для замены хранимой процедуры.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
--Transact-SQL Scalar Function Syntax  
CREATE FUNCTION [ schema_name. ] function_name   
( [ { @parameter_name [ AS ] parameter_data_type   
    [ = default ] }   
    [ ,...n ]  
  ]  
)  
RETURNS return_data_type  
    [ WITH <function_option> [ ,...n ] ]  
    [ AS ]  
    BEGIN   
        function_body   
        RETURN scalar_expression  
    END  
[ ; ]  
  
<function_option>::=   
{  
    [ SCHEMABINDING ]  
  | [ RETURNS NULL ON NULL INPUT | CALLED ON NULL INPUT ]  
}  
  
```  
  
## <a name="arguments"></a>Аргументы  
 *schema_name*  
 Имя схемы, к которой принадлежит определяемая пользователем функция.  
  
 *function_name*  
 Имя определяемой пользователем функции. Имена функций должны удовлетворять правилам построения идентификаторов и быть уникальными в пределах базы данных и схемы.  
  
> [!NOTE]  
>  Даже при отсутствии аргументов скобки после имени функции обязательны.  
  
 @*parameter_name*  
 Аргумент пользовательской функции. Может быть объявлен один или несколько аргументов.  
  
 Для функций допускается не более 2 100 параметров. При выполнении функции значение каждого из объявленных параметров должно быть указано пользователем, если для них не определены значения по умолчанию.  
  
 Определяет имя параметра, используя знак @ как первый символ. Имя параметра должно соответствовать правилам для идентификаторов. Параметры являются локальными в пределах функции, в разных функциях могут быть использованы одинаковые имена параметров. Аргументы могут использоваться только вместо констант. Они не могут использоваться вместо имен таблиц, имен столбцов или имен других объектов базы данных.  
  
> [!NOTE]  
>  Параметры ANSI_WARNINGS не годятся для передачи в хранимые процедуры, пользовательские функции и при объявлении и установке переменных в пакетных инструкциях. Например, если объявить переменную как **char(3)** , а затем присвоить ей значение длиннее трех символов, данные будут усечены до размера переменной, а инструкция INSERT или UPDATE завершится без ошибок.  
  
 *parameter_data_type*  
 Тип данных параметра. Для функций [!INCLUDE[tsql](../../includes/tsql-md.md)] допускаются все скалярные типы данных, которые поддерживаются в [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Тип данных timestamp (rowversion) не поддерживается.  
  
 [ =*default* ]  
 Значение по умолчанию для аргумента. Если определено значение *default*, то функция выполняется даже в том случае, если для данного параметра значение не указано.  
  
 Если параметр функции имеет значение по умолчанию, то для него должно быть указано ключевое слово DEFAULT для получения функцией значения по умолчанию. Применение ключевого слова DEFAULT следует отличать от использования аргументов со значениями по умолчанию в хранимых процедурах, когда не указанный аргумент неявно принимает значение по умолчанию.  
  
 *return_data_type*  
 Возвращаемое значение скалярной пользовательской функции. Для функций [!INCLUDE[tsql](../../includes/tsql-md.md)] допускаются все скалярные типы данных, которые поддерживаются в [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Тип данных timestamp (rowversion) не поддерживается. Нескалярные типы курсора и таблицы не допускаются.  
  
 *function_body*  
 Ряд инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)].  Аргумент function_body не может содержать инструкцию SELECT и ссылаться на данные базы данных.  Аргумент function_body не может ссылаться на таблицы или представления. Аргумент function_body может вызывать другие детерминированные функции, но не может вызывать недетерминированные. 
  
 Для скалярных функций *function_body* представляет собой ряд инструкций [!INCLUDE[tsql](../../includes/tsql-md.md)], которые в совокупности вычисляют скалярное выражение.  
  
 *scalar_expression*  
 Указывает скалярное значение, возвращаемое скалярной функцией.  
  
 **\<function_option>::=** 
  
 Указывает, что функция будет иметь один или несколько из следующих параметров.  
  
 SCHEMABINDING  
 Указывает, что функция привязана к объектам базы данных, которые содержат ссылки на нее. Если аргумент SCHEMABINDING указан, нельзя изменить базовые объекты таким способом, который может повлиять на определение функции. Сначала нужно изменить или удалить само определение функции, чтобы удалить зависимости от объекта, который требуется изменить.  
  
 Привязка функции к ссылающимся на нее объектам удаляется в следующих случаях:  
  
-   При удалении функции.  
  
-   При изменении функции инструкцией ALTER, если не указан параметр SCHEMABINDING.  
  
 Функция может быть привязана к схеме только в том случае, если выполняются следующие условия.  
  
-   Любые пользовательские функции, на которые ссылается данная функция, также привязаны к схеме.  
  
-   Функции и другие определяемые пользователем функции, на которые ссылается данная функция, указываются как одно- или двухкомпонентное имя.  
  
-   В теле определяемых пользователем функций может содержаться ссылка только на встроенные функции и другие определяемые пользователем функции в той же базе данных.  
  
-   Пользователь, выполняющий инструкцию CREATE FUNCTION, имеет разрешение REFERENCES на объекты базы данных, на которые ссылается функция.  
  
 Чтобы удалить SCHEMABINDING, используйте ALTER  
  
 RETURNS NULL ON NULL INPUT | **CALLED ON NULL INPUT**  
 Указывает атрибут **OnNULLCall** скалярной функции. Если данный аргумент не указан, по умолчанию предполагается CALLED ON NULL INPUT. Это означает, что текст функции выполняется даже в том случае, если в качестве аргумента передано значение NULL.  
  
## <a name="best-practices"></a>Рекомендации  
 Если определяемая пользователем функция создан без применения предложения SCHEMABINDING, то изменения базовых объектов могут повлиять на определение функции и привести к непредвиденным результатам при вызове функции. Рекомендуется реализовать один из следующих методов, чтобы обеспечить, что функция не устареет из-за изменения ее базовых объектов.  
  
-   Укажите при создании функции предложение WITH SCHEMABINDING. Это обеспечит невозможность изменения объектов, на которые ссылается определение функции, если при этом не изменяется сама функция.  
  
## <a name="interoperability"></a>Совместимость  
 В функциях допустимы следующие инструкции.  
  
-   Инструкции присваивания.  
  
-   Инструкции управления потоком, за исключением инструкций TRY...CATCH.  
  
-   Инструкции DECLARE, объявляющие локальные переменные.  
  
## <a name="limitations-and-restrictions"></a>Ограничения  
 Определяемые пользователем функции не могут выполнять действия, изменяющие состояние базы данных.  
  
 Определяемые пользователем функции могут быть вложенными, то есть из одной функции может быть вызвана другая. Уровень вложенности увеличивается на единицу каждый раз, когда начинается выполнение вызванной функции и уменьшается на единицу, когда ее выполнение завершается. Вложенность определяемых пользователем функций не может превышать 32 уровней. Превышение максимального уровня вложенности приводит к ошибке выполнения для всей цепочки вызываемых функций.   
  
## <a name="metadata"></a>Метаданные  
 В следующем разделе приводятся системные представления каталога, возвращающие метаданные об определяемых пользователем функциях.  
  
 [sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md): отображает определение определяемых пользователем функций [!INCLUDE[tsql](../../includes/tsql-md.md)]. Пример:  
  
```  
SELECT definition, type   
FROM sys.sql_modules AS m  
JOIN sys.objects AS o   
    ON m.object_id = o.object_id   
    AND type = ('FN');  
GO  
  
```  
  
 [sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md): выводит сведения о параметрах, определенных в определяемых пользователем функциях.  
  
 [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md): отображает базовые объекты, на которые ссылается функция.  
  
## <a name="permissions"></a>Разрешения  
 Требуется разрешение CREATE FUNCTION на базу данных и разрешение ALTER на схему, в которой создается функция.  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Примеры: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] и [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-using-a-scalar-valued-user-defined-function-to-change-a-data-type"></a>A. Применение скалярной определяемой пользователем функции для изменения типа данных  
 Эта скалярная функция принимает тип данных **int** и возвращает тип данных **decimal(10,2)** .  
  
```  
CREATE FUNCTION dbo.ConvertInput (@MyValueIn int)  
RETURNS decimal(10,2)  
AS  
BEGIN  
    DECLARE @MyValueOut int;  
    SET @MyValueOut= CAST( @MyValueIn AS decimal(10,2));  
    RETURN(@MyValueOut);  
END;  
GO  
  
SELECT dbo.ConvertInput(15) AS 'ConvertedValue';  
```  
  
## <a name="see-also"></a>См. также:  
 [ALTER FUNCTION (SQL Server PDW)](https://msdn.microsoft.com/25ff3798-eb54-4516-9973-d8f707a13f6c)   
 [DROP FUNCTION (SQL Server PDW)](https://msdn.microsoft.com/1792a90d-0d06-4852-9dec-6de1b9cd229e)  
  
  



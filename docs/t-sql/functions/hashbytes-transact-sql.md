---
title: HASHBYTES (Transact-SQL) | Документы Майкрософт
ms.custom: ''
ms.date: 07/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- HASHBYTES_TSQL
- HASHBYTES
dev_langs:
- TSQL
helpviewer_keywords:
- hash input
- HASHBYTES
ms.assetid: 0ea6a4d1-313e-4f70-b939-dd2cd570f6d6
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e3eef3752a362dcc6709d7c7461cd35fdc5c1892
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68077782"
---
# <a name="hashbytes-transact-sql"></a>HASHBYTES (Transact-SQL)

[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Возвращает хэш MD2, MD4, MD5, SHA1 или SHA2 входного значения в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
HASHBYTES ( '<algorithm>', { @input | 'input' } )  
  
<algorithm>::= MD2 | MD4 | MD5 | SHA | SHA1 | SHA2_256 | SHA2_512   
```  
  
## <a name="arguments"></a>Аргументы  

`<algorithm>`  
Указывает используемый алгоритм хэширования. Этот аргумент обязателен и не имеет значения по умолчанию. Указание одинарных кавычек (") также обязательно. Начиная с версии [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] все алгоритмы, отличные от SHA2_256 и SHA2_512, являются нерекомендуемыми.  
  
`@input`  
Указывает переменную, содержащую хэшируемые данные. `@input` имеет тип **varchar**, **nvarchar** или **varbinary**.  
  
*input*  
Определяет выражение, анализирующее тип хэшируемых данных (символьная или двоичная строка).  
  
 Выходные данные соответствуют стандарту алгоритма: 128 бит (16 байт) для MD2, MD4 и MD5; 160 бит (20 байт) для SHA и SHA1; 256 бит (32 байта) для SHA2_256 и 512 бит (64 байта) для SHA2_512.  
  
**Применимо к**: с [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] до [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]
  
 Для [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] и более ранних версий допустимый размер входных значений ограничен 8000 байтами.  
  
## <a name="return-value"></a>Возвращаемое значение  
 **varbinary** (не более 8000 байт)  

## <a name="remarks"></a>Remarks  
Попробуйте использовать `CHECKSUM` или `BINARY_CHECKSUM` в качестве альтернативы для вычисления хэш-значения.

Алгоритмы MD2, MD4, MD5, SHA и SHA1 начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] отмечены как нерекомендуемые. Вместо этого используйте алгоритмы SHA2_256 или SHA2_512. Старые алгоритмы по-прежнему будут работать, но будут вызывать событие нерекомендуемого алгоритма.

## <a name="examples"></a>Примеры  
### <a name="return-the-hash-of-a-variable"></a>Возвращение хэша данных в переменной  
 В приведенном ниже примере возвращается хэш `SHA2_256` данных типа **nvarchar**, хранящихся в переменной `@HashThis`.  
  
```sql  
DECLARE @HashThis nvarchar(32);  
SET @HashThis = CONVERT(nvarchar(32),'dslfdkjLK85kldhnv$n000#knf');  
SELECT HASHBYTES('SHA2_256', @HashThis);  
```  
  
### <a name="return-the-hash-of-a-table-column"></a>Возвращает хэш данных в столбце таблицы  
 Следующий пример возвращает хэш SHA2_256 для значений в столбце `c1` таблицы `Test1`.  
  
```sql  
CREATE TABLE dbo.Test1 (c1 nvarchar(32));  
INSERT dbo.Test1 VALUES ('This is a test.');  
INSERT dbo.Test1 VALUES ('This is test 2.');  
SELECT HASHBYTES('SHA2_256', c1) FROM dbo.Test1;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------------------------------------  
0x741238C01D9DB821CF171BF61D72260B998F7C7881D90091099945E0B9E0C2E3 
0x91DDCC41B761ACA928C62F7B0DA61DC763255E8247E0BD8DCE6B22205197154D  
(2 row(s) affected)  
```  
  
## <a name="see-also"></a>См. также:  
[Выбор алгоритма шифрования](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)
[CHECKSUM_AGG (Transact-SQL)](../../t-sql/functions/checksum-agg-transact-sql.md)
[CHECKSUM (Transact-SQL)](../../t-sql/functions/checksum-transact-sql.md)
[BINARY_CHECKSUM (Transact-SQL)](../../t-sql/functions/binary-checksum-transact-sql.md)

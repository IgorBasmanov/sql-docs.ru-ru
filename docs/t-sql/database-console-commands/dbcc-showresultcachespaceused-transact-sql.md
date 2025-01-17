---
title: DBCC SHOWRESULTCACHESPACEUSED (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 07/03/2019
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 73f598cf-b02a-4dba-8d89-9fc0b55a12b8
author: XiaoyuMSFT
ms.author: xiaoyul
monikerRange: = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: ffd0ad4ddcdae91071811e57cdb8c5f6aaaea656
ms.sourcegitcommit: db9bed6214f9dca82dccb4ccd4a2417c62e4f1bd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476309"
---
# <a name="dbcc-showresultcachespaceused-transact-sql"></a>DBCC SHOWRESULTCACHESPACEUSED (Transact-SQL)

[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-xxx-md.md)]

Отображает сведения о месте в хранилище, используемом кэшем результирующих наборов базы данных службы "[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] Azure".
  
![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Синтаксис  
  
```sql
DBCC SHOWRESULTCACHESPACEUSED  
[;]  
```  
## <a name="remarks"></a>Remarks

Команда `DBCC SHOWRESULTCACHESPACEUSED` не требует указания никаких параметров и возвращает пространство, используемое базой данных, в которой она выполняется.

Максимальный размер кэша результирующих наборов составляет 1 ТБ на базу данных.  Служба "Хранилище данных SQL Azure" автоматически исключает записи в кэше результирующих наборов:

- каждые 48 часов, если результирующий набор не использовался;
- если кэш результирующих наборов достигает максимального размера.

Пользователи могут вручную очистить кэш результирующих наборов базы данных, отключив функции кэша или с помощью команды `DBCC DROPRESULTSETCACHE`.   Если приостановить базу данных, кэш результирующих наборов не очищается.  

## <a name="permissions"></a>Разрешения

Необходимо разрешение VIEW SERVER STATE.
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Столбец|Тип данных|Описание|  
|------------|---------------|-----------------|  
|reserved_space|BIGINT|Общий размер пространства, используемого для базы данных, в КБ. Это число будет меняться по мере увеличения кэшированного результирующего набора.|  
|data_space|BIGINT|Пространство, используемое для данных, в КБ.|  
|index_space|BIGINT|Пространство, используемое для индексов, в КБ.|  
|unused_space|BIGINT|Пространство, которое является частью зарезервированного пространства и не используется, в КБ.|  


## <a name="see-also"></a>См. также раздел

[Параметры ALTER DATABASE SET &#40;Transact-SQL&#41;](/sql/t-sql/statements/alter-database-transact-sql-set-options?view=azure-sqldw-latest)</br>
[ALTER DATABASE (Transact-SQL)](/sql/t-sql/statements/alter-database-transact-sql?view=azure-sqldw-latest)</br>
[SET RESULT SET CACHING &#40;Transact-SQL&#41;](/sql/t-sql/statements/set-result-set-caching-transact-sql)</br>
[DBCC DROPRESULTSETCACHE  &#40;Transact-SQL&#41;](/sql/t-sql/database-console-commands/dbcc-dropresultsetcache-transact-sql)
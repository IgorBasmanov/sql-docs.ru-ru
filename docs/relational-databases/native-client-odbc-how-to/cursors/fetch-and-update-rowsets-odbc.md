---
title: Выбор и обновление наборов строк (ODBC) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- rowsets [ODBC]
ms.assetid: cf0eb3b4-8b72-49fc-a845-95edc360cf93
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bb0055e8ed8c17679c824fdf6cbf96c1a42c7531
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67939588"
---
# <a name="fetch-and-update-rowsets-odbc"></a>Выбор и обновление наборов строк (ODBC)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

    
### <a name="to-fetch-and-update-rowsets"></a>Получение и обновление наборов строк  
  
1.  Можно также вызвать [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) с SQL_ROW_ARRAY_SIZE, чтобы изменить число строк (R) в наборе строк.  
  
2.  Вызовите [SQLFetch](https://go.microsoft.com/fwlink/?LinkId=58401) или [SQLFetchScroll](../../../relational-databases/native-client-odbc-api/sqlfetchscroll.md) , чтобы получить набор строк.  
  
3.  Если используются связанные столбцы, используйте для набора строк значения данных и длины данных, доступные теперь в буферах связанных столбцов.  
  
     Если используются несвязанные столбцы, вызовите для каждой строки [SQLSetPos](https://go.microsoft.com/fwlink/?LinkId=58407) с параметром SQL_POSITION, чтобы установить позицию курсора, а затем выполните следующие действия для каждого несвязанного столбца.  
  
    -   Вызовите [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) один или более раз, чтобы получить данные для несвязанных столбцов после последнего связанного столбца в наборе строк. Вызовы [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) должны происходить в порядке возрастания номеров столбцов.  
  
    -   Получение данных из столбца типа text или image производится многократным вызовом функции [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) .  
  
4.  Настройте текстовые столбцы или столбцы изображений, получающие данные во время выполнения.  
  
5.  Используйте вызовы [SQLSetPos](https://go.microsoft.com/fwlink/?LinkId=58407) или [SQLBulkOperations](https://go.microsoft.com/fwlink/?LinkId=58398) для установки положения курсора, обновления, удаления или добавления строк в наборе строк.  
  
     Если для операций обновления и удаления используются текстовые столбцы или столбцы изображений, получающие данные во время выполнения, обработайте их.  
  
6.  Выполните инструкцию позиционирования UPDATE или DELETE, задав имя курсора (его можно получить с помощью [SQLGetCursorName](../../../relational-databases/native-client-odbc-api/sqlgetcursorname.md)) и используя дескриптор другой инструкции в том же соединении (необязательно).  
  
## <a name="see-also"></a>См. также  
 [Использование инструкций по курсорам &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-how-to/cursors/using-cursors-how-to-topics-odbc.md)  
  
  

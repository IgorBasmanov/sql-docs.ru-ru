---
title: Транзакции в ODBC | Документация Майкрософт
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, transactions
- transactions [ODBC]
- ODBC, transactions
ms.assetid: c5a87fa5-827a-4e6f-a0d9-924bac881eb0
author: MightyPen
ms.author: genemi
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f076622b30d1f1102bac8ff6c89ecf2e36a96860
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67913167"
---
# <a name="performing-transactions-in-odbc"></a>Выполнение транзакций в ODBC
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
[!INCLUDE[SNAC_Deprecated](../../../includes/snac-deprecated.md)]

  Управление транзакциями в ODBC выполняется на уровне соединения. Когда приложение завершает транзакцию, оно фиксирует или откатывает назад все операции со всеми инструкциями, хранящими дескриптор данного соединения. Для фиксации или отката транзакции приложения должны вызывать метод [SQLEndTran](../../../relational-databases/native-client-odbc-api/sqlendtran.md) вместо непосредственного выполнения инструкции COMMIT или ROLLBACK.  
  
 Для переключения между двумя режимами управления транзакциями, которые применяются в ODBC, используется метод [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) .  
  
-   Режим автоматической фиксации  
  
     Каждая отдельная инструкция языка автоматически фиксируется после успешного завершения. При работе в режиме автоматической фиксации других средств управления транзакциями не требуется.  
  
-   Режим ручной фиксации  
  
     Все выполненные инструкции будут включены в одну и ту же транзакцию, если ее не прекратить явным образом, вызвав **SQLEndTran**.  
  
 Режим автоматической фиксации применяется в ODBC по умолчанию. Любое новое соединение находится в режиме автоматической фиксации, пока не будет вызван метод **SQLSetConnectAttr** для перехода в режим ручной фиксации. Когда приложение отключает режим автоматической фиксации, следующая инструкция, введенная в базу данных, начнет транзакцию. Эта транзакция остается в силе, пока приложение не вызовет функцию **SQLEndTran** с параметром SQL_COMMIT либо SQL_ROLLBACK. Команда, посланная в базу данных после вызова функции **SQLEndTran** , начинает новую транзакцию.  
  
 Если приложение переключается с режима ручной фиксации на автоматическую, драйвер фиксирует все транзакции, открытые в этот момент для данного соединения.  
  
 Приложения ODBC не должны использовать инструкции языка Transact-SQL, такие как BEGIN TRANSACTION, COMMIT TRANSACTION и ROLLBACK TRANSACTION, потому что это может привести к ситуации, когда поведение драйвера не определено. Приложение ODBC должно выполняться в режиме автоматической фиксации и не использовать никаких функций или инструкций для управления транзакциями или работать в режиме ручной фиксации и использовать функцию ODBC **SQLEndTran** для фиксации и отката транзакций.  
  
## <a name="see-also"></a>См. также  
 [Выполнение транзакций &#40;ODBC&#41;](https://msdn.microsoft.com/library/f431191a-5762-4f0b-85bb-ac99aff29724)  
  
  

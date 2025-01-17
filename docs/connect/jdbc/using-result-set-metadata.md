---
title: Использование метаданных результирующего набора | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 5e37529a-30db-48c8-b90a-ae9657d0f6b0
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 86e41f52ed8296c46cfd7b167407b10fc9f0b285
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68005941"
---
# <a name="using-result-set-metadata"></a>Использование метаданных результирующего набора

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

В драйвере [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] реализован класс [SQLServerResultSetMetaData](../../connect/jdbc/reference/sqlserverresultsetmetadata-class.md), позволяющий запрашивать в результирующем наборе сведения о содержащихся в нем столбцах. Этот класс содержит многочисленные методы, которые возвращают сведения в виде одного значения.

Чтобы создать объект SQLServerResultSetMetaData, можно [использовать метод GetObject](../../connect/jdbc/reference/getmetadata-method-sqlserverresultset.md) класса [SQLServerResultSet](../../connect/jdbc/reference/sqlserverresultset-class.md) .

В следующем примере открытое соединение с [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] образцом базы данных передается в функцию, метод GetObject класса SQLServerResultSet используется для возврата объекта SQLServerResultSetMetaData, а затем различные методы класса Объект SQLServerResultSetMetaData используется для вывода сведений об имени и типе данных столбцов, содержащихся в результирующем наборе.

[!code[JDBC#UsingResultSetMetaData1](../../connect/jdbc/codesnippet/Java/using-result-set-metadata_1.java)]

## <a name="see-also"></a>См. также:

[Обработка метаданных с использованием драйвера JDBC](../../connect/jdbc/handling-metadata-with-the-jdbc-driver.md)

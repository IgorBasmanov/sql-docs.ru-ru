---
title: Работа с инструкциями и результирующими наборами | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: cc917534-f5f8-4844-87c8-597c48b4e06d
author: MightyPen
ms.author: genemi
ms.openlocfilehash: fb6d545a3a7f8c3b29e5bc372aa4fdadf95edd52
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68003786"
---
# <a name="working-with-statements-and-result-sets"></a>Работа с инструкциями и результирующими наборами

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

При работе с драйвером [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] и его объектами Statement и ResultSet существует ряд способов повысить производительность и надежность ваших приложений.

## <a name="use-the-appropriate-statement-object"></a>Использование подходящего объекта инструкции

При использовании объектов Statement драйвера JDBC, например [SQLServerStatement](../../connect/jdbc/reference/sqlserverstatement-class.md), [SQLServerPreparedStatement](../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) или [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md), обязательно используйте объект, подходящий для вашей задачи.

- Если у вас нет параметров OUT, не нужно использовать объект SQLServerCallableStatement. Вместо этого используйте объект SQLServerStatement или SQLServerPreparedStatement.

- Если вы не планируете выполнять инструкцию более одного раза или не используете параметры IN или OUT, не нужно использовать объект SQLServerCallableStatement или SQLServerPreparedStatement. Вместо этого используйте объект SQLServerStatement.

## <a name="use-the-appropriate-concurrency-for-resultset-objects"></a>Используйте подходящий параллелизм для объектов ResultSet

Обновляемый параллелизм при создании инструкций, выдающих результирующие наборы, требуется, только если планируется непременно обновлять результаты. Маленькие результирующие наборы быстрее всего считывает стандартная однопроходная неизменяемая модель курсора.

## <a name="limit-the-size-of-your-result-sets"></a>Ограничение размера результирующих наборов

Попробуйте использовать метод [setMaxRows](../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md) (либо SQL-синтаксис SET ROWCOUNT или SELECT TOP N), чтобы ограничить число строк, возвращаемых из потенциально больших результирующих наборов. Если приходится работать с большими результирующими наборами, то, возможно, следует использовать адаптивную буферизацию ответов. Для этого нужно установить свойству строки соединения responseBuffering значение «adaptive». Это значение используется по умолчанию. Такая методика позволяет приложению обрабатывать большие результирующие наборы, не прибегая к курсорам на стороне сервера, и свести к минимуму занимаемую приложением память. Дополнительные сведения см. [в разделе Использование адаптивной буферизации](../../connect/jdbc/using-adaptive-buffering.md).

## <a name="use-the-appropriate-fetch-size"></a>Использование выборки подходящего размера

Для серверных курсоров только для чтения целесообразность определяется сравнением затрат на обращение к серверу и затрат памяти драйвером. Для обновляемых серверных курсоров размер выборки также влияет на чувствительность результирующего набора к изменениям и параллелизму на сервере. Вы не увидите изменений в строках в текущем буфере выборки, пока не воспользуетесь в явном виде методом [refreshRow](../../connect/jdbc/reference/refreshrow-method-sqlserverresultset.md) или пока из буфера не выйдет курсор. У больших буферов выборки будет лучшая производительность (меньше обращений к серверу), но у них меньше чувствительность к изменениям и уменьшению параллелизма на сервере при использовании CONCUR_SS_SCROLL_LOCKS (1009). Для максимальной чувствительности к изменениям размер выборки должен равняться 1. Однако при этом будет совершаться одно обращение к серверу на каждую выбранную строку.

## <a name="use-streams-for-large-in-parameters"></a>Использование потоков для больших параметров IN

Используйте потоки объектов BLOB и CLOB, постепенно материализуемые для управления обновляющимися большими значениями столбцов или отправления больших параметров IN. Драйвер JDBC дробит их и отправляет на сервер за несколько обращений, тем самым позволяя задавать и обновлять значения большие, чем помещаются в памяти.

## <a name="see-also"></a>См. также:

[Повышение производительности и надежности с помощью драйвера JDBC](../../connect/jdbc/improving-performance-and-reliability-with-the-jdbc-driver.md)

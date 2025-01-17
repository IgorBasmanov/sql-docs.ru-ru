---
title: Принудительная отработка отказа SQL Server для групп доступности
description: Принудительная отработка отказа для групп доступности с типом кластера NONE
services: ''
author: MikeRayMSFT
ms.topic: include
ms.date: 02/05/2018
ms.author: mikeray
ms.custom: include file
ms.openlocfilehash: 87fce17db46dc590fbffe0bae0b27c17bd54320e
ms.sourcegitcommit: db9bed6214f9dca82dccb4ccd4a2417c62e4f1bd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68213087"
---
Каждая группа доступности имеет только одну первичную реплику. Первичная реплика позволяет выполнять операции чтения и записи. Чтобы изменить первичную реплику, можно выполнить переход на другой ресурс. В группе доступности для обеспечения высокой доступности диспетчер кластеров автоматизирует процесс перехода на другой ресурс. В группе доступности с типом кластера NONE принудительная отработка отказа выполняется вручную. 

Существует два способа отработки отказа первичной реплики в группе доступности с типом кластера NONE.

- Принудительный переход на другой ресурс вручную с потерей данных
- Переход на другой ресурс вручную без потери данных

### <a name="forced-manual-failover-with-data-loss"></a>Принудительный переход на другой ресурс вручную с потерей данных

Это метод можно использовать, если первичная реплика недоступна и не может быть восстановлена. 

Для принудительной отработки отказа с потерей данных подключитесь к экземпляру SQL Server, в котором размещена целевая вторичная реплика, и выполните следующие команды:

```SQL
ALTER AVAILABILITY GROUP [ag1] FORCE_FAILOVER_ALLOW_DATA_LOSS;
```

При восстановлении предыдущей первичной реплики требуется первичная роль. Чтобы убедиться, что предыдущая первичная реплика переходит на вторичную роль, выполните на ней приведенную ниже команду.

```SQL
ALTER AVAILABILITY GROUP [ag1]  SET (ROLE = SECONDARY);
```

### <a name="manual-failover-without-data-loss"></a>Переход на другой ресурс вручную без потери данных

Это метод можно использовать, если первичная реплика доступна, но необходимо временно или навсегда изменить конфигурацию и изменить экземпляр SQL Server, на котором размещена первичная реплика. Чтобы избежать возможной потери данных, перед началом ручной отработки отказа убедитесь, что целевая вторичная реплика находится в актуальном состоянии. 

Чтобы перейти на другой ресурс вручную без потери данных, выполните следующие действия.

1. Выполните для целевой вторичной реплики `SYNCHRONOUS_COMMIT`.

   ```SQL
   ALTER AVAILABILITY GROUP [ag1] 
        MODIFY REPLICA ON N'<node2>' 
        WITH (AVAILABILITY_MODE = SYNCHRONOUS_COMMIT);
   ```

2. Чтобы определить, что активные транзакции фиксируются в первичной реплике и по меньшей мере в одной синхронной вторичной реплике, выполните следующий запрос: 

   ```SQL
   SELECT ag.name, 
      drs.database_id, 
      drs.group_id, 
      drs.replica_id, 
      drs.synchronization_state_desc, 
      ag.sequence_number
   FROM sys.dm_hadr_database_replica_states drs, sys.availability_groups ag
   WHERE drs.group_id = ag.group_id; 
   ```

   Вторичная реплика синхронизируется, если `synchronization_state_desc` имеет значение `SYNCHRONIZED`.

3. Обновите `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` до 1.

   Следующий скрипт задает для `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` значение 1 в группе доступности `ag1`. Перед запуском скрипта замените `ag1` именем группы доступности.

   ```SQL
   ALTER AVAILABILITY GROUP [ag1] 
        SET REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = 1;
   ```

   Этот параметр означает, что все активные транзакции фиксируются на первичной реплике и по меньшей мере на одной синхронной вторичной реплике. 

4. Понизьте роль первичной реплики до вторичной. После понижения роли первичная реплика становится доступной только для чтения. Чтобы изменить ее роль на `SECONDARY`, выполните на экземпляре SQL Server, где размещена первичная реплика, следующую команду:

   ```SQL
   ALTER AVAILABILITY GROUP [ag1] 
        SET (ROLE = SECONDARY); 
   ```

5. Повысьте уровень целевой вторичной реплики до первичной. 

   ```SQL
   ALTER AVAILABILITY GROUP ag1 FORCE_FAILOVER_ALLOW_DATA_LOSS; 
   ```  

   > [!NOTE] 
   > Для удаления группы доступности используйте [DROP AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql). Для группы доступности, созданной с типом кластера NONE или EXTERNAL, выполните команду на всех репликах, входящих в группу доступности.

---
title: Инициализация подписки на публикацию транзакций из резервной копии | Документация Майкрософт
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- manual subscription initialization [SQL Server replication]
- subscriptions [SQL Server replication], initializing
- initializing subscriptions [SQL Server replication], without snapshots
- transactional replication, backup and restore
- backups [SQL Server replication], transactional replication
ms.assetid: d0637fc4-27cc-4046-98ea-dc86b7a3bd75
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 3e0848a5452ad7cdb189b00c5ed90a6ad1cb8a1a
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68127866"
---
# <a name="initialize-a-transactional-subscription-from-a-backup"></a>Инициализация подписки на публикацию транзакций из резервной копии
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Хотя инициализация подписки на публикацию транзакций осуществляется через моментальный снимок, она также может быть выполнена с помощью хранимых процедур репликации. Дополнительные сведения см. в статье [Initialize a Transactional Subscription Without a Snapshot](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
### <a name="to-initialize-a-transactional-subscriber-from-a-backup"></a>Инициализация подписчика на публикацию транзакций из резервной копии  
  
1.  Убедитесь, что существующая публикация поддерживает возможность инициализации из резервной копии. Для этого в базе данных публикации на издателе выполните хранимую процедуру [sp_helppublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md). В результирующем наборе проверьте значение параметра **allow_initialize_from_backup** .  
  
    -   Если оно равно **1**, то публикация поддерживает данную функцию.  
  
    -   Если значение равно **0**, выполните хранимую процедуру [sp_changepublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md) на издателе в базе данных публикации. В параметре **allow_initialize_from_backup** укажите значение **@property** , а в параметре **@value** укажите значение **@value** .  
  
2.  В новой публикации выполните хранимую процедуру [sp_addpublication (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md) на издателе в базе данных публикации. Задайте значение **true** для параметра **allow_initialize_from_backup**. Дополнительные сведения см. в разделе [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md).  
  
    > [!WARNING]  
    >  Чтобы предотвратить отсутствие данных подписчика, при использовании процедуры **sp_addpublication** с `@allow_initialize_from_backup = N'true'`всегда используйте `@immediate_sync = N'true'`.  
  
3.  Создайте резервную копию базы данных публикации с использованием инструкции [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md).  
  
4.  Восстановите резервную копию на подписчике, выполнив инструкцию [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md).  
  
5.  На издателе в базе данных издателя выполните хранимую процедуру [sp_addsubscription (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md). Укажите значения следующих параметров.  
  
    -   **@sync_type** — значение параметра **initialize with backup**.  
  
    -   **@backupdevicetype** — тип устройства резервного копирования: **logical** (по умолчанию), **disk**или **tape**.  
  
    -   **@backupdevicename** — логическое или физическое устройство резервного копирования, с которого будет производиться восстановление.  
  
         Для логического устройства задайте имя устройства резервного копирования, указанное при создании устройства при помощи хранимой процедуры **sp_addumpdevice** .  
  
         Для физического устройства укажите полный путь и имя файла, например: `DISK = 'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\BACKUP\Mybackup.dat'` или `TAPE = '\\.\TAPE0'`.  
  
    -   (Необязательно) **@password** — пароль, заданный во время сеанса создания резервного набора данных.  
  
    -   (Необязательно) **@mediapassword** — пароль, заданный во время сеанса форматирования набора носителей.  
  
    -   (Необязательно) **@fileidhint** — идентификатор восстанавливаемой резервной копии. Например, значение **1** указывает первый резервный набор данных на носителе данных резервных копий, а значение **2** — второй резервный набор данных.  
  
    -   (Для ленточных устройств необязательно.) **@unload** . Если после завершения сеанса восстановления необходимо извлечь ленту из устройства считывания, укажите значение **1** (по умолчанию), в противном случае — значение **0**.  
  
6.  (Необязательно.) Для подписки по запросу выполните процедуру [sp_addpullsubscription (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md) и [sp_addpullsubscription_agent (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md) на подписчике в базе данных подписки. Дополнительные сведения см. в статье [Создание подписки по запросу](../../relational-databases/replication/create-a-pull-subscription.md).  
  
7.  (Необязательно) Запустите агент распространителя. Дополнительные сведения см. в разделе [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md) или [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
## <a name="see-also"></a>См. также:  
 [Copy Databases with Backup and Restore](../../relational-databases/databases/copy-databases-with-backup-and-restore.md)  (Копирование баз данных путем создания и восстановления резервных копий)  
 [Резервное копирование и восстановление баз данных SQL Server](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)  
  
  

---
title: Резервное копирование и восстановление баз данных SQL Server | Документация Майкрософт
ms.custom: ''
ms.date: 03/30/2018
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- disaster recovery [SQL Server], see restoring [SQL Server]
- backups [SQL Server]
- restoring databases [SQL Server]
- backup [SQL Server], see backing up [SQL Server]
- databases [SQL Server], restoring
- backing up databases [SQL Server]
- restore [SQL Server], see restoring [SQL Server]
- disaster recovery [SQL Server], see backing up [SQL Server]
- backing up [SQL Server]
- Database Engine [SQL Server], backups
- databases [SQL Server], backups
ms.assetid: 570a21b3-ad29-44a9-aa70-deb2fbd34f27
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1b37e555c4118ca3199e132d20a6689c80b40bab
ms.sourcegitcommit: a1adc6906ccc0a57d187e1ce35ab7a7a951ebff8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68893472"
---
# <a name="back-up-and-restore-of-sql-server-databases"></a>Резервное копирование и восстановление баз данных SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  В этой статье описаны преимущества резервного копирования баз данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], основные условия резервного копирования и восстановления, а также приведены стратегии резервного копирования и восстановления для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и рассмотрены вопросы безопасности, связанные с резервным копированием и восстановлением в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

> **Нужны пошаговые инструкции?** Этот раздел не **содержит конкретные шаги по выполнению резервного копирования.** Если вы хотите перейти непосредственно к сведениям о создании резервных копий, прокрутите эту страницу вниз до раздела ссылок, упорядоченного по задачам резервного копирования и по потребности в SSMS или T-SQL.  
  
 Компонент резервного копирования и восстановления SQL Server обеспечивает необходимую защиту важных данных, которые хранятся в базах данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Чтобы минимизировать риск необратимой потери данных, необходимо регулярно создавать резервные копии баз данных, в которых будут сохраняться производимые изменения данных. Хорошо продуманная стратегия резервного копирования и восстановления защищает базы от потери данных при повреждениях, происходящих из-за различных сбоев. Проверьте выбранную стратегию, выполнив восстановление баз данных из набора резервных копий; это поможет эффективно среагировать на реальные проблемы.
  
 В дополнение к локальному хранилищу для хранения резервных копий SQL Server поддерживает резервное копирование и восстановление из службы хранилища больших двоичных объектов Windows Azure. Дополнительные сведения см. в разделе [Резервное копирование и восстановление SQL Server с помощью службы хранилища BLOB-объектов Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md). Для файлов базы данных, сохраненных в службе хранилища больших двоичных объектов Microsoft Azure, [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] предоставляет возможность использовать моментальные снимки Azure для практически мгновенного создания резервных копий и ускорения восстановления. Дополнительные сведения см. в разделе [Резервные копии моментальных снимков файлов для файлов базы данных в Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md).  
  
##  <a name="why-back-up"></a>Зачем выполнять резервное копирование  
-   Создание резервных копий баз данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , выполнение проверочных процедур восстановления резервных копий и хранение резервных копий в безопасном месте вне рабочей площадки помогают предотвратить возможную необратимую потерю данных. **Резервное копирование — единственный способ защитить данные.**

     При правильном создании резервных копий баз данных можно будет восстановить данные после многих видов сбоев, включая следующие:  
  
    -   сбой носителя;    
    -   ошибки пользователей (например, удаление таблицы по ошибке);    
    -   сбои оборудования (например, поврежденный дисковый накопитель или безвозвратная потеря данных на сервере);    
    -   стихийные бедствия. При выполнении резервного копирования SQL Server в службу хранилища больших двоичных объектов Windows Azure можно создать резервную копию в регионе, отличном от региона своего фактического локального расположения, на случай возникновения природных катастроф в своем регионе.  
  
-   Кроме того, резервные копии баз данных полезны и при выполнении повседневных административных задач, например для копирования баз данных с одного сервера на другой, настройки [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] или зеркального отображения баз данных и архивирования.  
  
##  <a name="glossary-of-backup-terms"></a>Глоссарий терминов, связанных с резервным копированием
 **создание резервных копий**  
 Процесс создания **резервной копии** путем копирования записей данных из базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или записей журнала из ее журнала транзакций.  
  
 **резервная копия**  
 Копия данных, которая может использоваться для восстановления данных в случае возникновения ошибки. Резервные копии баз данных также могут использоваться для восстановления копии базы данных в новом расположении.  
  
устройство**резервного копирования**  
 Диск или ленточное устройство, на которые записываются резервные копии SQL Server для последующего восстановления. Резервные копии SQL Server можно также записать в службу хранилища больших двоичных объектов Windows Azure, а формат **URL** используется, чтобы указать назначение и имя файла резервной копии. Дополнительные сведения см. в разделе [Резервное копирование и восстановление SQL Server с помощью службы хранилища BLOB-объектов Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).  
  
**носитель данных резервной копии**  
 Одна магнитная лента или несколько или один файл на диске или несколько, в которые были записаны одна резервная копия или несколько.  
  
**резервное копирование данных**  
 Резервная копия данных всей базы данных (резервная копия базы данных), части базы данных (частичная резервная копия) или набора файлов данных или файловых групп (резервная копия файлов).  
  
**резервное копирование базы данных**  
 Резервная копия базы данных. Полные резервные копии базы данных отображают состояние всей базы данных на момент завершения резервного копирования. Разностные резервные копии базы данных содержат только изменения базы данных с момента последнего полного резервного копирования.  
  
**разностная резервная копия**  
 Резервная копия данных, основанная на последней полной или частичной резервной копии базы данных или набора файлов данных или файловых групп (базовой копии для разностного копирования), которая содержит только данные, измененные по сравнению с базовой копией для разностного копирования.  
  
**полная резервная копия**  
 Резервная копия, которая содержит все данные заданной базы данных или наборов файлов или файловых групп, а также журналов для обеспечения возможности последующего восстановления этих данных.  
  
**резервная копия журналов**  
 Резервная копия журналов транзакций, включающая все записи журнала, не входившие в предыдущую резервную копию журналов. (модель полного восстановления)  
  
**восстановить**  
 Для возврата базы данных в стабильное и согласованное состояние.  
  
**recovery**  
 Фаза запуска или восстановления базы данных, которая приводит базу данных в состояние согласованности транзакций.  
  
**модель восстановления**  
 Свойство базы данных, с помощью которого выполняется управление обслуживанием журналов транзакций в базе данных. Существует три модели восстановления: простая модель восстановления, модель полного восстановления и модель восстановления с неполным протоколированием. Модель восстановления базы данных определяет требования к резервному копированию и восстановлению.  
  
**восстановление**  
 Многоэтапный процесс, в ходе которого все данные и страницы журнала копируются из указанной резервной копии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в определенную базу данных, а затем выполняется накат всех фиксированных транзакций, записанных в резервной копии журнала, путем внесения новых данных на основе зарегистрированных изменений.  
  
 ##  <a name="backup-and-restore-strategies"></a>Стратегии резервного копирования и восстановления  
 Операции резервирования и восстановления данных следует адаптировать под конкретную среду с учетом доступных ресурсов. Таким образом, для надежной работы операций резервирования и восстановления необходима стратегия резервирования и восстановления. Правильно созданная стратегия резервирования и восстановления увеличивает доступность данных и уменьшает их потери, учитывая требования пользователей.  
  
  > [!IMPORTANT] 
  > Базу данных и резервные копии следует размещать на отдельных устройствах резервного копирования. В противном случае при сбое устройства, содержащего базу данных, резервные копии окажутся недоступными. Кроме того, размещение данных и их резервных копий на различных устройствах повышает производительность ввода-вывода как при записи резервных копий, так и в процессе эксплуатации базы данных.**  
  
 Стратегия резервирования и восстановления состоит из части, относящейся к резервированию, и части, относящейся к восстановлению. Часть, относящаяся к резервированию, определяет тип и частоту создания резервных копий, тип и скоростные характеристики оборудования, необходимого для их создания, способ проверки резервных копий, а также местонахождение и тип носителя резервных копий (включая и вопросы безопасности). Часть, относящаяся к восстановлению, определяет ответственного за проведение операций восстановления, а также методы их проведения, позволяющие удовлетворить требования пользователей по доступности данных и минимизации их потерь. Рекомендуется документировать процедуры резервирования и восстановления и хранить копию этой документации в документации по задаче.  
  
 Разработка эффективной стратегии резервирования и восстановления требует тщательного планирования, реализации и тестирования. Необходимо тестирование. До тех пор пока не были успешно восстановлены все резервные копии во всех сочетаниях, вовлеченных в стратегию восстановления, нет и стратегии резервного копирования. Необходимо оценить ряд факторов. следующие основные параметры.  
  
-   Производственные задачи организации, относящиеся к базе данных, особенно требования к доступности данных и их защите от потери.  
  
-   Свойства каждой базы данных: размер, типичное использование, характер содержимого, требования к данным и т. д.  
  
-   Ограничения на ресурсы, например: оборудование, персонал, пространство для хранения носителей резервных копий, физическая безопасность этих носителей и так далее.  

### <a name="impact-of-the-recovery-model-on-backup-and-restore"></a>Влияние модели восстановления на резервное копирование и восстановление  
 Операции резервного копирования и восстановления выполняются в контексте моделей восстановления. Модель восстановления является свойством базы данных, которое задает, как выполняется управление журналом транзакций. Также модель восстановления базы данных определяет, какие типы резервных копий и сценарии восстановления поддерживаются для базы данных. Обычно база данных использует простую модель восстановления или модель полного восстановления. Модель полного восстановления может быть дополнена путем переключения на модель с неполным протоколированием перед выполнением массовых операций. Основные сведения о моделях восстановления и их влиянии на управление журналом транзакций см. в разделе [Журнал транзакций (SQL Server)](../logs/the-transaction-log-sql-server.md).  
  
 Лучший выбор модели восстановления базы данных зависит от бизнес-требований. Чтобы избежать управления журналом транзакций и упростить резервное копирование и восстановление, используйте простую модель восстановления. Чтобы снизить вероятность потери результатов работы ценой увеличения административных издержек, используйте модель полного восстановления. Дополнительные сведения о влиянии моделей восстановления на создание резервных копий и восстановление см. в разделе [Общие сведения о резервном копировании (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
### <a name="design-your-backup-strategy"></a>Создание стратегии резервного копирования  
 После того как выбрана модель восстановления, соответствующая бизнес-требованиям определенной базы данных, необходимо спланировать и выполнить соответствующую стратегию резервного копирования. Оптимальная стратегия зависит от многих факторов, среди которых наиболее важны следующие.  
  
-   Сколько часов в день приложения имеют доступ к базе данных?  
  
     Если существует прогнозируемый внепиковый период, рекомендуется запланировать полное резервное копирование базы данных именно на этот период.  
  
-   Насколько часты и вероятны изменения и обновления?  
  
     Если изменения часты, учтите следующее.  
  
    -   В рамках простой модели восстановления рассмотрите возможность запланировать разностное резервное копирование между полными резервными копированиями базы данных. Разностная резервная копия сохраняет только те изменения, которые были внесены с момента последнего полного резервного копирования.  
  
    -   В рамках модели полного восстановления следует запланировать частное резервное копирование журналов. Планирование разностного резервного копирования между полными резервными копиями сокращает время восстановления путем сокращения количества резервных копий журналов, которые необходимо восстанавливать после восстановления данных.  
  
-   Касаются ли обычно изменения небольшой или же значительной части базы данных?  
  
     В большой базе данных, в которой изменения концентрируются в части файлов или файловых групп, полезно частичное резервное копирование или резервное копирование файлов. Дополнительные сведения см. в разделах [Частичные резервные копии (SQL Server)](../../relational-databases/backup-restore/partial-backups-sql-server.md) и [Полные резервные копии файлов (SQL Server)](../../relational-databases/backup-restore/full-file-backups-sql-server.md).  
  
-   Сколько места на диске требуется для полного резервного копирования базы данных?  
  
 ### <a name="estimate-the-size-of-a-full-database-backup"></a>Оценка размера полной резервной копии базы данных  
 Перед тем как выбрать стратегию резервного копирования и восстановления, необходимо рассчитать, какой объем места на диске необходим для полной резервной копии базы данных. При выполнении операции резервного копирования данные, содержащиеся в базе данных, копируются в файл резервной копии. Резервная копия содержит только фактические данные в базе данных, а не любое неиспользованное пространство. Поэтому резервная копия обычно меньше, чем база данных. Размер полной резервной копии базы данных вы можете вычислить с помощью системной хранимой процедуры **sp_spaceused**. Дополнительные сведения см. в разделе [sp_spaceused (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-spaceused-transact-sql.md).  
  
### <a name="schedule-backups"></a>Создание расписания резервного копирования  
 Влияние, оказываемое осуществлением резервного копирования на выполняемые транзакции, минимально, поэтому операции резервного копирования могут выполняться одновременно с выполнением обычных операций. Резервное копирование в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно выполнять с минимальным влиянием на рабочие нагрузки.  
   
>  Сведения об ограничениях параллелизма во время резервного копирования см. в разделе [Общие сведения о резервном копировании (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md).  
  
 После принятия решения о том, какой тип резервного копирования необходим и как часто его выполнять, рекомендуется запланировать регулярное резервное копирование как часть плана обслуживания базы данных. Дополнительные сведения о планах обслуживания и об их создании для резервных копий баз данных и журналов см. в разделе [Use the Maintenance Plan Wizard](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md).
  
### <a name="test-your-backups"></a>Тестирование резервных копий  
 Можно сказать, что стратегия восстановления отсутствует, пока резервные копии не протестированы. Очень важно полностью протестировать стратегию резервного копирования для каждой базы данных, восстанавливая копию базы данных в систему тестирования. Необходимо протестировать восстановление каждого типа резервной копии, которую планируется использовать.
  
 Рекомендуется вести руководство по эксплуатации для каждой базы данных. Такое руководство по эксплуатации должно указывать расположение резервных копий, имена устройств резервного копирования (если есть), время, требуемое для восстановления тестовой резервной копии.

## <a name="monitor-progress-with-xevent"></a>Отслеживание хода выполнения с помощью xEvent
Операции резервного копирования и восстановления могут выполняться в течение длительного времени из-за размера базы данных и сложности выполняемых операций. При возникновении проблемы с любой из операций можно использовать расширенное событие **backup_restore_progress_trace** для отслеживания хода выполнения в реальном времени. Дополнительные сведения о расширенных событиях см. в разделе [Расширенные события](../extended-events/extended-events.md).

  >[!WARNING]
  > С помощью расширенных событий backup_restore_progress_trace можно повысить производительность и существенно сэкономить дисковое пространство. Используйте эти события в течение короткого периода времени, проявляйте внимание и тщательно проверяйте свои изменения перед их реализацией в рабочей среде.


```sql
-- Create the backup_restore_progress_trace extended event esssion
CREATE EVENT SESSION [BackupRestoreTrace] ON SERVER 
ADD EVENT sqlserver.backup_restore_progress_trace
ADD TARGET package0.event_file(SET filename=N'BackupRestoreTrace')
WITH (MAX_MEMORY=4096 KB,EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,MAX_DISPATCH_LATENCY=5 SECONDS,MAX_EVENT_SIZE=0 KB,MEMORY_PARTITION_MODE=NONE,TRACK_CAUSALITY=OFF,STARTUP_STATE=OFF)
GO

-- Start the event session  
ALTER EVENT SESSION [BackupRestoreTrace]  
ON SERVER  
STATE = start;  
GO  

-- Stop the event session  
ALTER EVENT SESSION [BackupRestoreTrace]  
ON SERVER  
STATE = stop;  
GO  
```

### <a name="sample-output-from-extended-event"></a>Пример выходных данных расширенного события 

![Пример выходных данных события резервного копирования xevent](media/back-up-and-restore-of-sql-server-databases/backup-xevent-example.png)
![Пример выходных данных события восстановления xevent](media/back-up-and-restore-of-sql-server-databases/restore-xevent-example.png)
 
  
## <a name="more-about-backup-tasks"></a>Дополнительные сведения о задачах резервного копирования  
-   [Создание плана обслуживания](../../relational-databases/maintenance-plans/create-a-maintenance-plan.md)  
  
-   [Создание задания](../../ssms/agent/create-a-job.md)  
  
-   [Планирование задания](../../ssms/agent/schedule-a-job.md)  
  
## <a name="working-with-backup-devices-and-backup-media"></a>Работа с устройствами резервного копирования и носителями резервных копий  
-   [Определение логического устройства резервного копирования для дискового файла (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-disk-file-sql-server.md)  
  
-   [Определение логического устройства резервного копирования для ленточного накопителя (SQL Server)](../../relational-databases/backup-restore/define-a-logical-backup-device-for-a-tape-drive-sql-server.md)  
  
-   [Указание в качестве назначения резервного копирования диска или ленты (SQL Server)](../../relational-databases/backup-restore/specify-a-disk-or-tape-as-a-backup-destination-sql-server.md)  
  
-   [Удаление устройства резервного копирования (SQL Server)](../../relational-databases/backup-restore/delete-a-backup-device-sql-server.md)  
  
-   [Назначение срока хранения резервной копии (SQL Server)](../../relational-databases/backup-restore/set-the-expiration-date-on-a-backup-sql-server.md)  
  
-   [Просмотр содержимого ленты или файла резервной копии (SQL Server)](../../relational-databases/backup-restore/view-the-contents-of-a-backup-tape-or-file-sql-server.md)  
  
-   [Просмотр файлов данных и журналов в резервном наборе данных (SQL Server)](../../relational-databases/backup-restore/view-the-data-and-log-files-in-a-backup-set-sql-server.md)  
  
-   [Просмотр свойств и содержимого логического устройства резервного копирования (SQL Server)](../../relational-databases/backup-restore/view-the-properties-and-contents-of-a-logical-backup-device-sql-server.md)  
  
-   [Восстановление резервной копии с устройства (SQL Server)](../../relational-databases/backup-restore/restore-a-backup-from-a-device-sql-server.md)  
  
## <a name="creating-backups"></a>Создание резервных копий  
**Примечание.** Для создания частичной резервной копии или резервной копии только для копирования используется инструкция [!INCLUDE[tsql](../../includes/tsql-md.md)][BACKUP](../../t-sql/statements/backup-transact-sql.md) с параметром PARTIAL или COPY_ONLY.  
  
 ### <a name="using-ssms"></a>Использование среды SSMS   
-   [Создание полной резервной копии базы данных (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
-   [Создание резервной копии журнала транзакций (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
-   [Резервное копирование файлов и файловых групп (SQL Server)](../../relational-databases/backup-restore/back-up-files-and-filegroups-sql-server.md)  
  
-   [Создание разностной резервной копии базы данных (SQL Server)](../../relational-databases/backup-restore/create-a-differential-database-backup-sql-server.md)  
  
 ### <a name="using-t-sql"></a>Использование T-SQL  
-   [Использование регулятора ресурсов для ограничения загрузки ЦП при сжатии резервной копии (Transact-SQL)](../../relational-databases/backup-restore/use-resource-governor-to-limit-cpu-usage-by-backup-compression-transact-sql.md)  
  
-   [Создание резервной копии журнала транзакций при повреждении базы данных (SQL Server)](../../relational-databases/backup-restore/back-up-the-transaction-log-when-the-database-is-damaged-sql-server.md)  
  
-   [Включение или отключение вычисления контрольных сумм резервных копий во время резервного копирования или восстановления (SQL Server)](../../relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server.md)  
  
-   [Определение, продолжает ли операция резервного копирования или восстановления работу после возникновения ошибки (SQL Server)](../../relational-databases/backup-restore/specify-if-backup-or-restore-continues-or-stops-after-error.md)  
  
## <a name="restore-data-backups"></a>Восстановление резервных копий данных 
### <a name="using-ssms"></a>Использование среды SSMS 
-   [Restore a Database Backup Using SSMS](../../relational-databases/backup-restore/restore-a-database-backup-using-ssms.md)  
  
-   [Восстановление базы данных в новое место (SQL Server)](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)  
  
-   [Восстановление разностной резервной копии базы данных (SQL Server)](../../relational-databases/backup-restore/restore-a-differential-database-backup-sql-server.md)  
  
-   [Восстановление файлов и файловых групп (SQL Server)](../../relational-databases/backup-restore/restore-files-and-filegroups-sql-server.md)  
  
### <a name="using-t-sql"></a>Использование T-SQL
-   [Восстановление резервной копии базы данных в простой модели восстановления (Transact-SQL)](../../relational-databases/backup-restore/restore-a-database-backup-under-the-simple-recovery-model-transact-sql.md)  
  
-   [Восстановление базы данных до точки сбоя в модели полного восстановления (Transact-SQL)](../../relational-databases/backup-restore/restore-database-to-point-of-failure-full-recovery.md)  
  
-   [Восстановление файлов и файловых групп поверх существующих файлов (SQL Server)](../../relational-databases/backup-restore/restore-files-and-filegroups-over-existing-files-sql-server.md)  
  
-   [Восстановление файлов в новое место (SQL Server)](../../relational-databases/backup-restore/restore-files-to-a-new-location-sql-server.md)  
  
-   [Восстановление базы данных master (Transact-SQL)](../../relational-databases/backup-restore/restore-the-master-database-transact-sql.md)  

## <a name="restore-transaction-logs-full-recovery-model"></a>Восстановление журналов транзакций (модель полного восстановления)
### <a name="using-ssms"></a>Использование среды SSMS  
-   [Восстановление базы данных до помеченной транзакции (среда SQL Server Management Studio)](../../relational-databases/backup-restore/restore-a-database-to-a-marked-transaction-sql-server-management-studio.md)  
  
-   [Восстановление резервной копии журнала транзакций (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)  
  
-   [Восстановление базы данных SQL Server до определенного момента времени (модель полного восстановления)](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
  
 ### <a name="using-t-sql"></a>Использование T-SQL 
-   [Восстановление базы данных SQL Server до определенного момента времени (модель полного восстановления)](../../relational-databases/backup-restore/restore-a-sql-server-database-to-a-point-in-time-full-recovery-model.md)  
 
-   [Перезапуск прерванной операции восстановления (Transact-SQL)](../../relational-databases/backup-restore/restart-an-interrupted-restore-operation-transact-sql.md)  
  
-   [Восстановление базы данных без восстановления данных (Transact-SQL)](../../relational-databases/backup-restore/recover-a-database-without-restoring-data-transact-sql.md)  
 
## <a name="more-information-and-resources"></a>Дополнительные сведения и ресурсы
 [Общие сведения о резервном копировании (SQL Server)](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [Обзор процессов восстановления (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)   
 [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)   
 [Создание и восстановление резервных копий баз данных служб Analysis Services](https://docs.microsoft.com/analysis-services/multidimensional-models/backup-and-restore-of-analysis-services-databases)   
 [Создание резервных копий и восстановление полнотекстовых каталогов и индексов](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md)   
 [Создание резервной копии и восстановление из копий реплицируемых баз данных](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md)   
 [Журнал транзакций (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [Модели восстановления (SQL Server)](../../relational-databases/backup-restore/recovery-models-sql-server.md)   
 [Наборы носителей, семейства носителей и резервные наборы данных (SQL Server)](../../relational-databases/backup-restore/media-sets-media-families-and-backup-sets-sql-server.md)  
  
  

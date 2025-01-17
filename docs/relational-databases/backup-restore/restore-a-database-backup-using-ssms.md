---
title: Восстановление резервной копии базы данных с помощью среды SSMS | Документация Майкрософт
ms.custom: ''
ms.date: 11/16/2016
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.locatebackupfileazure.f1
- sql13.swb.specifybackup.f1
helpviewer_keywords:
- full backups [SQL Server]
- database restores [SQL Server], full backups
- backing up databases [SQL Server], full backups
- database backups [SQL Server], full backups
- restoring databases [SQL Server], full backups
ms.assetid: 24b3311d-5ce0-4581-9a05-5c7c726c7b21
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: fc461f1653c0d135df49384c0ad8706082fdff8d
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67937627"
---
# <a name="restore-a-database-backup-using-ssms"></a>Restore a Database Backup Using SSMS
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Этот раздел содержит сведения о восстановлении полной резервной копии базы данных с использованием среды SQL Server Management Studio.    
       
### <a name="important"></a>Важно!    
Перед восстановлением базы данных по модели полного восстановления или восстановления с неполным протоколированием, возможно, необходимо будет выполнить резервное копирование активного журнала транзакций (который называется [заключительным фрагментом журнала](tail-log-backups-sql-server.md)). Дополнительные сведения см. в статье [Создание резервной копии журнала транзакций (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)).  

При восстановлении базы данных из другого экземпляра примите во внимание сведения из раздела [Управление метаданными при обеспечении доступности базы данных на другом экземпляре сервера (SQL Server)](../../relational-databases/databases/manage-metadata-when-making-a-database-available-on-another-server.md).   
    
Чтобы восстановить зашифрованную базу данных, потребуется доступ к сертификату или асимметричному ключу, использовавшемуся для шифрования этой базы данных. Без сертификата или асимметричного ключа восстановить базу данных невозможно. Необходимо сохранять сертификат, используемый для шифрования ключа шифрования базы данных до тех пор, пока нужно хранить резервную копию. Дополнительные сведения см. в статье [SQL Server Certificates and Asymmetric Keys](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).    
    
Если восстановить базу данных более старой версии до [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], эта база данных будет автоматически обновлена до [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Это исключает возможность использования базы данных с более старой версией [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. Тем не менее это относится к обновлению метаданных и не влияет на [режим совместимости базы данных](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md). Если уровень совместимости пользовательской базы данных до обновления был 100 или выше, после обновления он останется таким же. Если уровень совместимости до обновления был 90, в обновленной базе данных он устанавливается в 100, что является минимально поддерживаемым уровнем совместимости в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Дополнительные сведения см. в разделе [Уровень совместимости инструкции ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  
  
Как правило, база данных сразу становится доступной. Однако если база данных [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] содержит полнотекстовые индексы, то в процессе обновления будет произведен их импорт, сброс или перестроение в зависимости от установленного значения свойства сервера **Режим обновления полнотекстового каталога** . Если выбран режим обновления **Импортировать** или **Перестроить**, то полнотекстовые индексы во время обновления будут недоступны. В зависимости от объема индексируемых данных процесс импорта может занять несколько часов, а повторная сборка — до десяти раз больше.     
    
Если выбран режим обновления **Импортировать**, а полнотекстовый каталог недоступен, то связанные с ним полнотекстовые индексы будут перестроены. Сведения о просмотре и изменении параметра **Режим обновления полнотекстового поиска** см. в статье [Наблюдение за полнотекстовым поиском для экземпляра сервера и управление им](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md).    

Сведения о восстановлении SQL Server из службы хранилища BLOB-объектов Microsoft Azure см. в разделе [Резервное копирование и восстановление SQL Server с помощью службы хранилища BLOB-объектов Microsoft Azure](../../relational-databases/backup-restore/sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service.md).

## <a name="examples"></a>Примеры
    
### <a name="a-restore-a-full-database-backup"></a>A. Восстановление полной резервной копии базы данных   
    
1.  В **обозревателе объектов**подключитесь к экземпляру компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] и разверните его.  
    
2.  Щелкните правой кнопкой мыши узел **Базы данных** и выберите команду **Восстановить базу данных...**    
    
3.  Чтобы указать источник и расположение восстанавливаемых резервных наборов данных, используйте страницу **Общие** , раздел **Источник** . Выберите один из следующих вариантов.    
    
    -   **База данных**    
    
         Выберите из раскрывающегося списка базу данных для восстановления. Данный список содержит только базы данных, резервное копирование которых было выполнено в соответствии с журналом резервного копирования **msdb** .    
    
        > [!NOTE]
        > Если резервная копия была получена с другого сервера, на целевом сервере не будет журнала резервного копирования для указанной базы данных. В этом случае щелкните пункт **Устройство** , чтобы вручную указать файл или устройство для восстановления. 
    
    -   **Устройство**    
    
         Нажмите кнопку обзора ( **...** ), после чего откроется диалоговое окно **Выбор устройств резервного копирования** . 
         
        -   Диалоговое окно**Выбор устройств резервного копирования** .  
        
            **Носитель данных резервной копии**  
         Выберите тип носителя в раскрывающемся списке **Тип носителя данных резервной копии** .  Примечание. Параметр **Лента** появляется только в случае, если на компьютере установлен ленточный накопитель, а параметр **Устройство резервного копирования** — только в случае, если имеется хотя бы одно устройство резервного копирования.

            **Добавить**  
            В зависимости от типа носителя данных, выбранного в поле **Носитель резервной копии** , при нажатии кнопки **Добавить** открывается одно из следующих диалоговых окон. (Если список в поле со списком **Тип носителя резервной копии** заполнен, кнопка **Добавить** недоступна.)

            |Тип носителя данных|.|Описание|    
            |----------------|----------------|-----------------|    
            |**Файл**|**Локальный файл резервной копии**|В данном диалоговом окне можно выбрать локальный файл из дерева или указать удаленный файл, используя его полное имя в формате UNC. Дополнительные сведения см. в разделе [Устройства резервного копирования (SQL Server)](../../relational-databases/backup-restore/backup-devices-sql-server.md).|    
            |**Устройство**|**Выбор устройства резервного копирования**|В данном диалоговом окне из списка можно выбрать логические устройства резервного копирования, определенные на экземпляре сервера.|    
            |**Лента**|**Выбор ленты с резервной копией**|В данном диалоговом окне из списка можно выбрать ленточные накопители, физически подключенные к компьютеру, на котором запущен экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|    
            |**URL-адрес**|**Выберите расположение файла резервной копии**|В этом диалоговом окне можно выбрать существующие учетные данные SQL Server или контейнер хранилища Azure, добавить новый контейнер хранилища Azure с подписанным URL-адресом или сформировать подписанный URL-адрес и учетные данные SQL Server для уже существующего контейнера хранилища.  См. также статью [Соединение с подпиской Microsoft Azure](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md)|  
         
             **Удалить**    
             Удаляет один или несколько выбранных файлов, лент или устройств резервного копирования.    
                 
             **Содержание**    
            Отображает содержимое носителя выбранного файла, ленты или устройства резервного копирования.  Эта кнопка может не работать, если тип носителя — **URL-адрес**.  

             **Тип носителя резервной копии**   
             Перечисляет выбранные носители.
    
             После добавления нужных устройств в списке **Носитель резервной копии** нажмите кнопку **ОК** для возвращения на страницу **Общие** .    
    
         В списке **Источник: Устройство: База данных** выберите имя базы данных, из которой нужно восстановить резервные копии.    
    
         > [!NOTE]
         > Данный список доступен, только если выбран параметр **Устройство** . Будут выбраны только те базы данных, резервные копии которых доступны на выбранном устройстве.    
     
4.  В разделе **Назначение** , в поле **База данных** автоматически появится имя базы данных для восстановления. Для изменения имени базы данных введите новое имя в окно **База данных** .    
    
5.  В поле **Восстановить до** оставьте значение по умолчанию **До последней выбранной резервной копии** или щелкните **Временную шкалу** , чтобы перейти в диалоговое окно **Временная шкала резервной копии** и выбрать вручную конкретное пороговое время восстановления. Дополнительные сведения о назначении конкретного момента времени см. в разделе [Backup Timeline](../../relational-databases/backup-restore/backup-timeline.md).    
    
6.  В сетке **Резервные наборы данных для восстановления** выберите нужные резервные наборы. В этой сетке отображаются резервные копии, доступные в указанном месте. По умолчанию предлагается план восстановления. Чтобы переопределить предложенный план восстановления, можно изменить выбранные элементы в сетке. Выбор всех резервных копий, которые зависят от восстановления более ранних копий, отменяется автоматически, как только отменяется выбор более ранних копий. Сведения о столбцах в сетке **Резервные наборы данных для восстановления** см. в статье [Восстановление базы данных (страница "Общие")](../../relational-databases/backup-restore/restore-database-general-page.md)).    
    
7.  При необходимости нажмите кнопку **Файлы** на панели **Выбор страницы** и перейдите в диалоговое окно **Файлы** . Отсюда можно восстановить базу данных в новое расположение, определив новое место восстановления для каждого файла в сетке **Восстановить файлы базы данных как** . Дополнительные сведения об этой сетке см. в разделе [Восстановление базы данных (страница "Файлы")](../../relational-databases/backup-restore/restore-database-files-page.md).    
    
8. Чтобы просмотреть или выбрать дополнительные параметры, на странице **Параметры** на панели **Параметры восстановления** вберите любой из следующих параметров, если он подходит к данной ситуации.    

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

    1.  Параметры**WITH** (необязательно):    
    
        -   **Перезаписать существующую базу данных (WITH REPLACE)**    
    
        -   **Сохранить параметры репликации (WITH KEEP_REPLICATION)**    
    
        -   **Ограничить доступ к восстановленной базе данных (WITH RESTRICTED_USER)**    
    
    2.  Выберите параметр в поле **Состояние восстановления** . В данном окне определяется состояние базы данных после операции восстановления.    
    
        -   По умолчанию установлена схема**RESTORE WITH RECOVERY** , при этом база данных находится в готовом состоянии для использования путем отката незафиксированных транзакций. Невозможно восстановить дополнительные журналы транзакций. Выберите данный параметр, если выполняется восстановление всех необходимых резервных копий.    
    
        -   Схема**RESTORE WITH NORECOVERY** оставляет базу данных в нерабочем состоянии и не выполняет откат незафиксированных транзакций. Можно восстановить дополнительные журналы транзакций. База данных не может быть использована, пока не будет восстановлена.    
    
        -   Схема**RESTORE WITH STANDBY** оставляет базу данных в режиме только для чтения. С помощью данного параметра можно отменить незафиксированные транзакции и сохранить отмененные действия в резервном файле, чтобы результаты восстановления можно было отменить.    
    
    3.  **Создайте резервную копию заключительного фрагмента журнала до восстановления** Не для всех сценариев восстановления требуется резервная копия заключительного фрагмента журнала.  Дополнительные сведения см. в разделе **Сценарии, в которых требуется резервная копия заключительного фрагмента журнала** статьи [Резервные копии заключительного фрагмента журнала (SQL Server).](../../relational-databases/backup-restore/tail-log-backups-sql-server.md)
    4.  Если имеются активные соединения с базой данных, то операция восстановления может завершиться ошибкой. Проверьте окно **Закрыть существующие соединения** и убедитесь, что все активные соединения между [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] и базой данных закрыты. Этот параметр переводит базу данных в однопользовательский режим перед началом выполнения процедуры восстановления, а затем возвращает в многопользовательский режим после ее завершения.    
    5.  Установите флажок **Выдавать запрос перед восстановлением каждой резервной копии** , если хотите отследить каждую операцию восстановления. Обычно это не требуется, за исключением случаев, если необходимо наблюдать за состоянием операции восстановления базы данных большого объема.    
    
Дополнительные сведения об этих параметрах восстановления см. в разделе [Восстановление базы данных (страница "Параметры")](../../relational-databases/backup-restore/restore-database-options-page.md).    
    
9. [!INCLUDE[clickOK](../../includes/clickok-md.md)] 

### <a name="b-restore-an-earlier-disk-backup-over-an-existing-database"></a>Б. Восстановление более ранней резервной копии диска поверх существующей базы данных
В следующем примере восстанавливается более ранняя резервная копия диска из базы данных `Sales` и перезаписывается существующая база данных `Sales`.

1.  В **обозревателе объектов**подключитесь к экземпляру компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] и разверните его.  
2.  Щелкните правой кнопкой мыши узел **Базы данных** и выберите команду **Восстановить базу данных...**  
3.  На странице **Общие** выберите пункт **Устройство** в разделе **Источник** .
4.  Нажмите кнопку обзора ( **...** ), после чего откроется диалоговое окно **Выбор устройств резервного копирования** . Щелкните **Добавить** и перейдите к резервной копии. Нажмите кнопку **ОК** , после того как будут выбраны файлы резервной копии диска.
5.  Нажмите кнопку **OK** , чтобы вернуться на страницу **Общие** .
6.  Нажмите кнопку **Параметры** на панели **Выбрать страницу** .
7.  в разделе **Параметры восстановления** установите флажок **Перезаписать существующую базу данных (WITH REPLACE)** .

    > [!NOTE]
    > Если вы не выберете этот параметр, может отобразиться следующее сообщение об ошибке: "System.Data.SqlClient.SqlError: The backup set holds a backup of a database other than the existing '`Sales`' database. (Microsoft.SqlServer.SmoExtended)"

8.  В разделе **Резервная копия заключительного фрагмента журнала** снимите флажок **Делать резервную копию заключительного фрагмента журнала перед восстановлением**.

    > [!NOTE]
    > Не для всех сценариев восстановления требуется резервная копия заключительного фрагмента журнала. Резервная копия заключительного фрагмента журнала не нужна, если точка восстановления содержится в более ранней резервной копии журнала. Кроме того, резервная копия заключительного фрагмента журнала не требуется при перемещении или замещении (перезаписи) базы данных, при котором не нужно восстанавливать ее на определенный момент времени после создания ее последней резервной копии. Дополнительные сведения см. в разделе [Резервные копии заключительного фрагмента журнала (SQL Server)](../../relational-databases/backup-restore/tail-log-backups-sql-server.md).

    Этот параметр недоступен для баз данных в ПРОСТОЙ модели восстановления.

9.  В разделе **Соединения с сервером** установите флажок **Закрыть существующие подключения к целевой базе данных**.

    > [!NOTE]
    > Если вы не выберете этот параметр, может отобразиться следующее сообщение об ошибке: "System.Data.SqlClient.SqlError: Не удалось получить монопольный доступ, так как база данных используется. (Microsoft.SqlServer.SmoExtended)"
    
10. [!INCLUDE[clickOK](../../includes/clickok-md.md)] 

### <a name="c--restore-an-earlier-disk-backup-with-a-new-database-name-where-the-original-database-still-exists"></a>В.  Восстановление более ранней резервной копии диска с новым именем базы данных при условии, что исходная база данных по-прежнему существует
В следующем примере восстанавливается более ранняя резервная копия диска из базы данных `Sales` и создается новая база данных с именем `SalesTest`.  При этом исходная база данных, `Sales`, все еще существует на сервере.

1.  В **обозревателе объектов**подключитесь к экземпляру компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] и разверните его.  
2.  Щелкните правой кнопкой мыши узел **Базы данных** и выберите команду **Восстановить базу данных...**  
3.  На странице **Общие** выберите пункт **Устройство** в разделе **Источник** .
4.  Нажмите кнопку обзора ( **...** ), после чего откроется диалоговое окно **Выбор устройств резервного копирования** . Щелкните **Добавить** и перейдите к резервной копии. Нажмите кнопку **ОК** , после того как будут выбраны файлы резервной копии диска.
5.  Нажмите кнопку **OK** , чтобы вернуться на страницу **Общие** .
6.  В разделе **Назначение** , в поле **База данных** автоматически появится имя базы данных для восстановления. Для изменения имени базы данных введите новое имя в окно **База данных** .
7.  Нажмите кнопку **Параметры** на панели **Выбрать страницу** .
8.  В разделе **Резервная копия заключительного фрагмента журнала** снимите флажок**Делать резервную копию заключительного фрагмента журнала перед восстановлением**.

    > [!IMPORTANT]
    > Если оставить этот флажок установленным, существующая база данных `Sales`сменит состояние на состояние восстановления.

9. [!INCLUDE[clickOK](../../includes/clickok-md.md)] 

    > [!NOTE]
    > Если вы получаете следующее сообщение об ошибке:      
    > "System.Data.SqlClient.SqlError: The tail of the log for the database "`Sales`" has not been backed up. Если журнал содержит работу, потеря которой нежелательна, создайте резервную копию с помощью инструкции `BACKUP LOG WITH NORECOVERY`. Чтобы просто перезаписать содержимое журнала, используются предложения `WITH REPLACE` или `WITH STOPAT` с инструкцией `RESTORE`. (Microsoft.SqlServer.SmoExtended)".      
    > Скорее всего, вы не ввели название новой базы данных из шага 6 выше. Восстановление обычно не допускает случайной перезаписи базы данных другой базой данных. Если указанная в инструкции `RESTORE` база данных уже существует на текущем сервере, а идентификатор GUID семейства для заданной базы данных отличается от идентификатора GUID семейства для базы данных, записанного в резервном наборе данных, то ее восстановление не будет выполнено. Это является важной защитной мерой.

### <a name="d--restore-earlier-disk-backups-to-a-point-in-time"></a>Г.  Восстановление предыдущих дисковых резервных копий на определенный момент времени
В следующем примере база данных восстанавливается в состояние на `1:23:17 PM``May 30, 2016` и демонстрируется операция восстановления, использующая несколько резервных копий журналов. Указанная база данных не существует на сервере.

1.  В **обозревателе объектов**подключитесь к экземпляру компонента [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] и разверните его.  
2.  Щелкните правой кнопкой мыши узел **Базы данных** и выберите команду **Восстановить базу данных...**  
3.  На странице **Общие** выберите пункт **Устройство** в разделе **Источник** .
4.  Нажмите кнопку обзора ( **...** ), после чего откроется диалоговое окно **Выбор устройств резервного копирования** . Щелкните **Добавить** и перейдите к полной резервной копии и всем соответствующим резервным копиям журнала транзакций.  Нажмите кнопку **ОК** , выбрав файлы резервной копии диска.
5.  Нажмите кнопку **OK** , чтобы вернуться на страницу **Общие** .
6.  В разделе **Назначение** щелкните **Временная шкала** , чтобы получить доступ к диалоговому окну **Временная шкала резервного копирования** и вручную выбрать момент остановки восстановления.
7.  Выберите **Указанные дата и время**.  
8.  В поле **Интервал временной шкалы** поменяйте значение на **Час** в раскрывающемся списке (необязательно).  
9.  Переместите ползунок в нужное время.
10. Нажмите кнопку **OK**, чтобы вернуться на страницу "Общие".
11. [!INCLUDE[clickOK](../../includes/clickok-md.md)] 

### <a name="e--restore-a-backup-from-the-microsoft-azure-storage-service"></a>Д.  Восстановление резервной копии с помощью службы хранилища Microsoft Azure

#### <a name="common-steps"></a>Общие шаги
В двух примерах ниже выполняется восстановление базы данных `Sales` из резервной копии, расположенной в службе хранилища Microsoft Azure.  Имя учетной записи хранилища — `mystorageaccount`.  Контейнер называется `myfirstcontainer`.  Для краткости первые шесть шагов перечислены здесь однократно, а все примеры начинаются с **шага 7**.
1.  В **обозревателе объектов**подключитесь к экземпляру компонента SQL Server Database Engine и разверните его.
2.  Щелкните правой кнопкой мыши узел **Базы данных** и выберите команду **Восстановить базу данных...**
3.  На странице **Общие** выберите пункт **Устройство** в разделе **Источник** .
4.  Нажмите кнопку обзора (...), после чего откроется диалоговое окно **Выбор устройств резервного копирования** .    
5.  Выберите **URL-адрес** в раскрывающемся списке **Тип носителя резервной копии:** .
6.  Нажмите кнопку **Добавить**, чтобы открыть диалоговое окно **Выберите расположение файла резервной копии**.

#### <a name="e1---restore-a-striped-backup-over-an-existing-database-and-a-shared-access-signature-exists"></a>E1.   Восстановление чередующейся резервной копии поверх существующей базы данных при наличии подписанного URL-адреса.
Хранимая политика доступа была создана с правами на чтение, запись, удаление и составление списков.  Подписанный URL-адрес, связанный с хранимой политикой доступа, был создан для контейнера `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`.  Шаги, в основном, одинаковы, если учетные данные SQL Server уже существуют.  База данных `Sales` в настоящее время существует на сервере.  Файлы резервной копии — `Sales_stripe1of2_20160601.bak` и `Sales_stripe2of2_20160601.bak`.  
*  
7.  Выберите `https://mystorageaccount.blob.core.windows.net/myfirstcontainer` из раскрывающегося списка **Контейнер хранилища Azure:** , если учетные данные SQL Server уже существуют. В противном случае введите имя контейнера вручную `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`. 
8.  Введите подписанный URL-адрес в поле форматированного текста **Подписанный URL-адрес:** .
9.  Нажмите кнопку **ОК** , чтобы открыть диалоговое окно **Поиск файла резервной копии в Microsoft Azure** .
10. Разверните узел **Контейнеры** и перейдите к `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`.
11. Удерживая клавишу ctrl, выберите файлы `Sales_stripe1of2_20160601.bak` и `Sales_stripe2of2_20160601.bak`.
12. Нажмите кнопку **ОК**.
13. Нажмите кнопку **OK** , чтобы вернуться на страницу **Общие** .
14. Нажмите кнопку **Параметры** на панели **Выбрать страницу** .
15. в разделе **Параметры восстановления** установите флажок **Перезаписать существующую базу данных (WITH REPLACE)** .
16. В разделе **Резервная копия заключительного фрагмента журнала** снимите флажок **Делать резервную копию заключительного фрагмента журнала перед восстановлением**.
17. В разделе **Соединения с сервером** установите флажок **Закрыть существующие подключения к целевой базе данных**.
18. Нажмите кнопку **ОК**.

#### <a name="e2---a-shared-access-signature-does-not-exist"></a>E2.   Подписанный URL-адрес не существует.
В этом примере база данных `Sales` не существует на сервере.
7.  Щелкните **Добавить** , чтобы открыть диалоговое окно **Соединение с подпиской Microsoft** .  
8.  Выполните все действия в диалоговом окне **Соединение с подпиской Microsoft** и нажмите кнопку **ОК** , чтобы вернуться в диалоговое окно **Выбор расположения файла резервной копии** .  См. дополнительные сведения в статье [Соединение с подпиской Microsoft Azure](../../relational-databases/backup-restore/connect-to-a-microsoft-azure-subscription.md) .
9.  Нажмите кнопку **ОК** в диалоговом окне **Выбор расположения файла резервной копии** , чтобы открыть диалоговое окно **Поиск файла резервной копии в Microsoft Azure** .
10. Разверните узел **Контейнеры** и перейдите к `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`.
11. Выберите файл резервной копии и нажмите кнопку **ОК**.
12. Нажмите кнопку **OK** , чтобы вернуться на страницу **Общие** .
13. Нажмите кнопку **ОК**.

#### <a name="f-restore-local-backup-to-microsoft-azure-storage-url"></a>Е. Восстановление локальной резервной копии в хранилище Microsoft Azure (URL)
База данных `Sales` будет восстановлена в контейнер хранилища Microsoft Azure `https://mystorageaccount.blob.core.windows.net/myfirstcontainer` из резервной копии, расположенной по адресу `E:\MSSQL\BAK`.  Учетные данные SQL Server для контейнера Azure уже созданы.  Учетные данные SQL Server для целевого контейнера уже должны быть созданы, поскольку создать их, выполнив задачу **Восстановить** , невозможно.  База данных `Sales` в настоящее время не существует на сервере.
1.  В **обозревателе объектов**подключитесь к экземпляру компонента SQL Server Database Engine и разверните его.
2.  Щелкните правой кнопкой мыши узел **Базы данных** и выберите команду **Восстановить базу данных...**
3.  На странице **Общие** выберите пункт **Устройство** в разделе **Источник** .
4.  Нажмите кнопку обзора (...), после чего откроется диалоговое окно **Выбор устройств резервного копирования** .  
5.  Выберите **Файл** в раскрывающемся списке **Тип носителя резервной копии:** .
6.  Нажмите кнопку **Добавить** , откроется диалоговое окно **Поиск файла резервной копии** .
7.  Перейдите к `E:\MSSQL\BAK`, выберите файл резервной копии и нажмите кнопку **ОК**.
8.  Нажмите кнопку **OK** , чтобы вернуться на страницу **Общие** .
9.  На панели **Выбор страницы** нажмите кнопку **Файлы** .
10. Установите флажок **Переместить все файлы в папку**.
11. Укажите контейнер, `https://mystorageaccount.blob.core.windows.net/myfirstcontainer`, в текстовых полях **Папка файла данных:** и **Папка файлов журнала:** .
12. Нажмите кнопку **ОК**.

## <a name="see-also"></a>См. также:    
 [Создание резервной копии журнала транзакций (SQL Server)](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)     
 [Создание полной резервной копии базы данных (SQL Server)](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)     
 [Восстановление базы данных в новое место (SQL Server)](../../relational-databases/backup-restore/restore-a-database-to-a-new-location-sql-server.md)     
 [Восстановление резервной копии журнала транзакций (SQL Server)](../../relational-databases/backup-restore/restore-a-transaction-log-backup-sql-server.md)     
 [RESTORE (Transact-SQL)](../../t-sql/statements/restore-statements-transact-sql.md)     
 [Восстановление базы данных (страница "Параметры")](../../relational-databases/backup-restore/restore-database-options-page.md)     
 [Восстановление базы данных (страница "Общие")](../../relational-databases/backup-restore/restore-database-general-page.md)    
    
  

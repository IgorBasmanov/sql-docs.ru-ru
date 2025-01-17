---
title: Агент слияния репликации | Документация Майкрософт
ms.custom: ''
ms.date: 10/29/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Merge Agent, executables
- Merge Agent, parameter reference
- agents [SQL Server replication], Merge Agent
- command prompt [SQL Server replication]
ms.assetid: fe1e7f60-b0c8-45e9-a5e8-4fedfa73d7ea
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 27de402dfe659be7c6adc28504f4d17ddc1e4620
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68085956"
---
# <a name="replication-merge-agent"></a>Replication Merge Agent
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Агент слияния репликации — это исполняемый файл программы, который применяет к подписчикам исходный моментальный снимок, содержащийся в таблицах базы данных. Кроме того, он выполняет слияние добавочных изменений данных, которые произошли на издателе после создания исходного моментального снимка, а также улаживает конфликты либо в соответствии с правилами, заданными пользователем, либо с помощью созданного пользователем сопоставителя.  
  
> [!NOTE]  
>  Параметры можно указывать в любом порядке. Если необязательные параметры не указаны, используются стандартные значения из реестра локального компьютера.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
replmerg [-?]   
-Publisher server_name[\instance_name]  
-PublisherDB publisher_database  
-Publication publication  
-Subscriber server_name[\instance_name]  
-SubscriberDB subscriber_database  
[-AltSnapshotFolder alt_snapshot_folder_path]  
[-Continuous]  
[-DefinitionFile def_path_and_file_name]  
[-DestThreads number_of_destination_threads]  
[-Distributor server_name[\instance_name]]  
[-DistributorLogin distributor_login]  
[-DistributorPassword distributor_password]  
[-DistributorSecurityMode [0|1]]  
[-DownloadGenerationsPerBatch download_generations_per_batch]  
[-DownloadReadChangesPerBatch download_read_changes_per_batch]  
[-DownloadWriteChangesPerBatch download_write_changes_per_batch]  
[-DynamicSnapshotLocation dynamic_snapshot_location]  
[-EncryptionLevel [0|1|2]]  
[-ExchangeType [1|2|3]]  
[-FastRowCount [0|1]]  
[-FileTransferType [0|1]]  
[-ForceConvergenceLevel [0|1|2 (Publisher|Subscriber|Both)]]  
[-FtpAddress ftp_address]  
[-FtpPassword ftp_password]  
[-FtpPort ftp_port]  
[-FtpUserNameftp_user_name]  
[-HistoryVerboseLevel [0|1|2|3]]  
[-Hostname host_name]  
[-InteractiveResolution [0|1]]  
[-InternetLogin internet_login]  
[-InternetPassword internet_password]  
[-InternetProxyLogin internet_proxy_login]  
[–InternetProxyPassword internet_proxy_password]  
[-InternetProxyServer internet_proxy_server]  
[-InternetSecurityMode [0|1]]  
[-InternetTimeout internet_timeout]  
[-InternetURL internet_url]  
[-KeepAliveMessageInterval keep_alive_message_interval_seconds]  
[-LoginTimeOut login_time_out_seconds]  
[-MakeGenerationInterval make_generation_interval_seconds]  
[-MaxBcpThreads number_of_threads]  
[-MaxDownloadChanges number_of_download_changes]  
[-MaxUploadChanges number_of_upload_changes]  
[-MetadataRetentionCleanup [0|1]]  
[-Output]  
[-OutputVerboseLevel [0|1|2]]  
[-ParallelUploadDownload [0|1]]  
[-PacketSize packet_size]   
[-PollingInterval polling_interval]  
[-ProfileName profile_name]  
[-PublisherFailoverPartner server_name[\instance_name] ]  
[-PublisherLogin publisher_login]  
[-PublisherPassword publisher_password]  
[-PublisherSecurityMode [0|1]]  
[-QueryTimeOut query_time_out_seconds]  
[-SrcThreads number_of_source_threads]  
[-StartQueueTimeout start_queue_timeout_seconds]  
[-SubscriberConflictClean [0|1]]  
[-SubscriberDatabasePath subscriber_path]  
[-SubscriberDBAddOption [0|1|2|3]]  
[-SubscriberLogin subscriber_login]  
[-SubscriberPassword subscriber_password   
[-SubscriberSecurityMode [0|1]]  
[-SubscriberType [0|1|2|3|4|5|6|7|8|9]]  
[-SubscriptionType [0|1|2]]  
[-SyncToAlternate [0|1]  
[-UploadGenerationsPerBatch upload_generations_per_batch]  
[-UploadReadChangesPerBatch upload_read_changes_per_batch]  
[-UploadWriteChangesPerBatch upload_write_changes_per_batch]  
[-UseInprocLoader]  
[-Validate [0|1|2|3]]  
[-ValidateInterval validate_interval]  
```  
  
## <a name="arguments"></a>Аргументы  
 **-?**  
 Выводит список всех доступных параметров.  
  
 **-Publisher** _server_name_[ **\\** _instance_name_]  
 Имя издателя. Укажите *server_name* , чтобы использовать экземпляр сервера [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] по умолчанию. Укажите _имя_сервера_ **\\** _имя_экземпляра_ для именованного экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] на этом сервере.  
  
 **-PublisherDB** _publisher_database_  
 Имя базы данных издателя.  
  
 **-Publication** _publication_  
 Имя публикации. Этот параметр допустим только в том случае, если в данной публикации моментальный снимок всегда доступен для новых или повторно инициализированных подписок.  
  
 **-Subscriber** _server_name_[ **\\** _instance_name_]  
 Имя подписчика. Укажите *server_name* , чтобы использовать экземпляр сервера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] по умолчанию. Укажите _имя_сервера_ **\\** _имя_экземпляра_ для именованного экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] на этом сервере.  
  
 **-SubscriberDB** _subscriber_database_  
 Имя базы данных подписчика.  
  
 **-AltSnapshotFolder** _alt_snapshot_folder_path_  
 Путь к папке, где хранится исходный моментальный снимок для подписки.  
  
 **-Continuous**  
 Определяет, будет ли агент постоянно опрашивать реплицируемые транзакции. Если параметр задан, то даже при отсутствии ожидающих транзакций агент опрашивает реплицируемые транзакции с источника через интервалы опроса.  
  
 **-DestThreads** _number_of_destination_threads_  
 Указывает число целевых потоков, которые агент слияния использует для применения изменений к целевому объекту. Целевым объектом при передаче является издатель, а при загрузке — подписчик. Значение по умолчанию — 4.  
  
 **-DefinitionFile** _def_path_and_file_name_  
 Путь к файлу определения агента. Файл определения агента содержит параметры командной строки для агента. Содержимое файла анализируется как для исполняемого файла. Для указания значений параметров, содержащих произвольные символы, используются двойные кавычки (").  
  
 **-Distributor** _server_name_[ **\\** _instance_name_]  
 Имя распространителя. Укажите *server_name* , чтобы использовать экземпляр сервера [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] по умолчанию. Укажите _server_name_ **\\** _instance_name_ , чтобы обратиться к именованному экземпляру [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] по умолчанию. При распространении (принудительном) с помощью распространителя по умолчанию используется имя применяемого по умолчанию экземпляра [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] на локальном компьютере.  
  
 **-DistributorLogin** _имя_входа_распространителя_  
 Имя входа распространителя.  
  
 **-DistributorPassword** _distributor_password_  
 Пароль распространителя.  
  
 **-DistributorSecurityMode** [ **0**| **1**]  
 Указывает режим безопасности распространителя. Значение **0** означает проверку подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (по умолчанию), а значение **1** — проверку подлинности Windows.  
  
 **-DownloadGenerationsPerBatch** _download_generations_per_batch_  
 Число поколений, которые должны быть обработаны в одном пакете при загрузке изменений от издателя на подписчик. Поколение — это логическая группа изменений для статьи. Для надежного канала связи значение по умолчанию равно 100, для ненадежного — 10.  
  
 **-DownloadReadChangesPerBatch** _download_read_changes_per_batch_  
 Число изменений, которые считываются в одном пакете при загрузке изменений от издателя на подписчик. Значение по умолчанию — 100.  
  
 **-DownloadWriteChangesPerBatch** _download_write_changes_per_batch_  
 Число изменений, применяемых в одном пакете при загрузке изменений от издателя на подписчик. Значение по умолчанию — 100.  
  
 **-DynamicSnapshotLocation** _dynamic_snapshot_location_  
 Местоположение файлов моментальных снимков фильтруемых данных в тех случаях, когда в публикации используются параметризованные фильтры строк.  
  
 **-EncryptionLevel** [ **0** | **1** | **2** ]  
 Уровень шифрования по протоколу SSL, используемый агентом слияния при установлении соединений.  
  
|Значение EncryptionLevel|Описание|  
|---------------------------|-----------------|  
|**0**|Указывает, что SSL не используется.|  
|**1**|Указывает, что SSL используется, но агент не проверяет, подписан ли сертификат сервера SSL надежным издателем.|  
|**2**|Указывает, что SSL используется и сертификат подтвержден.|  

 > [!NOTE]  
 >  Допустимый SSL-сертификат задается с полным доменным именем SQL Server. Если параметр -EncryptionLevel имеет значение 2, то для подключения агента создайте псевдоним на локальном сервере SQL Server. Для параметра Alias Name (Имя псевдонима) должно быть указано имя сервера, а для параметра Server (Сервер) — полное доменное имя SQL Server.

 Дополнительные сведения см. в статье [Просмотр и изменение параметров безопасности репликации](../../../relational-databases/replication/security/view-and-modify-replication-security-settings.md).  
  
 **-ExchangeType** [ **1**| **2**| **3**]  
> [!WARNING]
>  [!INCLUDE[ssNoteDepFutureDontUse](../../../includes/ssnotedepfuturedontuse-md.md)] Для ограничения передачи используйте **@subscriber_upload_options** вместо **sp_addmergearticle** .  
  
 Указывает тип обмена данными во время синхронизации, который может быть одним из следующих значений.  
  
|Значение ExchangeType|Описание|  
|------------------------|-----------------|  
|**1**|Агент должен передавать изменения данных с подписчика на издатель.|  
|**2**|Агент должен загружать изменения данных с издателя на подписчик.|  
|**3** (по умолчанию)|Агент должен сначала передавать изменения данных с подписчика на издатель, а затем загружать изменения данных с издателя на подписчик. Этот параметр следует указывать при выполнении веб-синхронизации.|  
  
 Статьи только для загрузки дают возможность управлять поведением синхронизации отдельных статей в публикации и могут способствовать повышению производительности. Дополнительные сведения см. в статье [Оптимизация производительности репликации слиянием при работе со статьями, доступными только для загрузки](../../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md).  
  
 При использовании **ExchangeType** для разделения этапов передачи и загрузки репликации слиянием на отдельные сеансы сначала необходимо запустить агент слияния с параметром **ExchangeType** , которому задано значение 1, а затем снова запустить агент слияния, задав этому параметру значение 2. Если не запустить агент слияния с обоими параметрами, метаданные будут удалены, из-за чего подписку придется инициализировать повторно (без передачи данных).  
  
 **-FastRowCount** [**0**|**1**]  
 Указывает тип метода вычисления количества строк, который должен применяться при проверке числа строк. Значение **1** (по умолчанию) указывает быстрый метод. Значение **0** указывает метод полного подсчета строк.  
  
 **-FileTransferType** [**0**|**1**]  
 Определяет тип передачи файла. **0** означает UNC (формат соглашения об универсальных именах), а **1** означает FTP (протокол передачи файлов).  
  
 **-ForceConvergenceLevel** [**0**|**1**|**2** ( **Publisher**| **Subscriber**| **Both**)]  
 Указывает уровень конвергенции, которого должен придерживаться агент слияния. Может принимать одно из следующих значений.  
  
|Значение ForceConvergenceLevel|Описание|  
|---------------------------------|-----------------|  
|**0** (по умолчанию)|По умолчанию. Выполнить стандартное слияние без дополнительной конвергенции.|  
|**1**|Принудительно включить конвергенцию для всех поколений.|  
|**2**|Принудительно включить конвергенцию для всех поколений и исправить поврежденные журналы преобразований. Если указано это значение, укажите также, где следует исправлять поврежденные журналы преобразований: на издателе, подписчике или в обоих местах.|  
  
 **-FtpAddress** _ftp_address_  
 Сетевой адрес службы FTP распространителя. Если этот параметр не указан, то по умолчанию применяется параметр **Distributor** .  
  
 **-FtpPassword** _ftp_password_  
 Пароль пользователя, используемый для подключения к службе FTP.  
  
 **-FtpPort** _ftp_port_  
 Номер порта службы FTP распространителя. Если не указан, используется порт службы FTP по умолчанию (21).  
  
 **-FtpUserName** _ftp_user_name_  
 Имя пользователя для соединения со службой FTP. Если имя не указано, используется имя входа «anonymous».  
  
 **-HistoryVerboseLevel** [**1**|**2**|**3**]  
 Указывает объем данных, регистрируемых в журнале при выполнении операции слияния. Выбрав значение **1**, можно свести к минимуму влияние ведения журнала на производительность.  
  
|Значение HistoryVerboseLevel|Описание|  
|-------------------------------|-----------------|  
|**0**|Регистрировать последнее сообщение о состоянии агента, подробные сведения о последнем сеансе и любые ошибки.|  
|**1**|В каждой записи о состоянии сеанса регистрировать дополнительные сведения о сеансе, в том числе процент выполнения операции, а также последнее сообщение о состоянии агента, подробные сведения о последнем сеансе и любые ошибки.|  
|**2**|По умолчанию. Регистрировать в каждой записи о состоянии сеанса как дополнительные сведения о сеансе, так и подробные сведения о сеансе на уровне статьи, в том числе процент выполнения операции, а также последнее сообщение о состоянии агента, подробные сведения о последнем сеансе и любые ошибки. Регистрируются также сообщения о состоянии агентов.|  
|**3**|То же, что и **-HistoryVerboseLevel** = **2**, с той лишь разницей, что регистрируется большее число сообщений о работе агентов.|  
  
 **-Hostname** _host_name_  
 Сетевое имя локального компьютера. По умолчанию используется имя локального компьютера.  
  
 **-InteractiveResolution** [**0**|**1**]  
 Указывает, используется ли интерактивный механизм устранения конфликтов в тех случаях, когда конфликт происходит в процессе синхронизации. Значение по умолчанию — **0**, то есть интерактивный механизм устранения конфликтов не применяется.  
  
 **-InternetLogin** _internet_login_  
 Указывает имя входа, используемое при соединении с ISAPI DLL прослушивателя репликации [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , который требует проверки подлинности.  
  
 **-InternetPassword** _internet_password_  
 Указывает пароль, используемый при соединении с ISAPI DLL прослушивателя репликации [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , который требует проверки подлинности.  
  
 **-InternetProxyLogin**  *internet_proxy_login*  
 Указывает имя входа, используемое при соединении с заданным в параметре *internet_proxy_server*прокси-сервером, который требует проверки подлинности.  
  
 **–InternetProxyPassword**  *internet_proxy_password*  
 Указывает пароль, используемый при соединении с заданным в параметре *internet_proxy_server*прокси-сервером, который требует проверки подлинности.  
  
 **-InternetProxyServer**  *internet_proxy_server*  
 Указывает прокси-сервер, который должен использоваться при обращении по протоколу HTTP к ресурсу, заданному в параметре *internet_url*.  
  
 **-InternetSecurityMode** [**0**|**1**]  
 Указывает режим безопасности IIS, используемый при соединении с веб-сервером в ходе веб-синхронизации. Значение **0** указывает на обычную проверку подлинности, а значение **1** — на встроенную проверку подлинности Windows (по умолчанию).  
  
 **-InternetTimeout** _internet_timeout_  
 Время в секундах, по истечении которого разрывается соединение с ISAPI DLL прослушивателя репликации [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 **-InternetURL** _internet_url_  
 Указывает URL-адрес, используемый для подключения к ISAPI DLL прослушивателя репликации [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Задание этого свойства обязательно.  
  
 **-KeepAliveMessageInterval** _keep_alive_message_interval_seconds_  
 Количество секунд до того, как поток журнала проверяет наличие соединений, ожидающих ответа от сервера. Это значение можно уменьшить, чтобы агент проверки не помечал агент слияния как подозрительный при выполнении долго выполняющегося пакета. Значение по умолчанию — **300** секунд.  
  
 **-LoginTimeOut** _время_ожидания_входа_в_сек_  
 Время ожидания входа в секундах. Значение по умолчанию составляет **15** секунд.  
  
 **-MakeGenerationInterval** _make_generation_interval_seconds_  
 Время ожидания (в секундах) между созданием поколений, или пакетов изменений, для загрузки на клиент. Значение по умолчанию составляет **15** секунд.  
  
 Makegeneration — процесс, который подготавливает изменения издателя для загрузки на подписчиков, и он может быть узким местом производительности во время загрузок. Если процесс makegeneration уже выполнялся в интервале, указанном параметром **-MakeGenerationInterval**, то процесс пропущен в текущем сеансе синхронизации. Это может способствовать параллелизму синхронизации, и особенно полезно, если подписчики не ожидают загрузки изменений.  
  
 **-MaxBcpThreads** _number_of_threads_  
 Указывает число операций массового копирования, которые можно проводить параллельно. Максимальное число одновременно существующих потоков и соединений ODBC, которое равно меньшему из двух значений: значению параметра **MaxBcpThreads** или числу запросов на массовое копирование, которое отображается в системной таблице **sysmergeschemachange** базы данных публикации. Значение**MaxBcpThreads** должно быть больше 0 и не имеет жестко зафиксированного максимума. Значение по умолчанию — **1**.  
  
 **-MaxDownloadChanges** _number_of_download_changes_  
 Указывает максимальное число измененных строк, которые должны загружаться с издателя на подписчик. Число загружаемых строк может быть больше указанного максимума, так как обработаны завершенные поколения, и может выполняться несколько параллельных потоков назначения, каждый из которых обрабатывает по крайней мере 100 изменений за первый проход. По умолчанию отправляются все изменения, готовые к загрузке.  
  
 **-MaxUploadChanges** _number_of_upload_changes_  
 Означает максимальное число измененных строк, которые должны быть переданы с подписчика на издатель. Число загружаемых строк может быть больше указанного максимума, так как обработаны завершенные поколения, и может выполняться несколько параллельных потоков назначения, каждый из которых обрабатывает по крайней мере 100 изменений за первый проход. По умолчанию отправляются все изменения, готовые к передаче.  
  
 **-MetadataRetentionCleanup** [**0**|**1**]  
 Указывает, удаляются ли метаданные из [MSmerge_genhistory](../../../relational-databases/system-tables/msmerge-genhistory-transact-sql.md), [MSmerge_contents](../../../relational-databases/system-tables/msmerge-contents-transact-sql.md), [MSmerge_tombstone](../../../relational-databases/system-tables/msmerge-tombstone-transact-sql.md), [MSmerge_past_partition_mappings](../../../relational-databases/system-tables/msmerge-past-partition-mappings-transact-sql.md)и [MSmerge_current_partition_mappings](../../../relational-databases/system-tables/msmerge-current-partition-mappings.md) в соответствии со сроком хранения публикации. Значение по умолчанию равно **1**, что означает необходимость проведения очистки. Значение **0** указывает на то, что очистка не должна осуществляться автоматически.  
  
 **-Output** _output_path_and_file_name_  
 Путь к выходному файлу агента. Если имя файла не указано, данные выводятся на консоль. Если указанный файл существует, то выходные данные добавляются в конец файла.  
  
 **-OutputVerboseLevel** [**0**|**1**|**2**]  
 Указывает, должны ли выводимые данные быть подробными. Если уровень подробностей равен **0**, выводятся только сообщения об ошибках. Если задан уровень детализации **1**, то выводятся все сообщения отчета о состоянии. Если уровень подробностей равен **2** (по умолчанию), выводятся и сообщения об ошибках, и сообщения отчета о состоянии, что удобно для отладки.  
  
 **-ParallelUploadDownload** [**0**|**1**]  
 Указывает, должен ли агент слияния параллельно обрабатывать изменения, переданные на издатель, и изменения, загруженные на подписчик. Это может оказаться удобным в средах с большими объемами данных и высокой пропускной способностью сети. Если параметр **ParallelUploadDownload** имеет значение **1**, включается параллельная обработка.  
  
 **-PacketSize**  
 Размер пакета в байтах. Значение по умолчанию равно 4 096 байт.  
  
 **-PollingInterval** _polling_interval_  
 Частота (в секундах), с которой издатель или подписчик выполняют запросы об изменении данных. Значение по умолчанию — 60 секунд.  
  
 **-ProfileName** _profile_name_  
 Указывает профиль агента, из которого берутся параметры агента. Если **ProfileName** имеет значение NULL, профиль агента отключен. Если значение **ProfileName** не указано, используется профиль по умолчанию для агентов этого типа. Дополнительные сведения см. в статье [Профили агента репликации](../../../relational-databases/replication/agents/replication-agent-profiles.md).  
  
 **-PublisherFailoverPartner** _server_name_[ **\\** _instance_name_]  
 Указывает партнера по обеспечению отработки отказа служб [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , участвующего в сеансе зеркального отображения базы данных с базой данных публикации. Дополнительные сведения см. в статье [Зеркальное отображение и репликация баз данных (SQL Server)](../../../database-engine/database-mirroring/database-mirroring-and-replication-sql-server.md).  
  
 **-PublisherLogin** _имя_входа_на_издателе_  
 Имя входа издателя. Данный параметр должен быть указан, если значение **PublisherSecurityMode** равно **0** (при проверке подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ).  
  
 **-PublisherPassword** _publisher_password_  
 Пароль издателя. Данный параметр должен быть указан, если значение **PublisherSecurityMode** равно **0** (при проверке подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ).  
  
 **-PublisherSecurityMode** [**0**|**1**]  
 Указывает режим безопасности издателя. Значение **0** означает проверку подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (по умолчанию), а значение **1** — проверку подлинности Windows.  
  
 **-QueryTimeOut** _query_time_out_seconds_  
 Время ожидания запроса в секундах. Значение по умолчанию — 300 секунд. Агент слияния также использует значение параметра **QueryTimeout** для определения времени ожидания формирования секционированного моментального снимка, когда это значение превышает отметку 1800.  
  
 **-SrcThreads** _number_of_source_threads_  
 Указывает число потоков на источнике, которые агент слияния использует для перечисления поступивших от него изменений. При передаче источником считается подписчик, а при загрузке — издатель. Значение по умолчанию — **3**.  
  
 **-StartQueueTimeout** _start_queue_timeout_seconds_  
 Максимальное время ожидания (в секундах) агента слияния в тех случаях, когда число параллельно выполняемых процессов слияния достигло предельного значения, указываемого свойством **@max_concurrent_merge** хранимой процедуры **sp_addmergepublication**. Если по истечении этого времени агент слияния все еще находится в процессе ожидания, то его работа будет завершена. Значение 0 означает, что агент ждет неопределенно долгое время, хотя его выполнение может быть отменено.  
  
 **-SubscriberDatabasePath** _subscriber_database_path_  
 Путь к базе данных Jet (MDB-файл) при значении **SubscriberType** равном **2** (позволяет соединиться с базой данных Jet без указания имени источника данных ODBC (DSN)).  
  
 **-SubscriberDBAddOption** [**0**| **1**| **2**| **3**]  
 Указывает, имеется ли существующая база данных подписчика.  
  
|Значение SubscriberDBAddOption|Описание|  
|---------------------------------|-----------------|  
|**0**|Использовать существующую базу данных (по умолчанию).|  
|**1**|Создать новую пустую базу данных подписчика.|  
|**2**|Создать новую базу данных и присоединить ее к указанному файлу.|  
|**3**|Создать новую базу данных, присоединить ее и включить все подписки, которые могут существовать в этом файле.|  
  
> [!NOTE]  
>  При указании значений **2** и **3**в параметре **SubscriberDatabasePath** необходимо также задать путь базы данных для подписчика.  
  
 **-SubscriberLogin** _subscriber_login_  
 Имя входа подписчика. Данный параметр должен быть указан, если значение **SubscriberSecurityMode** равно **0** (при проверке подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Authentication).  
  
 **-SubscriberPassword** _subscriber_password_  
 Пароль подписчика. Данный параметр должен быть указан, если значение **SubscriberSecurityMode** равно **0** (при проверке подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Authentication).  
  
 **-SubscriberSecurityMode** [ **0**| **1**]  
 Определяет режим безопасности подписчика. Значение **0** означает проверку подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (по умолчанию), а значение **1** — проверку подлинности Windows.  
  
 **-SubscriberConflictClean** [ **0**| **1**]  
 Указывает, выполняется ли очистка таблиц конфликтов на подписчике в процессе синхронизации. Значение **1** указывает на то, что таблицы на подписчике подвергаются очистке. Этот параметр используется только для подписок на публикации с децентрализованной регистрацией конфликтов.  
  
 **-SubscriberType** [ **0**| **1**| **3**| **4**| **5**| **6**| **7**| **8**]  
 Указывает тип соединения с подписчиком, используемый агентом слияния. Для этого параметра поддерживается только одно значение по умолчанию — **0** .  
  
 **-SubscriptionType**[ **0**| **1**| **2**]  
 Определяет тип подписки для распространения. Значение **0** указывает принудительную подписку (принимается по умолчанию), значение **1** — подписку по запросу, а значение **2** — анонимную подписку.  
  
 **-SyncToAlternate** [ **0|1**]  
 Указывает, осуществляет ли агент слияния процедуру синхронизации между подписчиком и альтернативным издателем. Значение **1** указывает на то, что это альтернативный издатель. Значение по умолчанию — **0**.  
  
 **-UploadGenerationsPerBatch** _upload_generations_per_batch_  
 Число поколений, которые должны обрабатываться в одном пакете при передаче изменений с подписчика на издатель. Поколение — это логическая группа изменений для статьи. Для надежного канала связи значение по умолчанию равно **100**, для ненадежного — **1**.  
  
 **-UploadReadChangesPerBatch** _upload_read_changes_per_batch_  
 Число изменений, которые считываются в одном пакете при передаче изменений с подписчика на издатель. Значение по умолчанию — **100**.  
  
 **-UploadWriteChangesPerBatch** _upload_write_changes_per_batch_  
 Число изменений, применяемых в одном пакете при передаче изменений с подписчика на издатель. Значение по умолчанию — **100**.  
  
 **-UseInprocLoader**  
 Повышает производительность исходного моментального снимка, предписывая агенту слияния применять его к подписчику командой BULK INSERT. Это устаревший параметр, несовместимый с типом данных XML. Этот параметр может использоваться, если не выполняется репликация XML-данных. Этот параметр нельзя использовать с моментальными снимками в символьном режиме. При использовании этого параметра учетная запись службы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] на подписчике должна иметь разрешение на чтение каталога, в котором находятся BCP-файлы данных моментальных снимков. Если этот параметр не указан, то загружаемый агентом драйвер ODBC считывает данные из файлов, поэтому контекст безопасности учетной записи службы [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] не применяется.  
  
 **-Validate** [**0**|**1**|**2**|**3**]  
 Указывает, следует ли осуществлять проверку в конце сеанса слияния и, если да, каков ее тип. Рекомендуется указывать значение **3** .  
  
|Значение Validate|Описание|  
|--------------------|-----------------|  
|**0** (по умолчанию)|Проверка не выполняется.|  
|**1**|Проверка достоверности только количества строк.|  
|**2**|Проверка по количеству строк и контрольной сумме.|  
|**3**|Проверка достоверности по количеству строк и двоичной контрольной сумме.|  
  
> [!NOTE]  
>  Проверка с использованием двоичной контрольной суммы может неверно сообщить об отказе, если типы данных на подписчике и издателе отличаются. Дополнительные сведения см. в разделе "Аспекты проверки данных" статьи [Проверка реплицированных данных](../../../relational-databases/replication/validate-data-at-the-subscriber.md).  
  
 **-ValidateInterval** _validate_interval_  
 Указывает частоту (в минутах) проверки подписки в непрерывном режиме. Значение по умолчанию равно **60** минут.  
  
## <a name="remarks"></a>Remarks  
  
> [!IMPORTANT]  
>  Если агент [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] установлен для запуска от учетной записи пользователя не домена (по умолчанию), а локальной системы, то служба имеет доступ только к локальному компьютеру. Если агент слияния, запускаемый агентом [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , настроен для входа в [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]с проверкой подлинности Windows, то работа агента слияния завершится ошибкой. Значением по умолчанию является проверка подлинности [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 Чтобы запустить агент слияния, из командной строки выполните файл **replmerg.exe** . Дополнительные сведения см. в разделе [Исполняемые объекты агента репликации](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
 Журнал агента слияния для текущего сеанса не удаляется при работе в непрерывном режиме. Длительная непрерывная работа агента может привести к появлению большого количества записей в таблицах журнала слияния, что может снизить производительность. Чтобы устранить эту проблему, переключитесь в режим работы по расписанию или продолжите работу в непрерывном режиме, но создайте выделенное задание, которое будет периодически перезапускать агент слияния. Также можно понизить уровень детализации журнала, чтобы уменьшить число строк, и таким образом уменьшить снижение производительности.  
  
## <a name="see-also"></a>См. также:  
 [Администрирование агента репликации](../../../relational-databases/replication/agents/replication-agent-administration.md)  
  
  

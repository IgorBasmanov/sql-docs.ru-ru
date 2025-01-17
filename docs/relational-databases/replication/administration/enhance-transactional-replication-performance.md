---
title: Повышение производительности репликации транзакций | Документация Майкрософт
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- publications [SQL Server replication], design and performance
- performance [SQL Server replication], transactional replication
- designing databases [SQL Server], replication performance
- performance [SQL Server replication], snapshot replication
- snapshot replication [SQL Server], performance
- subscriptions [SQL Server replication], performance considerations
- agents [SQL Server replication], performance
- Distribution Agent, performance
- transactional replication, performance
- Log Reader Agent, performance
ms.assetid: 67084a67-43ff-4065-987a-3b16d1841565
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2014||=sqlallproducts-allversions
ms.openlocfilehash: 2db87395b7170315e14e10db075a4d6ca5721ab3
ms.sourcegitcommit: 728a4fa5a3022c237b68b31724fce441c4e4d0ab
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2019
ms.locfileid: "68768789"
---
# <a name="enhance-transactional-replication-performance"></a>Повышение производительности репликации транзакций
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]
  После рассмотрения общих рекомендаций в отношении производительности, описываемых в разделе [Повышение общей производительности репликации](../../../relational-databases/replication/administration/enhance-general-replication-performance.md), необходимо проанализировать следующие дополнительные области, относящиеся к репликации транзакций.  
  
## <a name="database-design"></a>Структура базы данных  
  
-   Минимизируйте размер транзакций в структуре приложения.  
  
     По умолчанию репликация транзакций распространяет изменения в соответствии с границами транзакций. Если транзакции небольшие, агенту распространения скорее всего не придется повторно пересылать транзакцию из-за проблем в сети. Если агенту потребуется переслать транзакцию, то количество отсылаемых данных будет меньшим. 

  
## <a name="distributor-configuration"></a>Настройка распространителя  
  
-   Настройте распространитель на выделенном сервере.  
  
     Нагрузку, связанную с обработкой на издателе, можно снизить, настроив удаленный распространитель. Дополнительные сведения см. в разделе [Configure Distribution](../../../relational-databases/replication/configure-distribution.md).  
  
-   Установите надлежащий размер для базы данных распространителя.  
  
     Проверьте репликацию с типовой нагрузкой системы, чтобы определить пространство, необходимое для хранения команд. Убедитесь в том, что база данных имеет достаточный размер для хранения команд без частого автоматического увеличения размера. Дополнительные сведения об изменении размера базы данных см. в разделе [ALTER DATABASE (Transact-SQL)](../../../t-sql/statements/alter-database-transact-sql.md).  
  
## <a name="publication-design"></a>Разработка публикации  
  
-   Реплицируйте выполнение хранимых процедур при выполнении пакетных обновлений опубликованных таблиц.  
  
     При наличии пакетов обновлений, которые эпизодически воздействуют на большое количество строк на подписчике, следует рассмотреть возможность обновления опубликованной таблицы с использованием хранимой процедуры и опубликовать результаты выполнения этой хранимой процедуры. Вместо отправки обновления или удаления для каждой изменяемой строки агент распространителя выполняет эту процедуру на подписчике с теми же значениями параметров. Дополнительные сведения см. в статье [Publishing Stored Procedure Execution in Transactional Replication](../../../relational-databases/replication/transactional/publishing-stored-procedure-execution-in-transactional-replication.md).  
  
-   Распределение статей по нескольким публикациям.  
  
     Если невозможно использовать [параметр **-SubscriptionStreams**](#subscriptionstreams), попробуйте создать несколько публикаций. Распределение статей между этими публикациями позволяет репликации параллельно применять изменения на подписчиках.  
  
## <a name="subscription-considerations"></a>Вопросы использования подписок  
  
-   Если на одном и том же издателе имеется несколько публикаций, используйте независимые агенты вместо общих агентов (эта конфигурация устанавливается по умолчанию в мастере создания публикаций).  
  
-   Лучше выполнять агенты в непрерывном режиме, чем часто запускать их по расписанию.  
  
     Настройка агентов на непрерывное выполнение вместо создания часто выполняемых расписаний (например, ежеминутных) позволяет повысить производительность репликации за счет экономии времени на запусках и остановках агента. Если агент распространителя настроен на непрерывную работу, изменения пересылаются на другие серверы, соединенные в топологии, с малой задержкой. Дополнительные сведения см. в разделе:  
  
    -   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. [Указание расписаний синхронизации](../../../relational-databases/replication/specify-synchronization-schedules.md)  
  
## <a name="distribution-agent-and-log-reader-agent-parameters"></a>Параметры агента распространителя и агента чтения журнала  
Параметры профиля агента часто изменяют, чтобы увеличить пропускную способность агентов чтения журнала и распространения в системах OLTP с высоким трафиком. 

Было выполнено тестирование, чтобы определить оптимальные значения, которые позволяют повысить производительность для агентов чтения журнала и распространения. По результатам этого тестирования выяснилось, что именно рабочая нагрузка определяет использование соответствующих значений в соответствующем сценарии. Следовательно, настройка единственного значения не может повысить производительность во всех ситуациях. 

Основные результаты 
- Для *агента чтения журнала* с небольшими (не более 500 команд) транзакциями в рабочих нагрузках повысить пропускную способность можно, увеличив значение **ReadBatchSize**. Но для рабочих нагрузок с крупными транзакциями изменение этого значения не повлияет на производительность. 
    - При параллельной работе на одном сервере нескольких агентов чтения журнала и распространения использование более высокого значения **ReadBatchSize** приводит к состязанию в базе данных распространителя. 
- Для *агента распространения*
    - Увеличение **CommitBatchSize** может повысить пропускную способность. Недостатком является то, что в случае сбоя агенту распространения придется вернуться назад и повторно применить большое число транзакций. 
    - Увеличение значения **SubscriptionStreams** помогает повысить общую пропускную способность агента распространения, позволяя создать несколько подключений к подписчику для параллельного применения пакетов изменений. Но в зависимости от числа процессоров и других условий метаданных (таких как первичный ключ, внешние ключи, уникальные ограничения и индексы) использование более высокого значения SubscriptionStreams может привести к нежелательным результатам. Кроме того, если не удается осуществить или зафиксировать поток, агент распространения возвращается к режиму по умолчанию и пытается повторно обработать эти пакеты в одном потоке.


Дополнительные сведения о проведенном тестировании см. в записи блога [Optimizing replication agent profile parameters for better performance](https://blogs.msdn.microsoft.com/sql_server_team/optimizing-replication-agent-profile-parameters-for-better-performance/) (Оптимизация параметров профилей агента репликации для повышения производительности).


### <a name="log-reader-agent"></a>Агент чтения журнала.

#### <a name="readbatchsize"></a>ReadBatchSize
- Увеличьте значение параметра **-ReadBatchSize** для агента чтения журнала.  
  
Агент чтения журнала и агент распространителя поддерживают разные размеры пакетов для операций чтения и фиксации транзакций. По умолчанию размер пакетов составляет 500 транзакций. Агент чтения журнала считывает заданное количество транзакций из журнала независимо от того, отмечены они для репликации или нет. Когда в базу данных публикации сохраняется большое число транзакций, из которых только немногие отмечены для репликации, следует использовать параметр **-ReadBatchSize** для увеличения размера пакета, считываемого агентом чтения журнала. Этот параметр не применим для издателей Oracle.  

   - Рабочие нагрузки с небольшим (не более 500 команд) числом транзакций при выборе значения 5000 для параметра **ReadBatchSize** увеличивают скорость обработки команд. 
   - Для более крупных (с числом команд от 500 до 1000) рабочих нагрузок увеличение **ReadBatchSize** мало влияет на повышение производительности. Увеличение значения **ReadBatchSize** приводит к тому, что в базу данных распространителя за один цикл обмена сохраняется больше транзакций. Это увеличивает время доступности транзакций и команд для агента распространения и вызывает задержку при репликации.  

#### <a name="pollinginterval"></a>PollingInterval
- Уменьшите значение параметра **-PollingInterval** для агента чтения журнала.  
  
Параметр **-PollingInterval** задает частоту опроса журнала транзакций опубликованной базы данных для определения транзакций, подлежащих репликации. Значение по умолчанию — 5 секунд. При уменьшении этого значения журнал опрашивается чаще, что может привести к меньшей задержке доставки транзакций из базы данных публикации в базу данных распространителя. Однако необходимо найти баланс между уменьшением задержки и ростом нагрузки на сервер с увеличением частоты опросов.   
  
#### <a name="maxcmdsintran"></a>MaxCmdsInTran
- Чтобы устранить случайные, возникающие нерегулярно ограничения, используйте параметр **–MaxCmdsInTran** для агента чтения журнала.  
  
Параметр **–MaxCmdsInTran** задает максимальное количество инструкций, группируемых в транзакцию, когда агент чтения журнала записывает команды в базу данных распространителя. Использование этого параметра позволяет агенту чтения журнала и агенту распространителя разделять большие транзакции (состоящие из множества команд) на издателе на несколько меньших транзакций при применении команд на подписчике. Задание этого параметра может снизить вероятность состязаний на распространителе и уменьшить задержку при передаче данных между издателем и подписчиком. Поскольку исходная транзакция применяется меньшими по размеру блоками, подписчик может получить доступ к строкам большой логической транзакции издателя до завершения исходной транзакции, разбивая строгую атомарность транзакции. По умолчанию установлено значение **0**, сохраняющее границы транзакции издателя. Этот параметр не применим для издателей Oracle.  
  
   > [!WARNING]  
   >  Параметр**MaxCmdsInTran** не предназначен для постоянного использования. Он существует для разрешения ситуаций, в которых случайно было запущено большое количество операций DML в рамках одной транзакции (из-за чего возникают задержки при распределении команд до тех пор, пока вся транзакция не будет находиться в базе данных распространителя, поскольку удерживаются блокировки и т. д.). Если такая ситуация возникает регулярно, изучите приложение и уменьшите размер транзакций.  
  
### <a name="distribution-agent"></a>Агент распространителя

#### <a name="subscriptionstreams"></a>SubscriptionStreams
- Увеличьте параметр **–SubscriptionStreams** для агента распространения.  
  
Параметр **–SubscriptionStreams** может значительно повысить суммарную пропускную способность репликации. Он позволяет нескольким соединениям с подписчиком параллельно применять пакеты изменений, при этом сохраняя многие свойства транзакций, характерные для использования одиночного потока. Если одному из соединений не удается осуществить выполнение или фиксацию, все подключения прекратят выполнение текущего пакета, и агент будет использовать одиночный поток для повторных попыток выполнения поврежденных пакетов. Перед завершением фазы выполнения повторной попытки могут существовать временные несоответствия транзакций на подписчике. После успешной фиксации всех поврежденных пакетов подписчик возвращается в состояние согласованности транзакций.  
  
Значение для этого параметра агента можно указать, используя параметр **@subscriptionstreams** процедуры [sp_addsubscription (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md).  

Дополнительные сведения о реализации потоков подписки см. в руководстве по [использованию параметра subscriptionStream для репликации SQL](https://blogs.msdn.microsoft.com/repltalk/2010/03/01/navigating-sql-replication-subscriptionstreams-setting).
  
### <a name="blocking-monitor-thread"></a>Поток монитора блокировок

Агент распространения поддерживает поток монитора блокировок, который обнаруживает блокировки между сеансами. Если поток монитора блокировок обнаружит блокировку между сеансами, агент распространения переключится на один из сеансов, чтобы повторно попытаться применить текущий пакет команд.

Хотя поток монитора блокировок может обнаруживать блокировки между сеансами агента распространения, он не может сделать это в следующих ситуациях:
- один из сеансов, в которых произошла блокировка, не является сеансом агента распространения;
- взаимоблокировка сеансов вызвала зависание агента распространения.

В этом случае агент распространения скоординирует все сеансы, чтобы выполнить в них одновременную фиксацию, как только их команды будут выполнены. Взаимоблокировка в сеансах происходит при выполнении следующих условий:

- блокировка происходит между сеансами агента распространения и сеансом, не являющимся сеансом агента распространения;
- агент распространения ожидает выполнения команд во всех сеансах, чтобы скоординировать их для одновременной фиксации.

Например, вы задали для параметра *SubscriptionStreams* значение 8. Сеансы с 10 по 17 являются сеансами агента распространения. Сеанс 18 не является сеансом агента распространения. Сеанс 10 блокируется сеансом 18, а сеанс 18 — сеансом 11. Кроме того, сеансы 10 и 11 должны быть зафиксированы вместе. Однако агент распространения не может выполнить их фиксацию из-за блокировки. Следовательно, агент распространения не может скоординировать эти восемь сеансов для одновременной фиксации, пока сеансы 10 и 11 не завершат выполнение своих команд.

В результате ни один из сеансов не выполняет свои команды. По истечении времени, указанного в свойстве **QueryTimeout**, агент распространения отменяет все сеансы.

> [!Note]
> По умолчанию значение свойства **QueryTimeout** равно пяти минутам.

Во время ожидания запроса вы можете заметить следующие закономерности в счетчиках производительности агента распространения. 

- Значение счетчика производительности **Дист.: доставлено команд в секунду** всегда равно 0.
- Значение счетчика производительности **Дист.: доставлено транз. в секунду** всегда равно 0.
- Значение счетчика производительности **Дист.: задержка при доставке** увеличивается вплоть до устранения взаимоблокировки потоков.

В разделе "Агент распространения репликации" в электронной документации по SQL Server приводится следующее описание параметра *SubscriptionStreams*: "Если одному из соединений не удается осуществить выполнение или фиксацию, все подключения прекратят выполнение текущего пакета, и агент будет использовать одиночный поток для повторных попыток выполнения поврежденных пакетов".

Агент распространения воспользуется одним сеансом, чтобы повторить попытку выполнения пакета команд. После успешного применения пакета команд агент распространения возобновит использование нескольких сеансов без перезапуска.

#### <a name="commitbatchsize"></a>CommitBatchSize
- Увеличьте значение параметра **-CommitBatchSize** для агента распространителя.  
  
Фиксация набора транзакций требует постоянного объема служебных операций. При выполнении менее частых фиксаций большего количества транзакций постоянный объем служебных операций приходится на больший объем данных.  Увеличение значения CommitBatchSize (до 200) может повысить производительность, увеличивая число фиксируемых в подписчике транзакций. Однако выгода от увеличения значения этого параметра невелика, поскольку затраты на применение изменений управляются другими факторами, например максимальной пропускной способностью системы ввода-вывода диска, на котором хранится журнал. Кроме того, необходимо найти баланс между преимуществами и недостатками, ведь в случае любого сбоя, останавливающего работу агента распространения, ему приходится возвращаться далеко назад и повторно применять большее число транзакций. В сетях с низкой надежностью более низкое значение параметра может уменьшить число сбоев и число транзакций, которые придется повторно применять в случае сбоя.  
  

## <a name="see-more"></a>Подробнее
  
[Работа с профилями агента репликации](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)  
[Просмотр и изменение параметров командной строки агента репликации (SQL Server Management Studio)](../../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
[Replication Agent Executables Concepts](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  
  

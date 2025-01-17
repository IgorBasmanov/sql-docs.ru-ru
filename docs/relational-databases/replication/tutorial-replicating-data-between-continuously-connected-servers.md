---
title: Руководство. Настройка репликации между двумя полностью подключенными серверами (репликация транзакций) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- tutorials [SQL Server replication]
- replication [SQL Server], tutorials
- wizards [SQL Server replication]
ms.assetid: 7b18a04a-2c3d-4efe-a0bc-c3f92be72fd0
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 379a7fe83694307c9f4d981d000dc8b9457fa6c9
ms.sourcegitcommit: 728a4fa5a3022c237b68b31724fce441c4e4d0ab
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2019
ms.locfileid: "68769415"
---
# <a name="tutorial-configure-replication-between-two-fully-connected-servers-transactional"></a>Руководство. Настройка репликации между двумя полностью подключенными серверами (репликация транзакций)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Репликация транзакций — это хорошее решение для перемещения данных между постоянно подключенными серверами. С помощью мастера репликации можно легко настроить и администрировать топологию репликации. 

В этом руководстве описано, как настроить топологию для репликации транзакций для постоянно подключенных серверов. См. дополнительные сведения о [репликации транзакций](https://docs.microsoft.com/sql/relational-databases/replication/transactional/transactional-replication). 
  
## <a name="what-you-will-learn"></a>Новые знания  
Из этого руководства вы узнаете, как публиковать данные из одной базы данных в другую, используя репликацию транзакций.  

В этом учебнике рассматривается следующее.
> [!div class="checklist"]
> * создание издателя путем репликации транзакций;
> * создание подписчика для издателя транзакций;
> * проверка подписки и измерение задержки.
  
  
## <a name="prerequisites"></a>предварительные требования  
Это руководство для пользователей, которые умеют выполнять основные операции с базами данных и которые имеют ограниченный опыт репликации. Перед тем как приступить к работе с этим учебником, необходимо освоить [Учебник. Подготовка SQL Server к репликации](../../relational-databases/replication/tutorial-preparing-the-server-for-replication.md).  
  
Для работы с этим учебником требуется SQL Server, среда SQL Server Management Studio (SSMS) и база данных AdventureWorks.  
  
- На сервере-издателе (источник) установите следующее:  
  
   - Любой выпуск [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], кроме SQL Server Express или SQL Server Compact. Эти выпуски не могут быть издателями репликации.   
   - Образец базы данных [!INCLUDE[ssSampleDBUserInputNonLocal](../../includes/sssampledbuserinputnonlocal-md.md)] . В целях повышения безопасности образцы баз данных по умолчанию не устанавливаются.  
  
- На сервере-подписчике (целевом) установите любой выпуск [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], кроме [!INCLUDE[ssEW](../../includes/ssew-md.md)]. [!INCLUDE[ssEW](../../includes/ssew-md.md)] не может быть подписчиком при репликации транзакций.  
  
- Установите [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).
- Установите выпуск [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Скачайте [пример базы данных AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases). См. дополнительные сведения о [восстановлении базы данных в среде SSMS](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). 
 
>[!NOTE]
> - Репликация не поддерживается в экземплярах SQL Server, которые отличаются друг от друга больше, чем на две версии. См. дополнительные сведения о [поддерживаемых версиях SQL Server в топологии репликации](https://blogs.msdn.microsoft.com/repltalk/2016/08/12/suppported-sql-server-versions-in-replication-topology/).
> - В среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] необходимо подключиться к издателю и подписчику с помощью имени входа, которое является членом предопределенной роли сервера **sysadmin**. Дополнительные сведения о роли см. в статье [Роли уровня сервера](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/server-level-roles).  
  
  
**На изучение этого руководства потребуется примерно 60 минут**  
  
## <a name="configure-the-publisher-for-transactional-replication"></a>Настройка издателя для репликации транзакций
В этом разделе с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] создается публикация транзакций для публикации фильтрованного подмножества таблицы **Продукт** из примера базы данных [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Также в список доступа к публикации (PAL) добавляется имя входа SQL Server, используемое агентом распространителя.


### <a name="create-a-publication-and-define-articles"></a>Создание публикации и определение статей
1. Подключитесь к издателю в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], а затем разверните узел сервера.  
  
2. Щелкните правой кнопкой мыши элемент **Агент SQL Server** и выберите пункт **Запустить**. Прежде чем приступить к созданию публикации, необходимо запустить агент SQL Server. Если при выполнении этого действия агент не запускается автоматически, его нужно запустить вручную с помощью диспетчера конфигурации SQL Server. 
3. Разверните папку **Репликация**, щелкните правой кнопкой мыши папку **Локальные публикации** и выберите пункт **Создать публикацию**. После этого запустится мастер создания публикации:  

   ![Выделенные элементы для запуска мастера создания публикации](media/tutorial-replicating-data-between-continuously-connected-servers/newpublication.png)
  
  
3. На странице **База данных публикации** выберите [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] и нажмите кнопку **Далее**.  
  
4. На странице **Тип публикации** выберите **Публикация транзакций** и нажмите кнопку **Далее**:  

   ![Страница "Тип публикации" с выбранным типом публикации](media/tutorial-replicating-data-between-continuously-connected-servers/tranrepl.png)
  
5. На странице **Статьи** разверните узел **Таблицы** и установите флажок **Продукт**. Затем разверните узел **Продукт** и снимите флажки **ListPrice** и **StandardCost**. Выберите **Далее**.  

   ![Страница "Статьи" с выбранными статьями для публикации](media/tutorial-replicating-data-between-continuously-connected-servers/replarticles.png)
  
6. На странице **Фильтрация строк таблицы** нажмите кнопку **Добавить**.   
  
7. В диалоговом окне **Добавление фильтра** выберите столбец **SafetyStockLevel**. Щелкните стрелку вправо, чтобы добавить столбец в предложение WHERE инструкции фильтра запроса. После этого вручную введите следующий модификатор предложения WHERE:  
  
   ```sql  
   WHERE [SafetyStockLevel] < 500  
   ```
  
   ![Страница "Фильтрация строк таблицы" и диалоговое окно "Добавление фильтра"](media/tutorial-replicating-data-between-continuously-connected-servers/filter.png)
  
8. Нажмите кнопку **ОК**, а затем кнопку **Далее**.  
  
9. Установите флажок **Создать моментальный снимок немедленно и обеспечить доступ к нему для инициализации подписок** и нажмите кнопку **Далее**:  

   ![Страница "Агент моментальных снимков" с установленным флажком](media/tutorial-replicating-data-between-continuously-connected-servers/snapshot.png)
  
10. На странице **Безопасность агентов** снимите флажок **Использовать настройки безопасности агента моментальных снимков**.   
  
    Выберите **Настройки безопасности** для агента моментальных снимков. Введите <*имя_компьютера_издателя*> **\repl_snapshot** в поле **Учетная запись процесса**, укажите пароль для этой учетной записи и нажмите кнопку **ОК**.  

    ![Страница "Безопасность агентов" и диалоговое окно "Безопасность агента моментальных снимков"](media/tutorial-replicating-data-between-continuously-connected-servers/snapshotagentsecurity.png)
  
12. Повторите предыдущий шаг, чтобы указать <*имя_компьютера_издателя*> **\repl_logreader** в качестве учетной записи процесса для агента чтения журнала. Нажмите кнопку **ОК**.  

    ![Диалоговое окно "Безопасность агента чтения журнала" и страница "Безопасность агентов"](media/tutorial-replicating-data-between-continuously-connected-servers/logreaderagentsecurity.png)   

  
13. На странице **Завершение работы мастера** введите **AdvWorksProductTrans** в поле **Имя публикации** и нажмите кнопку **Готово**:  

    ![Страница "Завершение работы мастера" с именем публикации](media/tutorial-replicating-data-between-continuously-connected-servers/advworksproducttrans.png)
  
14. После создания публикации нажмите кнопку **Закрыть**, чтобы закрыть мастер. 

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

Если при попытке создать публикацию обнаруживается, что агент SQL Server не запущен, может возникнуть следующая ошибка. Она указывает на то, что публикация создана успешно, но при этом агент моментальных снимков запустить не удалось. В этом случае необходимо запустить агент SQL Server, а затем вручную запустить агент моментальных снимков. Инструкции приведены в следующем разделе. 

![Предупреждение о том, что агент моментальных снимков запустить не удалось](media/tutorial-replicating-data-between-continuously-connected-servers/snapshotagenterror.png)
    
  
### <a name="view-the-status-of-snapshot-generation"></a>Просмотр сведений о состоянии создания моментального снимка  
  
1. Подключитесь к издателю в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], а затем разверните узел сервера и папку **Репликация**.  
  
2. В папке **Локальные публикации** щелкните правой кнопкой мыши публикацию **AdvWorksProductTrans** и выберите пункт **Просмотр состояния агента моментальных снимков**:  
   ![Команды контекстного меню для просмотра сведений о состоянии агента моментальных снимков](media/tutorial-replicating-data-between-continuously-connected-servers/viewsnapshot.png)
  
3. Отобразятся сведения о текущем состоянии задания агента моментальных снимков для публикации. Перед тем как перейти к следующему разделу, убедитесь, что задание моментального снимка выполнено успешно.
          
Если агент SQL Server не был запущен при создании публикации, в сведениях о состоянии агента моментальных снимков для публикации будет указано, что он никогда не запускался. В таком случае выберите **Запустить**, чтобы запустить агент моментальных снимков: 

![Кнопка "Запустить" и изменение в сообщении о состоянии, информирующее о запуске агента моментальных снимков](media/tutorial-replicating-data-between-continuously-connected-servers/startsnapshotagent.png)
     
Если отображается сообщение об ошибке, ознакомьтесь с разделом [Устранение неполадок с агентом моментальных снимков](troubleshoot-tran-repl-errors.md#find-errors-with-the-snapshot-agent).


  
### <a name="add-the-distribution-agent-login-to-the-pal"></a>Добавление имени входа агента распространения в список доступа к публикации  
  
1. Подключитесь к издателю в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], а затем разверните узел сервера и папку **Репликация**.  
  
2. В папке **Локальные публикации** щелкните правой кнопкой мыши публикацию **AdvWorksProductTrans** и выберите пункт **Свойства**.  Откроется диалоговое окно **Свойства публикации**.    
  
   A. Выберите страницу **Список доступа к публикации** и нажмите кнопку **Добавить**.  
   Б. В диалоговом окне **Добавление доступа к публикации** выберите *<имя_компьютера_издателя*> **\repl_distribution** и нажмите кнопку **ОК**.
   
   ![Выбранные элементы для добавления имени входа в список доступа к публикации](media/tutorial-replicating-data-between-continuously-connected-servers/tranreplproperties.png)

Дополнительные сведения см. статье [Основные понятия программирования репликации](../../relational-databases/replication/concepts/replication-programming-concepts.md).  
  

## <a name="create-a-subscription-to-the-transactional-publication"></a>Создание подписки на публикацию транзакций
В рамках этого раздела к созданной ранее публикации добавляется подписчик. В этом руководстве используется удаленный подписчик (NODE2\SQL2016), но при необходимости подписку для издателя можно добавить локально. 

### <a name="create-the-subscription"></a>Создание подписки  
  
1. Подключитесь к издателю в среде [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], а затем разверните узел сервера и папку **Репликация**.  
  
2. В папке **Локальные публикации** щелкните правой кнопкой мыши публикацию **AdvWorksProductTrans** и выберите команду **Создать подписку**. Запустится мастер создания подписки: 
 
   ![Выбранные элементы для запуска мастера создания подписки](media/tutorial-replicating-data-between-continuously-connected-servers/newsub.png)     
  
3. На странице **Публикация** выберите публикацию **AdvWorksProductTrans** и нажмите кнопку **Далее**:  

   ![Страница "Публикация" с выбранной публикацией](media/tutorial-replicating-data-between-continuously-connected-servers/selectpub.png)
  
4. На странице **Расположение агента распространения** установите флажок **Выполнять все агенты в распространителе** и нажмите кнопку **Далее**.  Дополнительные сведения о подписках см. в статье [Подписка на публикации](https://docs.microsoft.com/sql/relational-databases/replication/subscribe-to-publications).

   ![Страница "Расположение агента распространения" с установленным флажком "Выполнять все агенты в распространителе"](media/tutorial-replicating-data-between-continuously-connected-servers/runagentsatdist.png)
  
5. На странице **Подписчики**, если имя экземпляра подписчика не отображается, выберите пункт **Добавить подписчик**, а затем выберите **Добавить подписчик SQL Server** из раскрывающегося списка. После этого откроется диалоговое окно **Подключение к серверу**. Введите имя экземпляра подписчика, а затем выберите **Подключиться**.  
    
   Добавив подписчик, установите флажок рядом с именем его экземпляра. Затем выберите пункт **Создать базу данных** в разделе **База данных подписки**.   

   ![Страница "Подписчики" с выбранными элементами для добавления сервера подписчика](media/tutorial-replicating-data-between-continuously-connected-servers/addsub.png)

7. Откроется диалоговое окно **Новая база данных**. Введите **ProductReplica** в поле **Имя базы данных**, нажмите кнопку **ОК** и затем кнопку **Далее**: 
  
   ![Указание имени для базы данных подписки](media/tutorial-replicating-data-between-continuously-connected-servers/productreplica.png)
  
8. На странице **Безопасность агента распространения** нажмите кнопку с многоточием ( **…** ). Введите <*имя_компьютера_издателя*> **\repl_distribution** в поле **Учетная запись процесса**, введите пароль этой учетной записи, нажмите кнопку **ОК**, а затем — кнопку **Далее**.

   ![Сведения об учетной записи в диалоговом окне "Безопасность агента распространения"](media/tutorial-replicating-data-between-continuously-connected-servers/adddistaccount.png)
  
9. Нажмите кнопку **Готово**, чтобы принять значения по умолчанию на оставшихся страницах и завершить работу мастера.  
  
### <a name="set-database-permissions-at-the-subscriber"></a>Установка разрешений базы данных на подписчике  
  
1. Установите подключение к подписчику в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Разверните узел **Безопасность**, щелкните правой кнопкой мыши **Имена для входа**, а затем выберите команду **Создать имя для входа**.     
  
   A. На странице **Общие** в разделе **Имя для входа** выберите **Найти** и добавьте имя для входа для <*имя_компьютера_подписчика*> **\repl_distribution**.

   Б. На странице **Сопоставления пользователей** назначьте базе данных **ProductReplica** членство с именем для входа **db_owner**. 

   ![Выбранные элементы для настройки имени для входа в подписчике](media/tutorial-replicating-data-between-continuously-connected-servers/loginforsub.png)

2. Нажмите кнопку **ОК**, чтобы закрыть диалоговое окно **Создание имени для входа**. 

  
### <a name="view-the-synchronization-status-of-the-subscription"></a>Просмотр сведений о состоянии синхронизации подписки  
  
1. Подключитесь к издателю в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Разверните узел сервера и папку **Репликация**.  
  
2. В папке **Локальные публикации** разверните публикацию **AdvWorksProductTrans**, щелкните правой кнопкой мыши подписку в базе данных **ProductReplica** и выберите пункт **Просмотр состояния синхронизации**. Отобразятся сведения о текущем состоянии синхронизации подписки:

   ![Выбранные элементы в диалоговом окне "Просмотр состояния синхронизации"](media/tutorial-replicating-data-between-continuously-connected-servers/viewsyncstatus.png)
3. Если подписка не отображается под публикацией **AdvWorksProductTrans**, нажмите клавишу F5 для обновления списка.  
  
Дополнительные сведения см. в разделе:  
- [Инициализация подписки с помощью моментального снимка](../../relational-databases/replication/initialize-a-subscription-with-a-snapshot.md)  
- [Создание принудительной подписки](../../relational-databases/replication/create-a-push-subscription.md)  
- [Подписка на публикации](../../relational-databases/replication/subscribe-to-publications.md)  

## <a name="measure-replication-latency"></a>Измерение задержки репликации
При работе с этим разделом необходимо использовать трассировочные маркеры, чтобы проверить состояние репликации изменений и определить задержку. Задержка определяет время, требуемое для отображения в подписчике изменений, внесенных в издателе.
  
1. Подключитесь к издателю в [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Разверните узел сервера, щелкните правой кнопкой мыши папку **Репликация** и выберите пункт **Запустить монитор репликации**:

   ![Команда "Запустить монитор репликации" в контекстном меню](media/tutorial-replicating-data-between-continuously-connected-servers/launchreplmonitor.png)

2. Разверните группу издателей на левой панели, затем разверните экземпляр издателя и выберите публикацию **AdvWorksProductTrans**.  
  
   A. Откройте вкладку **Трассировочные токены**.  
   Б. Выберите команду **Вставить трассировочный токен**.    
   в. Просмотрите затраченное время для трассировочного токена в следующих столбцах: **От издателя к распространителю**, **От распространителя к подписчику**, **Общая задержка**. Значение **Ожидание** указывает на то, что маркер еще не достиг указанной точки.

   ![Сведения для трассировочного маркера](media/tutorial-replicating-data-between-continuously-connected-servers/tracertoken.png)


Дополнительные сведения см. в разделе: 
- [Измерение задержки и проверка правильности соединений для репликации транзакций](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)
- [Поиск ошибок в агентах репликации транзакций](troubleshoot-tran-repl-errors.md)


## <a name="next-steps"></a>Следующие шаги
Вы успешно настроили издатель и подписчик для репликации транзакций. Теперь вы можете вставлять, обновлять и удалять данные в таблице **Продукт** на стороне издателя. После этого вы можете выполнить запрос к таблице **Продукт** в подписчике, чтобы просмотреть реплицированные изменения. 

В следующей статье будет описана настройка репликации слиянием:  

> [!div class="nextstepaction"]
> [Учебник. Настройка репликации между сервером и мобильными клиентами (репликация слиянием)](tutorial-replicating-data-with-mobile-clients.md)

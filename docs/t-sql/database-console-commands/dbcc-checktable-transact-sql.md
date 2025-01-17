---
title: DBCC CHECKTABLE (Transact-SQL) | Документы Майкрософт
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CHECKTABLE_TSQL
- DBCC_CHECKTABLE_TSQL
- DBCC CHECKTABLE
- CHECKTABLE
dev_langs:
- TSQL
helpviewer_keywords:
- indexed views [SQL Server], DBCC CHECKTABLE
- page integrity checks [SQL Server]
- consistency [SQL Server], tables
- consistency [SQL Server], indexed views
- DBCC CHECKTABLE statement
- integrity [SQL Server]
- low overhead checks
- table integrity checks [SQL Server]
ms.assetid: 0d6cb620-eb58-4745-8587-4133a1b16994
author: pmasl
ms.author: umajay
ms.openlocfilehash: 7352a7e2db64da959cbefaacee5c9f3d14a8a579
ms.sourcegitcommit: 495913aff230b504acd7477a1a07488338e779c6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2019
ms.locfileid: "68809879"
---
# <a name="dbcc-checktable-transact-sql"></a>DBCC CHECKTABLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Производит проверку целостности всех страниц и структур, составляющих таблицу или индексированное представление.

![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
    
## <a name="syntax"></a>Синтаксис    
    
```    
DBCC CHECKTABLE     
(    
    table_name | view_name    
    [ , { NOINDEX | index_id }    
     |, { REPAIR_ALLOW_DATA_LOSS | REPAIR_FAST | REPAIR_REBUILD }     
    ]     
)    
    [ WITH     
        { [ ALL_ERRORMSGS ]    
          [ , EXTENDED_LOGICAL_CHECKS ]     
          [ , NO_INFOMSGS ]    
          [ , TABLOCK ]     
          [ , ESTIMATEONLY ]     
          [ , { PHYSICAL_ONLY | DATA_PURITY } ]     
          [ , MAXDOP = number_of_processors ]    
        }    
    ]    
```    
    
## <a name="arguments"></a>Аргументы    
 *table_name* | *view_name*  
 Таблица или индексированное представление, для которой необходимо выполнять проверки целостности. Имена таблиц и представлений должны соответствовать правилам построения [идентификаторов](../../relational-databases/databases/database-identifiers.md).  
    
NOINDEX  
 Указывает, что тщательную проверку некластеризованных индексов для пользовательских таблиц выполнять не следует. Это уменьшает общее время выполнения. Аргумент NOINDEX не оказывает влияния на проверки системных таблиц, поскольку проверки целостности для индексов всех системных таблиц выполняются всегда.  
    
 *index_id*  
 Идентификатор индекса, для которого выполняются проверки целостности. Если указан аргумент *index_id*, инструкция DBCC CHECKTABLE выполняет проверки целостности только для этого индекса, включая кучу или кластеризованный индекс.  
    
REPAIR_ALLOW_DATA_LOSS | REPAIR_FAST | REPAIR_REBUILD  
 Указывает, что инструкции DBCC CHECKTABLE следует исправлять обнаруженные ошибки. Для использования параметра исправления база данных должна быть открыта в однопользовательском режиме.  
    
REPAIR_ALLOW_DATA_LOSS  
 Пытается устранить все обнаруженные ошибки. Эти исправления могут привести к частичной потере данных.  
    
REPAIR_FAST  
 Синтаксис сохраняется только в целях обратной совместимости. Действия по восстановлению не выполняются.  
    
REPAIR_REBUILD  
 Выполняет действия по восстановлению данных, которые можно выполнить без риска их потери. Это может быть быстрое восстановление (например, восстановление отсутствующих строк в некластеризованных индексах) или более ресурсоемкие операции (например, перестроение индекса).  
 Этот аргумент не исправляет ошибки, связанные с данными FILESTREAM.  
    
 > [!NOTE]  
 > Используйте аргументы REPAIR только как последнее средство. Для устранения ошибок рекомендуется восстановление из резервной копии. Операции восстановления не учитывают никакие ограничения, которые могут существовать для таблиц или между таблицами. Если указанная таблица включена в одно или несколько ограничений, рекомендуется выполнить инструкцию `DBCC CHECKCONSTRAINTS` после операции восстановления.
 > Если необходимо использовать аргумент REPAIR, выполните инструкцию `DBCC CHECKTABLE` без параметра восстановления, чтобы узнать требуемый уровень восстановления. При использовании уровня REPAIR_ALLOW_DATA_LOSS рекомендуется создать резервную копию базы данных перед выполнением инструкции `DBCC CHECKTABLE` с этим параметром.  
    
ALL_ERRORMSGS  
 Отображает неограниченное число ошибок. Все сообщения об ошибках выводятся по умолчанию. Указание или пропуск этого параметра не приводит к изменениям.  
    
EXTENDED_LOGICAL_CHECKS  
 При уровне совместимости 100 ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) и выше выполняются проверки логической согласованности индексированных представлений, XML-индексов и пространственных индексов, если они есть.  
 Дополнительные сведения см. в подразделе *Выполнение проверок логической целостности индексов* раздела [Замечания](#remarks) далее в этой статье.  
    
NO_INFOMSGS  
 Подавляет вывод всех информационных сообщений.  
    
TABLOCK  
 Указывает инструкции DBCC CHECKTABLE использовать разделяемую блокировку таблицы вместо создания внутреннего моментального снимка базы данных. Аргумент TABLOCK позволяет ускорить работу инструкции DBCC CHECKTABLE в условиях высокой загрузки, но снижает текущую согласованность данных для таблицы во время выполнения этой инструкции.  
    
ESTIMATEONLY  
 Отображает предполагаемый размер базы данных tempdb, необходимый для выполнения инструкции DBCC CHECKTABLE с указанными параметрами.  
    
PHYSICAL_ONLY  
 Ограничивает область проверки целостностью физической структуры страниц, заголовков записей и физической структуры сбалансированных деревьев. Параметр предназначен для выполнения облегченной проверки физической согласованности таблицы, а также может выявлять поврежденные страницы и общие сбои оборудования, которые могут привести к потере данных. Полный запуск инструкции DBCC CHECKTABLE может занять значительно больше времени, чем в предыдущих версиях. Это вызвано следующими причинами.  
 -   Логические проверки стали более сложными.  
 -   Усложнился ряд базовых структур, нуждающихся в проверке.  
 -   Добавлено много новых проверок для поддержки новых функций.  
   
Иными словами, указание аргумента PHYSICAL_ONLY может существенно снизить время выполнения инструкции DBCC CHECKTABLE для больших таблиц и поэтому рекомендуется для частого использования в рабочих системах. Также рекомендуется периодически производить полный запуск инструкции DBCC CHECKTABLE. Периодичность запуска зависит от факторов, индивидуальных для каждого предприятия и каждой производственной среды. Аргумент PHYSICAL_ONLY всегда неявно включает аргумент NO_INFOMSGS и не должен указываться вместе с параметрами исправления ошибок.  
    
 > [!NOTE]  
 > Указание аргумента PHYSICAL_ONLY приводит к пропуску инструкцией DBCC CHECKTABLE всех проверок данных FILESTREAM.  
    
DATA_PURITY  
 Указание значения аргумента приводит к выполнению инструкцией DBCC CHECKTABLE проверки таблицы на недействительность или выход из допустимого диапазона значений столбцов. Например, инструкция DBCC CHECKTABLE обнаруживает столбцы со значениями даты и времени, выходящими за допустимый диапазон значений типа данных **datetime**, либо столбцы типа данных **decimal** или приблизительных числовых типов данных с недопустимыми значениями масштаба или точности.  
 Проверки целостности значений столбцов включены по умолчанию, и для них не требуется указывать параметр DATA_PURITY. Для баз данных, обновляемых с предыдущих версий [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], имеется возможность использовать инструкцию DBCC CHECKTABLE WITH DATA_PURITY для нахождения и исправления ошибок для заданной таблицы, однако проверки столбцов-значений таблицы запрещены по умолчанию до тех пор, пока инструкция DBCC CHECKDB WITH DATA_PURITY работает с базой данных без ошибок. После этого инструкции DBCC CHECKDB и DBCC CHECKTABLE проверяют целостность данных в столбцах-значениях по умолчанию.  
 Ошибки проверки, передаваемые при наличии этого параметра, не могут быть устранены с помощью параметров восстановления DBCC. Дополнительные сведения об устранении этих ошибок вручную см. в статье 923247 базы знаний Майкрософт: [Устранение ошибки DBCC 2570 в SQL Server 2005 и более поздних версиях](https://support.microsoft.com/kb/923247).  
 Если указан аргумент PHYSICAL_ONLY, проверка целостности значений в столбцах не выполняется.  
    
MAXDOP  
 **Область применения**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (начиная с версии [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] с пакетом обновления 2 (SP2) до версии [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]).  
 
 Переопределяет параметр конфигурации, задающий **максимальный уровень параллелизма**, в **sp_configure** для инструкции. Значение MAXDOP может превышать значение, настроенное с помощью sp_configure. Если MAXDOP превышает значение, настроенное с помощью Resource Governor, ядро СУБД использует значение MAXDOP из Resource Governor, как описано в статье "ALTER WORKLOAD GROUP (Transact-SQL)". Все семантические правила, используемые параметром конфигурации max degree of parallelism, применимы при использовании указания запроса MAXDOP. Дополнительные сведения см. в разделе [Настройка параметра конфигурации сервера max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).  
    
 > [!NOTE]  
 > Если значение MAXDOP равно нулю, то сервер выбирает максимальную степень параллелизма.  
    
## <a name="remarks"></a>Remarks    
    
> [!NOTE]    
> Чтобы выполнить инструкцию DBCC CHECKTABLE для каждой таблицы в базе данных, используйте инструкцию [DBCC CHECKDB](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md).    
    
Для указанной таблицы инструкция DBCC CHECKTABLE выполняет следующие проверки.
-   Верны ли ссылки на страницы данных индекса, на страницы данных в строке, на превышающие размер страницы данные строки и на страницы больших объектов.    
-   Верен ли порядок сортировки индексов.    
-   Согласованы ли указатели.    
-   Обоснованы ли данные, содержащиеся на каждой из страниц, в том числе вычисляемые столбцы.    
-   Обоснованы ли смещения страниц.    
-   Все ли строки в базовой таблице имеют совпадающие строки в некластеризованных индексах, и наоборот.    
-   Все ли строки в секционированной таблице или индексе находятся в соответствующих секциях.    
-   Согласованность между файловой системой и таблицей на уровне ссылок при хранении данных **varbinary(max)** в файловой системе с помощью FILESTREAM.    
    
## <a name="performing-logical-consistency-checks-on-indexes"></a>Выполнение проверок логической целостности индексов    
Процедура проверки логической целостности индексов зависит от уровня совместимости базы данных следующим образом.
-   При уровне совместимости 100 ([!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]) и выше.    
    -   Если не указан NOINDEX, DBCC CHECKTABLE выполняет как физические, так и логические проверки целостности отдельной таблицы и всех ее некластеризованных индексов. Однако в XML-индексах, пространственных индексах и индексированных представлениях по умолчанию выполняются только проверки физической целостности.    
    -   Если указан параметр WITH EXTENDED_LOGICAL_CHECKS, выполняются логические проверки в индексированных представлениях, XML-индексах и пространственных индексах (при их наличии). По умолчанию проверки физической согласованности выполняются раньше, чем проверки логической согласованности. Если также указан параметр NOINDEX, выполняются только проверки логической согласованности.    
         Они проверяют согласованность внутренней таблицы индексов или объекта индекса с пользовательской таблицей, на которую он указывает. Для поиска выбросов создается внутренний запрос, выполняющий полную проверку пересечения внутренних и пользовательских таблиц. Выполнение этого запроса может крайне отрицательно сказаться на производительности, а ход его выполнения невозможно отследить. Поэтому рекомендуется указывать параметр WITH EXTENDED_LOGICAL_CHECKS только в тех случаях, когда возможно возникновение проблем с индексированием, не связанных с физическими повреждениями, или при неверных контрольных суммах на уровне страниц, или же при подозрении на повреждение оборудования на уровне столбцов.    
    -   Если это отфильтрованный индекс, инструкция DBCC CHECKDB выполняет проверку целостности, чтобы убедиться, что записи индекса удовлетворяют условию предиката фильтра.   
      
- Начиная с [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] дополнительные проверки материализованных вычисляемых столбцов, столбцов пользовательских типов и фильтруемых индексов по умолчанию не выполняются во избежание ресурсоемких вычислений выражений. Это изменение значительно сокращает длительность выполнения инструкции CHECKDB для баз данных, содержащих такие объекты. Однако проверки физической согласованности этих объектов проводятся всегда. Вычисление выражений в дополнение к уже имеющимся логическим проверкам (индексированное представление, индексы XML и пространственные индексы) производится только в том случае, если задан параметр EXTENDED_LOGICAL_CHECKS.
-  Если уровень совместимости меньше либо равен 90 ([!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]) и не указан параметр NOINDEX, DBCC CHECKTABLE выполняет как физические, так и логические проверки целостности отдельной таблицы или индексированного представления и всех его некластеризованных и XML-индексов. Пространственные индексы не поддерживаются.
    
 **Определение уровня совместимости базы данных**    
[Просмотр или изменение уровня совместимости базы данных](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)    
    
## <a name="internal-database-snapshot"></a>Моментальный снимок внутренней базы данных    
Для обеспечения согласованности транзакций, необходимой для выполнения этих проверок, инструкция DBCC CHECKTABLE использует внутренний моментальный снимок базы данных. Дополнительные сведения см. в статье [Просмотр размера разреженного файла снимка базы данных (Transact-SQL)](../../relational-databases/databases/view-the-size-of-the-sparse-file-of-a-database-snapshot-transact-sql.md) и разделе "Использование внутреннего моментального снимка базы данных в командах DBCC" статьи [DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md).
Если моментальный снимок не может быть создан или указана подсказка TABLOCK, инструкция DBCC CHECKTABLE запрашивает совмещаемую блокировку таблицы, чтобы обеспечить необходимую согласованность.
    
> [!NOTE]    
> Если инструкция DBCC CHECKTABLE выполняется для базы данных tempdb, она должна использовать разделяемую блокировку таблицы. Это обусловлено тем, что по соображениям, связанным с производительностью, моментальные снимки базы данных недоступны для базы данных tempdb. Это означает, что нельзя достичь требуемой согласованности транзакций.    
    
## <a name="checking-and-repairing-filestream-data"></a>Проверка и восстановление данных FILESTREAM    
Если для базы данных и таблицы включен режим FILESTREAM, то существует возможность хранения больших двоичных объектов (BLOB) типа **varbinary(max)** в файловой системе. При использовании инструкции DBCC CHECKTABLE для таблицы, хранящей данные типа BLOB в файловой системе, DBCC проверяет согласованность между файловой системой и базой данных на уровне ссылок.
Например, если столбец таблицы типа **varbinary(max)** использует атрибут FILESTREAM, инструкция DBCC CHECKTABLE проверит, находятся ли файлы и каталоги файловой системы в сопоставлении один к одному со столбцами, строками и значениями строк таблицы. DBCC CHECKTABLE может исправить повреждения при задании параметра REPAIR_ALLOW_DATA_LOSS. При восстановлении повреждений FILESTREAM инструкция DBCC удаляет все строки таблиц, в которых отсутствуют данные файловой системы, а также все каталоги и файлы, которые не сопоставлены строкам, столбцам и значениям столбцов в таблицах.
    
## <a name="checking-objects-in-parallel"></a>Проверка объектов в параллельном режиме    
По умолчанию инструкция DBCC CHECKTABLE выполняет проверку объектов параллельно. Степень параллелизма определяется автоматически обработчиком запросов. Максимальная степень параллелизма настраивается точно так же, как и для параллельных запросов. Чтобы ограничить максимальное число процессоров, доступных для проверки DBCC, используйте процедуру [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md). Дополнительные сведения см. в разделе [Настройка параметра конфигурации сервера max degree of parallelism](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).
Параллельную проверку можно отключить с помощью флага трассировки 2528. Дополнительные сведения см. в разделе [Флаги трассировки (Transact-SQL)](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).
    
> [!NOTE]    
> Во время выполнения операции DBCC CHECKTABLE байты, сохраненные в столбце побайтно упорядоченного определяемого пользователем типа, должны совпасть с вычисленной сериализацией значения пользовательского типа. В противном случае процедура DBCC CHECKTABLE сообщит о несогласованности.    
    
## <a name="understanding-dbcc-error-messages"></a>Основные сведения о сообщениях об ошибках DBCC    
После завершения выполнения инструкции DBCC CHECKTABLE заносится сообщение в журнал ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. При успешном выполнении команды DBCC сообщается об успешном завершении и количестве времени, затраченном на выполнение команды. Если выполнение команды DBCC прерывается до завершения проверки по причине ошибки, сообщение указывает на прерывание команды и приводит значение состояния и количество времени, затраченного на выполнение команды. В следующей таблице перечислены и описаны значения состояний, которые могут быть включены в сообщение.
    
|Состояние|Описание|    
|-----------|-----------------|    
|0|Возникла ошибка с номером 8930. Это указывает на повреждение метаданных, вызвавшее прекращение выполнения команды DBCC.|    
|1|Возникла ошибка с номером 8967. Внутренняя ошибка DBCC.|    
|2|При аварийном восстановлении базы данных произошла ошибка.|    
|3|Это указывает на повреждение метаданных, вызвавшее прекращение выполнения команды DBCC.|    
|4|Обнаружено нарушение доступа или утверждения.|    
|5|Возникла неизвестная ошибка, которая привела к прекращению выполнения команды DBCC.|    
    
## <a name="error-reporting"></a>Отчет об ошибках    
Всякий раз при обнаружении командой DBCC CHECKTABLE ошибки повреждения данных в папке LOG [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создается мини-файл дампа (`SQLDUMP*nnnn*.txt`). Если для экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] включены функции сбора данных об *использовании компонентов* и *отчетов об ошибках*, этот файл автоматически отправляется в корпорацию [!INCLUDE[msCoName](../../includes/msconame-md.md)]. Собранные данные используются для улучшения функциональности [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
Файл дампа содержит результаты выполнения инструкции DBCC CHECKTABLE и дополнительные диагностические сведения. Доступ к этому файлу ограничен списками управления доступом на уровне пользователей. Доступ ограничен учетной записью службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и членами роли sysadmin. По умолчанию роль sysadmin содержит всех членов группы Windows BUILTIN\Administrators и группы локальных администраторов. В случае ошибки процесса сбора данных команда DBCC не завершается ошибкой.
    
## <a name="resolving-errors"></a>Разрешение ошибок    
 Если инструкция DBCC CHECKTABLE выдает какие-либо ошибки, рекомендуется восстановить базу данных из резервной копии, а не запускать параметр REPAIR с параметрами исправления ошибок. Если резервная копия отсутствует, исправить выданные ошибки можно запуском параметра REPAIR. В конце списка ошибок указано, какой из параметров REPAIR следует использовать. Однако исправление ошибок с параметром REPAIR_ALLOW_DATA_LOSS может потребовать удаления некоторых страниц, которые могут содержать данные.    
Исправление ошибок может быть выполнено в пользовательской транзакции, позволяя откатить произведенные изменения. При выполнении отката исправлений база данных снова будет содержать ошибки и ее необходимо будет восстановить из резервной копии. После завершения исправления всех ошибок создайте резервную копию базы данных.
    
## <a name="result-sets"></a>Результирующие наборы    
Инструкция DBCC CHECKTABLE возвращает следующий результирующий набор. Тот же результирующий набор возвращается как при указании только имени таблицы, так и при указании любых параметров.
    
```
DBCC results for 'HumanResources.Employee'.    
There are 288 rows in 13 pages for object 'Employee'.    
DBCC execution completed. If DBCC printed error messages, contact your system administrator.    
```    
    
Инструкция DBCC CHECKTABLE возвращает следующий результирующий набор, если указан параметр ESTIMATEONLY:
    
```
Estimated TEMPDB space needed for CHECKTABLES (KB)     
--------------------------------------------------     
21    
(1 row(s) affected)    
DBCC execution completed. If DBCC printed error messages, contact your system administrator.    
```    
    
## <a name="permissions"></a>Разрешения    
Пользователь должен быть либо владельцем таблицы, либо членом предопределенной роли сервера sysadmin, предопределенной роли базы данных db_owner или предопределенной роли базы данных db_ddladmin.    
    
## <a name="examples"></a>Примеры    
    
### <a name="a-checking-a-specific-table"></a>A. Проверка указанной таблицы    
В приведенном ниже примере производится проверка целостности страниц данных таблицы `HumanResources.Employee` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
    
```sql    
DBCC CHECKTABLE ('HumanResources.Employee');    
GO    
```    
    
### <a name="b-performing-a-low-overhead-check-of-the-table"></a>Б. Выполнение облегченной проверки таблицы    
 В приведенном ниже примере производится упрощенная проверка таблицы `Employee` в базе данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].    
    
```sql    
DBCC CHECKTABLE ('HumanResources.Employee') WITH PHYSICAL_ONLY;    
GO    
```    
    
### <a name="c-checking-a-specific-index"></a>В. Проверка указанного индекса    
 В следующем примере производится проверка указанного индекса, полученного из `sys.indexes`.    
    
```sql    
DECLARE @indid int;    
SET @indid = (SELECT index_id     
              FROM sys.indexes    
              WHERE object_id = OBJECT_ID('Production.Product')    
                    AND name = 'AK_Product_Name');    
DBCC CHECKTABLE ('Production.Product',@indid);    
```    
    
## <a name="see-also"></a>См. также:    
[DBCC (Transact-SQL)](../../t-sql/database-console-commands/dbcc-transact-sql.md)     
[DBCC CHECKDB (Transact-SQL)](../../t-sql/database-console-commands/dbcc-checkdb-transact-sql.md)    
    
  

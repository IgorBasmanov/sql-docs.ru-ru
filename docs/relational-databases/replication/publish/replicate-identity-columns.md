---
title: Репликация столбцов идентификаторов | Документация Майкрософт
ms.custom: ''
ms.date: 10/04/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- identities [SQL Server replication]
- identity values [SQL Server replication]
- merge replication [SQL Server replication], identity range management
- publishing [SQL Server replication], identity columns
- transactional replication, identity range management
- identity columns [SQL Server], replication
ms.assetid: eb2f23a8-7ec2-48af-9361-0e3cb87ebaf7
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2014||=sqlallproducts-allversions
ms.openlocfilehash: 3429a9c1e99277c9113e1773e99c8bd58a1cc01a
ms.sourcegitcommit: 728a4fa5a3022c237b68b31724fce441c4e4d0ab
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2019
ms.locfileid: "68769830"
---
# <a name="replicate-identity-columns"></a>Репликация столбцов идентификаторов
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]
  Когда столбцу назначается свойство IDENTITY, [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] автоматически создает последовательные номера для новых строк, вставляемых в таблицу, которая содержит столбец идентификаторов. Дополнительные сведения см. в статье о [свойстве IDENTITY (Transact-SQL)](../../../t-sql/statements/create-table-transact-sql-identity-property.md). Так как столбцы идентификаторов могут быть включены как часть первичного ключа, важно исключить появление повторяющихся значений в столбцах идентификаторов. Для использования столбцов идентификаторов в топологии репликации, имеющей обновления на нескольких узлах, все узлы топологии репликации должны использовать разные диапазоны значений идентификаторов, чтобы исключить появление повторяющихся идентификаторов.  
  
 Например, для издателя может быть задано значение в диапазоне от 1 до 100, для подписчика А в диапазоне от 101 от 200, а для подписчика Б в диапазоне от 201 до 300. Если строка вставляется в издателе и идентификатор имеет значение, равное, например, 65, это значение реплицируется на все подписчики. Когда репликация вставляет данные на каждый подписчик, она не увеличивает значение столбца идентификаторов в таблице подписчика; вместо этого вставляется буквенное значение 65. Увеличение значения столбца идентификаторов вызывается только пользовательскими вставками, а не вставками агента репликации.  
  
 Репликация обрабатывает столбцы идентификаторов в публикациях и подписках всех типов, что позволяет управлять столбцами вручную или разрешать механизму репликации автоматически управлять столбцами.  
  
> [!NOTE]  
>  Добавление столбца идентификаторов в опубликованную таблицу не поддерживается, поскольку это может привести к расхождению данных при репликации столбца на подписчик. Значения в столбце идентификаторов на издателе зависят от порядка, в котором строки изменяемой таблицы хранятся физически. Строки могут храниться по-разному на подписчике. Поэтому значение для столбца идентификаторов может быть разным для одинаковых строк.  
  
## <a name="specifying-an-identity-range-management-option"></a>Указание режима управления диапазонами идентификаторов  
 Репликация предоставляет три режима управления диапазонами идентификаторов.  
  
-   Автоматический. Используется для репликации слиянием и репликации транзакций с обновлениями на подписчике. Укажите диапазоны размера для издателя и подписчиков, и репликация будет автоматически управлять назначением новых диапазонов. Репликация устанавливает параметр NOT FOR REPLICATION для столбца идентификаторов на подписчике, чтобы увеличение значения на подписчике вызывалось только пользовательскими вставками.  
  
    > [!NOTE]  
    >  Для получения новых диапазонов подписчики должны синхронизироваться с издателем. Поскольку диапазоны идентификаторов назначаются подписчикам автоматически, существует возможность исчерпания всех диапазонов идентификаторов каким-либо подписчиком, если он будет многократно повторять запросы новых диапазонов.  
  
-   Ручной. Используется для репликации моментальных снимков и репликации транзакций без обновлений на подписчике, для одноранговой репликации транзакций или если приложение должно программно управлять диапазонами идентификаторов. Если указывается ручной режим управления, следует обеспечить назначение диапазонов издателю и каждому подписчику, а также назначение новых диапазонов при использовании исходных диапазонов. Репликация устанавливает параметр NOT FOR REPLICATION для столбца идентификаторов на подписчике.  
  
-   Нет. Этот режим рекомендуется только для обеспечения обратной совместимости с предыдущими версиями [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] и доступен только из интерфейса хранимой процедуры для публикаций транзакций.  
  
 Сведения об указании варианта управления диапазонами идентификаторов см. в [этой статье](../../../relational-databases/replication/publish/manage-identity-columns.md).  
  
## <a name="assigning-identity-ranges"></a>Назначение диапазонов идентификаторов  
 Репликация слиянием и репликация транзакций используют разные методы для назначения диапазонов. Эти методы описаны в данном разделе.  
  
 Существует два типа диапазонов, которые необходимо учитывать при репликации столбцов идентификаторов: диапазоны, назначаемые издателю и подписчикам, и диапазон типа данных в столбце. Следующая таблица показывает доступные диапазоны для типов данных, обычно используемых в столбцах идентификаторов. Диапазон используется во всех узлах топологии. Например, если используется **smallint** , начинающийся с 1 с шагом 1, максимальное количество вставок для издателя и всех подписчиков равно 32 767. Реальное число вставок зависит от наличия промежутков в используемых значениях и от использования порогового значения. Дополнительные сведения о пороговых значениях см. в следующих разделах: «Репликация слиянием» и «Репликация транзакций с подписками, обновляемыми посредством очередей».  
  
 Если издатель после вставки исчерпывает свой диапазон идентификаторов, он может автоматически назначить новый диапазон при условии, что вставка была выполнена членом предопределенной роли **db_owner** базы данных. Если вставка была выполнена пользователем, не являющимся членом этой роли, следует выполнить [sp_adjustpublisheridentityrange &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-adjustpublisheridentityrange-transact-sql.md) от имени агента чтения журнала, агента слияния или пользователя, входящего в роль **db_owner**. Чтобы назначение нового диапазона для публикаций транзакций происходило автоматически, должен выполняться агент чтения журнала (по умолчанию агент функционирует в непрерывном режиме).  
  
> [!WARNING]  
>  Во время вставки большого пакета триггер репликации запускается только один раз, а не каждый раз при вставке строки. Это может привести к ошибке инструкции вставки, если диапазон идентификаторов исчерпан в ходе большой вставки, например при выполнении инструкции **INSERT INTO** .  
  
|Тип данных|Диапазон|  
|---------------|-----------|  
|**tinyint**|Режимом автоматического управления не поддерживается|  
|**smallint**|от -2^15 (-32 768) до 2^15-1 (32 767)|  
|**int**|от -2^31 (-2 147 483 648) до 2^31-1 (2 147 483 647)|  
|**bigint**|от -2^63 (-9 223 372 036 854 775 808) до 2^63-1 (9 223 372 036 854 775 807)|  
|**decimal** и **numeric**|от -10^38+1 до 10^38-1|  
  
> [!NOTE]  
>  Сведения о том, как создать автоматически увеличивающееся числовое значение, которое может использоваться в нескольких таблицах или вызываться из приложений без ссылки на какие-либо таблицы, см. в разделе [Порядковые номера](../../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
### <a name="merge-replication"></a>Репликация слиянием  
 Диапазоны идентификаторов управляются издателем и распространяются на подписчики агентом слияния (в иерархии переиздания диапазоны управляются корневым издателем и переиздающими подписчиками). Значения идентификаторов назначаются из пула на издателе. Если вы добавляете в публикацию статью со столбцом идентификаторов с помощью мастера создания публикации или хранимой процедуры [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md), нужно указать значения следующих параметров.  
  
-   Параметр **@identity_range** , управляющий размером диапазона идентификаторов, первоначально назначаемым издателю и подписчикам с клиентскими подписками.  
  
    > [!NOTE]  
    >  Для подписчиков, на которых выполняются предыдущие версии [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], этот параметр (вместо параметра **@pub_identity_range** ) также управляет размером диапазона идентификаторов на переиздающих подписчиках.  
  
-   Параметр **@pub_identity_range** , управляющий размером диапазона идентификаторов для переиздания, назначается подписчикам с серверными подписками (необходим для переиздания данных). Все подписчики с серверными подписками получают диапазон для переиздания, даже если они реально не переиздают данные.  
  
-   Параметр **@threshold** , который используется, чтобы определить, когда для подписки на [!INCLUDE[ssEW](../../../includes/ssew-md.md)] или предыдущую версию [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 Например, можно было бы указать 10000 для **@identity_range** и 500000 для **@pub_identity_range** . Издателю и всем подписчикам, на которых запущена [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] или более поздняя версия, включая подписчик с серверной подпиской, назначается основной диапазон 10 000. Подписчику с серверной подпиской также назначается основной диапазон 500 000, который может использоваться подписчиками, которые синхронизируются с переиздающим подписчиком (для статей публикации на переиздающем подписчике необходимо также указать параметры **@identity_range** , **@pub_identity_range** и **@threshold** ).  
  
 Каждый подписчик, использующий [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] или более позднюю версию, также получает дополнительный диапазон идентификаторов. Дополнительный диапазон равен по размеру основному диапазону. При исчерпании основного диапазона используется дополнительный диапазон, и агент слияния назначает подписчику новый диапазон. Новый диапазон становится дополнительным диапазоном, и процесс продолжается по мере использования подписчиком значений идентификаторов.  
  
  
### <a name="transactional-replication-with-queued-updating-subscriptions"></a>Репликация транзакций с подписками посредством очередей  
 Диапазоны идентификаторов управляются распространителем и передаются на подписчики агентом распространителя. Значения идентификаторов назначаются из пула на распространителе. Размер пула основан на размере типа данных и приращении, используемом для столбца идентификаторов. Если вы добавляете в публикацию статью со столбцом идентификаторов с помощью мастера создания публикации или хранимой процедуры [sp_addarticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md), нужно указать значения следующих параметров.  
  
-   Параметр **@identity_range** , который управляет размером диапазона идентификаторов, первоначально назначаемого всем подписчикам.  
  
-   Параметр **@pub_identity_range** , который управляет размером диапазона идентификаторов, назначаемого издателю.  
  
-   Параметр **@threshold** , который используется для определения необходимости в новом диапазоне идентификаторов для подписки.  
  
 Например, можно было бы указать 10000 для **@pub_identity_range** , 1000 для **@identity_range** (в предположении небольшого количества обновлений на подписчике) и 80 процентов для **@threshold** . После 800 вставок на подписчике (80 процентов от 1000) подписчику назначается новый диапазон. После 8000 вставок на издателе ему назначается новый диапазон. Когда назначается новый диапазон идентификаторов, появляется зазор в значениях диапазона идентификаторов в таблице. При указании более высоких пороговых значений зазоры становятся меньше, но при этом уменьшается отказоустойчивость системы: если агент распространителя не может быть запущен по каким-либо причинам, подписчик может быстрее исчерпать диапазон доступных идентификаторов.  
  
## <a name="assigning-ranges-for-manual-identity-range-management"></a>Назначение диапазонов для ручного управления диапазонами идентификаторов  
 Если указывается ручное управление диапазонами идентификаторов, следует убедиться, что издатель и каждый подписчик используют разные диапазоны идентификаторов. Например, рассмотрим таблицу на издателе со столбцом идентификаторов, определенным как `IDENTITY(1,1)`: столбец идентификаторов начинается с 1 и увеличивается с шагом 1 при каждой вставке строки. Если таблица на издателе имеет 5 000 строк и ожидается увеличение таблицы на протяжении существования приложения, издатель может использовать диапазон от 1 до 10 000. При наличии двух подписчиков подписчик А может использовать диапазон от 10 001 до 20 000, а подписчик Б может использовать диапазон от 20 001 до 30 000.  
  
 После инициализации подписчика с помощью моментального снимка или иным способом выполните DBCC CHECKIDENT, чтобы назначить подписчику начальную точку для его диапазона идентификаторов. Например, на подписчике А следовало бы выполнить `DBCC CHECKIDENT('<TableName>','reseed',10001)`. На подписчике B следовало бы выполнить `CHECKIDENT('<TableName>','reseed',20001)`.  
  
 Чтобы назначить новые диапазоны издателю или подписчикам, выполните DBCC CHECKIDENT и укажите новое значение для повторной установки начальных значений таблицы. Необходим какой-либо способ, чтобы определить, когда должен назначаться новый диапазон. Например, приложение могло бы иметь механизм, который обнаруживает, когда узел близок к исчерпыванию своего диапазона, и назначает новый диапазон, используя DBCC CHECKIDENT. Можно также добавить проверочное ограничение, чтобы исключить добавление строки, если это приведет к использованию значения идентификатора, находящегося за пределами диапазона.  
  
## <a name="handling-identity-ranges-after-a-database-restore"></a>Обработка диапазонов идентификаторов после восстановления базы данных  
 Если при восстановлении подписчика из резервной копии используется автоматическое управление диапазонами идентификаторов, новый диапазон значений идентификаторов запрашивается автоматически. Если издатель восстанавливается из резервной копии, следует убедиться, что издателю назначен соответствующий диапазон. Для репликации слиянием назначьте новый диапазон с помощью [sp_restoremergeidentityrange (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-restoremergeidentityrange-transact-sql.md). Для репликации транзакций определите максимальное использованное значение и затем установите начальную точку для новых диапазонов. Используйте следующую процедуру после восстановления базы данных публикации.  
  
1.  Остановите все действия на всех подписчиках.  
  
2.  Для каждой опубликованной таблицы, которая содержит столбец идентификаторов, выполните следующие действия.  

[!INCLUDE[freshInclude](../../../includes/paragraph-content/fresh-note-steps-feedback.md)]

    1.  В базе данных подписок на каждом подписчике выполните `IDENT_CURRENT('<TableName>')`.  
  
    2.  Запишите максимальное значение, найденное среди всех подписчиков.  
  
    3.  В базе данных публикаций на издателе выполните `DBCC CHECKIDENT(<TableName>','reseed',<HighestValueFound+1>`.  
  
    4.  В базе данных публикаций на издателе выполните `sp_adjustpublisheridentityrange <PublicationName>, <TableName>`.  
  
    > [!NOTE]  
    >  Если значение в столбце идентификаторов настроено на уменьшение, а не на увеличение, запишите минимальное найденное значение, а затем повторно задайте это значение в качестве начального условия.  
  
## <a name="see-also"></a>См. также:  
 [BACKUP (Transact-SQL)](../../../t-sql/statements/backup-transact-sql.md)   
 [DBCC CHECKIDENT (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-checkident-transact-sql.md)   
 [IDENT_CURRENT (Transact-SQL)](../../../t-sql/functions/ident-current-transact-sql.md)   
 [Свойство IDENTITY (Transact-SQL)](../../../t-sql/statements/create-table-transact-sql-identity-property.md)   
 [sp_adjustpublisheridentityrange (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-adjustpublisheridentityrange-transact-sql.md)  
  
  

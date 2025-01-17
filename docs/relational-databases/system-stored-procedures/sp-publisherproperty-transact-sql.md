---
title: sp_publisherproperty (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_publisherproperty
- sp_publisherproperty_TSQL
helpviewer_keywords:
- sp_publisherproperty
ms.assetid: 0ed1ebc1-a1bd-4aed-9f46-615c5cf07827
author: stevestein
ms.author: sstein
ms.openlocfilehash: 185ad0ad33419b20fffae9bff3e5562761ea7b31
ms.sourcegitcommit: 728a4fa5a3022c237b68b31724fce441c4e4d0ab
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2019
ms.locfileid: "68771171"
---
# <a name="sppublisherproperty-transact-sql"></a>sp_publisherproperty (Transact-SQL)
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

  Отображает или изменяет свойства издателя для [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] издателей, отличных от. Эта хранимая процедура выполняется на распространителе.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_publisherproperty [ @publisher = ] 'publisher'   
   [ , [ @propertyname = ] 'propertyname' ]   
   [ , [ @propertyvalue = ] 'propertyvalue' ]  
```  
  
## <a name="arguments"></a>Аргументы  
 [ **@publisher** =] **'***Издатель***'**  
 Имя разнородного издателя. параметр *Publisher* имеет тип **sysname**и не имеет значения по умолчанию.  
  
 [ **@propertyname** =] **'***PropertyName***'**  
 Имя устанавливаемого свойства. Аргумент *PropertyName* имеет тип **sysname**и может принимать одно из следующих значений.  
  
|Значение|Описание|  
|-----------|-----------------|  
|**ксактсетбатчинг**|Показывает, группируются ли транзакции на издателе для последующей обработки в транзакционно целостные наборы, известные как наборы транзакций. Значение **Enabled** означает, что наборы транзакций можно создать, что является значением по умолчанию. Значение Disabled означает, что существующие наборы транзакций обрабатываются без создания новых наборы транзакций.|  
|**параметре xactsetjob**|Разрешен ли запуск задания набора транзакций для создания набора транзакций. Значение **Enabled** означает, что задание по набору транзакций периодически выполняется для создания наборы транзакций на издателе. Значение Disabled означает, что наборы транзакций создаются только агент чтения журнала при опросе издателем изменений.|  
|**xactsetjobinterval**|Интервал между запусками задания набора транзакций, в минутах.|  
  
 Если аргумент *PropertyName* опущен, возвращаются все устанавливаемые свойства.  
  
 [ **@propertyvalue** =] **'***propertyvalue***'**  
 Новое значение свойства. *propertyvalue* имеет тип **sysname**и значение по умолчанию NULL. Если параметр *propertyvalue* опущен, возвращается текущее значение свойства.  
  
## <a name="result-sets"></a>Результирующие наборы  
  
|Имя столбца|Тип данных|Описание|  
|-----------------|---------------|-----------------|  
|**propertyname**|**sysname**|Возвращает следующие свойства публикации, которые могут быть установлены:<br /><br /> **ксактсетбатчинг**<br /><br /> **параметре xactsetjob**<br /><br /> **xactsetjobinterval**|  
|**PropertyValue**|**sysname**|Текущий параметр для свойства в столбце **PropertyName** .|  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Примечания  
 **sp_publisherproperty** используется в репликации транзакций для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] издателей, отличных от.  
  
 Если указан только *Издатель* , результирующий набор включает текущие параметры для всех свойств, которые могут быть установлены.  
  
 Если указано значение *PropertyName* , в результирующем наборе отображается только именованное свойство.  
  
 Если указаны все аргументы, указанное свойство изменяется и результирующий набор не возвращается.  
  
 При изменении свойства **xactsetjobinterval** для выполняющегося задания необходимо перезапустить задание, чтобы новый интервал вступил в силу.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** на распространителе могут выполнять **sp_publisherproperty**.  
  
## <a name="see-also"></a>См. также  
 [Configure the Transaction Set Job for an Oracle Publisher](../../relational-databases/replication/administration/configure-the-transaction-set-job-for-an-oracle-publisher.md)  (Настройка задания для набора транзакции в издателе Oracle)  
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

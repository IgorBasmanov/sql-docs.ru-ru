---
title: sp_copysnapshot (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_copysnapshot
- sp_copysnapshot_TSQL
helpviewer_keywords:
- sp_copysnapshot
ms.assetid: a012a32f-6f26-45bf-8046-b51cd7fec455
author: stevestein
ms.author: sstein
ms.openlocfilehash: 30e96ad145abdb123e5bc5540f74f23251d1a69e
ms.sourcegitcommit: 728a4fa5a3022c237b68b31724fce441c4e4d0ab
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2019
ms.locfileid: "68768890"
---
# <a name="spcopysnapshot-transact-sql"></a>sp_copysnapshot (Transact-SQL)
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

  Копирует папку моментальных снимков указанной публикации в папку, указанную в **@destination_folder** . Эта хранимая процедура выполняется на издателе в базе данных публикации. Данная хранимая процедура полезна для копирования моментального снимка на извлекаемые носители, такие как CD-ROM.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_copysnapshot [ @publication = ] 'publication', [ @destination_folder = ] 'destination_folder' ]  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @publication = ] 'publication'`Имя публикации, содержимое моментального снимка которого должно быть скопировано. Аргумент *publication* имеет тип **sysname**и не имеет значения по умолчанию.  
  
`[ @destination_folder = ] 'destination_folder'`Имя папки, в которую будет скопировано содержимое моментального снимка публикации. *destination_folder*имеет тип **nvarchar (255)** и не имеет значения по умолчанию. *Destination_folder* может быть альтернативным расположением, например на другом сервере, на сетевом диске или на съемном носителе (например, на компакт-дисках или съемных дисках).  
  
`[ @subscriber = ] 'subscriber'`Имя подписчика. Аргумент *Subscriber* имеет тип sysname и значение по умолчанию NULL.  
  
`[ @subscriber_db = ] 'subscriber_db'`Имя базы данных подписки. *subscriber_db* имеет тип sysname и значение по умолчанию NULL.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Примечания  
 **sp_copysnapshot** используется во всех типах репликации. Подписчики [!INCLUDE[msCoName](../../includes/msconame-md.md)] с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] версией 7,0 и более ранними версиями не могут использовать альтернативное расположение моментального снимка.  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** или предопределенной роли базы данных **db_owner** могут выполнять **sp_copysnapshot**.  
  
## <a name="see-also"></a>См. также  
 [Альтернативные расположения папки моментальных снимков](../../relational-databases/replication/snapshot-options.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

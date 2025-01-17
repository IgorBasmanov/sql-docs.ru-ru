---
title: sp_altermessage (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_altermessage_TSQL
- sp_altermessage
dev_langs:
- TSQL
helpviewer_keywords:
- sp_altermessage
ms.assetid: 1b28f280-8ef9-48e9-bd99-ec14d79abaca
author: stevestein
ms.author: sstein
ms.openlocfilehash: 0722bbc713804af6b2b97b5651df5b564d17a136
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68117796"
---
# <a name="spaltermessage-transact-sql"></a>sp_altermessage (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Изменяет состояние определяемых пользователем или системных сообщений в экземпляре [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Определяемые пользователем сообщения можно просматривать при помощи **sys.messages** представления каталога.  

  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_altermessage [ @message_id = ] message_number   ,[ @parameter = ]'write_to_log'  
   ,[ @parameter_value = ]'value'   
```  
  
## <a name="arguments"></a>Аргументы  
 [ **@message_id =** ] *message_number*  
 Номер ошибки сообщения из **sys.messages**. *message_number* — **int** без значения по умолчанию.  
  
`[ @parameter = ] 'write\_to\_log_'` Используется с **@parameter_value** для указания того, что сообщение для записи [!INCLUDE[msCoName](../../includes/msconame-md.md)] журнал приложений Windows. *write_to_log* — **sysname** без значения по умолчанию. *write_to_log* должно быть присвоено WITH_LOG или значение NULL. Если *write_to_log* имеет значение WITH_LOG или значение NULL, и значение для **@parameter_value** — **true**, сообщение записывается в журнал приложений Windows. Если *write_to_log* имеет значение WITH_LOG или значение NULL, а также значение **@parameter_value** — **false**, сообщение не всегда записывается в журнал приложений Windows, но может быть записи в зависимости от того, как произошла ошибка. Если *write_to_log* указан, значение **@parameter_value** также должен быть указан.  
  
> [!NOTE]  
>  Если сообщение заносится в журнал приложений Windows, оно также заносится и в журнал ошибок компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
`[ @parameter_value = ]'value_'` Используется с **@parameter** для указания того, что ошибка для записи [!INCLUDE[msCoName](../../includes/msconame-md.md)] журнал приложений Windows. *значение* — **varchar(5)** , не имеет значения по умолчанию. Если **true**, ошибка всегда записывается в журнал приложений Windows. Если **false**, ошибка не всегда записывается в журнал приложений Windows, но может записываться в зависимости от того, как произошла ошибка. Если *значение* указано, *write_to_log* для **@parameter** также должен быть указан.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 0 (успешное завершение) или 1 (неуспешное завершение)  
  
## <a name="result-sets"></a>Результирующие наборы  
 None  
  
## <a name="remarks"></a>Примечания  
 Последствия **sp_altermessage** с WITH_LOG параметр аналогична параметра инструкции RAISERROR WITH LOG, за исключением случаев, **sp_altermessage** изменяет поведение в журнале существующего сообщения. Если сообщение изменено с параметром WITH_LOG, это сообщение всегда записывается в журнал приложений Windows, независимо от того, как была вызвана ошибка. Даже если инструкция RAISERROR выполняется без параметра WITH_LOG, ошибка записывается в журнал приложений Windows.  
  
 Системные сообщения можно изменить с помощью **sp_altermessage**.  
  
## <a name="permissions"></a>Разрешения  
 Требуется членство в **serveradmin** предопределенной роли сервера.  
  
## <a name="examples"></a>Примеры  
 В следующем примере существующее сообщение `55001` записывается в журнал приложений Windows.  
  
```  
EXECUTE sp_altermessage 55001, 'WITH_LOG', 'true';  
GO  
```  
  
## <a name="see-also"></a>См. также  
 [RAISERROR (Transact-SQL)](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [sp_addmessage (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addmessage-transact-sql.md)   
 [sp_dropmessage (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropmessage-transact-sql.md)   
 [Системные хранимые процедуры (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  

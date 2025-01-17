---
title: sp_dropdistributor (Transact-SQL) | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sp_dropdistributor
- sp_dropdistributor_TSQL
helpviewer_keywords:
- sp_dropdistributor
ms.assetid: 0644032f-5ff0-4718-8dde-321bc9967a03
author: stevestein
ms.author: sstein
ms.openlocfilehash: 032ecf59a3ffba4a7a7a6f4739c92b688858d501
ms.sourcegitcommit: 728a4fa5a3022c237b68b31724fce441c4e4d0ab
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2019
ms.locfileid: "68768867"
---
# <a name="spdropdistributor-transact-sql"></a>sp_dropdistributor (Transact-SQL)
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

  Удаляет распространитель. Эта хранимая процедура выполняется на распространителе в любой базе данных, за исключением базы данных распространителя.  
  
 ![Значок ссылки на раздел](../../database-engine/configure-windows/media/topic-link.gif "Значок ссылки на раздел") [Синтаксические обозначения в Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
sp_dropdistributor [ [ @no_checks= ] no_checks ]   
    [ , [ @ignore_distributor= ] ignore_distributor ]  
```  
  
## <a name="arguments"></a>Аргументы  
`[ @no_checks = ] no_checks`Указывает, следует ли проверять зависимые объекты перед удалением распространителя. *no_checks* имеет **бит**и значение по умолчанию 0.  
  
 Если значение **равно 0**, **sp_dropdistributor** проверяет, что все объекты публикации и распространения в дополнение к распространителю удалены.  
  
 Если значение равно **1**, **sp_dropdistributor** удаляет все объекты публикации и распространения перед удалением распространителя.  
  
`[ @ignore_distributor = ] ignore_distributor`Указывает, выполняется ли эта хранимая процедура без соединения с распространителем. *ignore_distributor* имеет **бит**и значение по умолчанию **0**.  
  
 Если значение **равно 0**, **sp_dropdistributor** подключается к распространителю и удаляет все объекты репликации. Если **sp_dropdistributor** не удается подключиться к распространителю, хранимая процедура завершается ошибкой.  
  
 Если значение равно **1**, то соединение с распространителем не устанавливается, а объекты репликации не удаляются. Этот вариант используется при удалении распространителя или если он постоянно работает в режиме «вне сети». Объекты издателя на распространителе не удаляются до тех пор, пока распространитель не будет переустановлен.  
  
## <a name="return-code-values"></a>Значения кода возврата  
 **0** (успешное завершение) или **1** (сбой)  
  
## <a name="remarks"></a>Примечания  
 **sp_dropdistributor** используется во всех типах репликации.  
  
 Если на сервере существуют другие объекты издателя или распространения, **sp_dropdistributor** завершается ошибкой **@no_checks** , если для параметра не задано значение **1**.  
  
 Эта хранимая процедура должна быть выполнена после удаления базы данных распространителя путем выполнения **sp_dropdistributiondb**.  
  
## <a name="example"></a>Пример  
 [!code-sql[HowTo#sp_DropDistPub](../../relational-databases/replication/codesnippet/tsql/sp-dropdistributor-trans_1.sql)]  
  
## <a name="permissions"></a>Разрешения  
 Только члены предопределенной роли сервера **sysadmin** могут выполнять **sp_dropdistributor**.  
  
## <a name="see-also"></a>См. также  
 [Disable Publishing and Distribution](../../relational-databases/replication/disable-publishing-and-distribution.md)  (Отключение публикации и распространения)  
 [sp_adddistributor &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md)   
 [sp_changedistributor_property &#40;TRANSACT-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistributor-property-transact-sql.md)   
 [sp_helpdistributor (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [Хранимые процедуры репликации (Transact-SQL)](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  

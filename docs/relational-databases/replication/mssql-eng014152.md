---
title: MSSQL_ENG014152 | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG014152 error
ms.assetid: 4215e2b1-cd30-441f-9671-9afc742adf6e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2014||=sqlallproducts-allversions
ms.openlocfilehash: ff2f443d5aa1811b55c9202d902b8713f6195621
ms.sourcegitcommit: 728a4fa5a3022c237b68b31724fce441c4e4d0ab
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2019
ms.locfileid: "68766053"
---
# <a name="mssqleng014152"></a>MSSQL_ENG014152
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]
    
## <a name="message-details"></a>Сведения о сообщении  
  
|||  
|-|-|  
|Название продукта|SQL Server|  
|Идентификатор события|14152|  
|Источник события|MSSQLSERVER|  
|Компонент|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Символическое имя||  
|Текст сообщения|Репликация-%s: назначено повторное выполнение агента %s. %s|  
  
## <a name="explanation"></a>Объяснение  
 Для указанного агента репликации было запланировано повторное выполнение запрошенной операции. Повторные попытки продолжаются до тех пор, пока не будет достигнуто максимальное число попыток, настроенное для шага задания.  
  
 Причиной повторного выполнения обычно становится одно из следующего.  
  
-   Превышено значение **QueryTimeout** .  
  
-   Превышено значение **LoginTimeout** .  
  
-   Процесс репликации был выбран как жертва взаимоблокировки.  
  
## <a name="user-action"></a>Действие пользователя  
 Если сообщение о повторном выполнении появляется нечасто, никакие пользовательские действия не требуются.  
  
 Используйте процедуру [sp_help_jobstep](../../relational-databases/system-stored-procedures/sp-help-jobstep-transact-sql.md) для проверки текущего значения максимального числа повторений шага **Запустить агент** для указанного агента репликации. Для корректировки допустимого числа повторений шага задания можно использовать параметр **@retry_attempts** хранимой процедуры [sp_update_jobstep](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md) .  
  
 Если сообщение о повторном выполнении появляется часто, способ устранения неполадок зависит от сообщения, вызывающего повтор. Проверьте в журнале агента наличие сообщений, указывающих причину назначения повторного выполнения. В некоторых случаях может потребоваться включить ведение более подробного журнала для агента репликации. Дополнительные сведения о настройке ведения журнала репликации см. в статье базы знаний Майкрософт [312292](https://support.microsoft.com/kb/312292).  
  
## <a name="see-also"></a>См. также:  
 [Справочник по ошибкам и событиям (репликация)](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  

---
title: Параметр конфигурации сервера "external scripts enabled" | Документы Майкрософт
ms.date: 11/13/2017
ms.prod: sql
ms.technology: configuration
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- external scripts enabled
- external_scripts_enabled_TSQL
helpviewer_keywords:
- external scripts enabled option
ms.assetid: 9d0ce165-8719-4007-9ae8-00f85cab3a0d
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 424268eb32eb3430e2e4eb8450abfb3471f51701
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68011764"
---
# <a name="external-scripts-enabled-server-configuration-option"></a>Параметр конфигурации сервера external scripts enabled
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
**Применимо к:** [!INCLUDE[sssql15-md](../../includes/sssql15-md.md)] [!INCLUDE[rsql-productname-md](../../includes/rsql-productname-md.md)] и [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)] [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)]

Параметр **external scripts enabled** позволяет включить выполнение скриптов с некоторыми удаленными расширениями языка. По умолчанию это свойство отключено. Программа установки может при необходимости задать этому свойству значение true, если установлены **расширенные службы аналитики**.

## <a name="remarks"></a>Remarks

Параметр external scripts enabled необходимо включить перед выполнением внешнего скрипта с помощью процедуры [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) . Используйте **sp_execute_external_script**, чтобы выполнять в скрипты, написанные на нескольких поддерживаемых языках, таких как R или Python. 

+ При анализе служб [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]

    [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] включает поддержку языка R в [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], а также набор инструментов рабочей станции R и библиотек подключений.

    Установите компонент **Расширения углубленной аналитики** в ходе установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , чтобы включить выполнение скриптов R. Язык R устанавливается по умолчанию.

+ Для [[!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]

    [!INCLUDE[rsql-productnamenew-md](../../includes/rsql-productnamenew-md.md)] использует ту же архитектуру, что и SQL Server 2016, но поддерживает язык Python.

    Установите компонент **Расширения углубленной аналитики** в ходе установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], чтобы включить выполнение внешних скриптов R. Во время начальной настройки нужно выбрать хотя бы один язык: R, Python или оба. 

## <a name="additional-requirements"></a>Дополнительные требования

Чтобы после установки включить внешние скрипты, выполните следующий скрипт:

```sql
sp_configure 'external scripts enabled', 1;
RECONFIGURE WITH OVERRIDE;  
```

Чтобы это изменение вступило в силу, необходимо перезапустить [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Дополнительные сведения см. в разделе [Установка машинного обучения SQL Server](../../advanced-analytics/r/set-up-sql-server-r-services-in-database.md).

## <a name="see-also"></a>См. также раздел

[sp_configure (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)

[RECONFIGURE (Transact-SQL)](../../t-sql/language-elements/reconfigure-transact-sql.md)

[sp_execute_external_script (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)

[Изучение служб машины SQL Server](../../advanced-analytics/r/sql-server-r-services.md)

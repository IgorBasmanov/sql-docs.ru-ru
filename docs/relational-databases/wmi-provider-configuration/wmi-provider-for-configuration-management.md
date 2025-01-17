---
title: Поставщик WMI для Configuration Management Concepts | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: wmi
ms.topic: reference
helpviewer_keywords:
- WMI Provider for Configuration Management
- SQL Server WMI Provider
- configuration management [WMI]
- WMI Provider for Configuration Management, about WMI Provider for Configuration Management
ms.assetid: 7e41db24-b915-4eb8-a1d6-e6948ee915b7
author: CarlRabeler
ms.author: carlrab
ms.openlocfilehash: 2a1af3bff481c2c1c6b9a4d5fc128f324966a3c7
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68139283"
---
# <a name="wmi-provider-for-configuration-management"></a>Поставщик инструментария WMI для управления конфигурации
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Поставщик WMI представляет собой опубликованный слой, который используется с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager – – оснастка для [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console (MMC) и [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager. Предоставляет единообразный метод взаимодействия с API-интерфейсом, позволяющий управлять операциями с реестром, запрошенными диспетчером конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], и обеспечивает улучшенное управление выбранным экземпляром [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Поставщик WMI [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] представляет собой динамическую библиотеку (DLL) и файл MOF, которые компилируются автоматически программой установки [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Поставщик WMI [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] содержит набор классов объектов, используемых для управления службами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] при помощи следующих методов.  
  
-   Язык скриптов (например, VBScript, [!INCLUDE[jsprjscript](../../includes/jsprjscript-md.md)] или Perl), в который можно внедрить язык Windows Query Language (WQL).  
  
-   Объект <xref:Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer> в программе на управляемом коде SMO.  
  
-   Диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] или MMC с оснасткой поставщика WMI [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="using-a-script-language"></a>Использование языка скриптов  
 Применение языка скриптов дает следующие преимущества.  
  
-   Не требуется среда разработки.  
  
-   Файлы, поддерживающие язык скриптов, широкодоступны.  
  
 Скрипт может работать не только с поставщиком WMI для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], но и с другими поставщиками WMI. Администратор домена может использовать скрипт для настройки служб, конфигурирования сети и настройки псевдонимов на нескольких компьютерах в сети.  
  
 В данном разделе подробно описывается доступ к поставщику WMI для управления конфигурацией с помощью скриптов.  
  
## <a name="using-the-smo-managedcomputer-object"></a>Использование объекта ManagedComputer SMO  
 Объект <xref:Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer> представляет собой управляемый объект SMO, предоставляющий доступ к поставщику WMI для управления конфигурацией. С помощью программы SMO можно использовать объект <xref:Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer> для просмотра и изменения служб, сетевых настроек и псевдонимов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения см. в разделе [управление службами и сетевыми настройками с помощью поставщика WMI](../../relational-databases/server-management-objects-smo/tasks/managing-services-and-network-settings-by-using-wmi-provider.md).  
  
## <a name="using-the-microsoft-management-console-or-sql-server-configuration-manager"></a>Использование консоли управления (MMC) и диспетчера конфигурации SQL Server  
 Консоль управления (MMC) предоставляет интерфейс для управления службами [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в противоположность управлению с помощью языка сценариев и программ управляемого кода. Консоль управления (MMC) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно использовать для запуска и остановки служб и для изменения учетных записей службы.  
  
 Диспетчер конфигурации [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] можно также использовать для управления службами, серверными и клиентскими протоколами и псевдонимами серверов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>См. также  
 [Основные сведения о поставщике WMI для управления конфигурацией](../../relational-databases/wmi-provider-configuration/understanding-the-wmi-provider-for-configuration-management.md)   
 [Работа с поставщиком WMI для управления конфигурацией](../../relational-databases/wmi-provider-configuration/working-with-the-wmi-provider-for-configuration-management.md)   
 [Использование WQL и языков сценариев с поставщиком WMI для управления конфигурациями](../../relational-databases/wmi-provider-configuration/using-wql-and-scripting-languages-with-the-wmi-provider.md)  
  
  

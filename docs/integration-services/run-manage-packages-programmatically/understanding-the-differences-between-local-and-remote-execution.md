---
title: Основные сведения об отличиях между локальным и удаленным выполнением | Документы Майкрософт
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Integration Services packages, running
- packages [Integration Services], running
- packages [Integration Services], troubleshooting
ms.assetid: 610ee7d9-4fea-4aba-9395-57add826923b
author: janinezhang
ms.author: janinez
ms.openlocfilehash: 61233900def55fc28627869a134bddaf786f5548
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68028439"
---
# <a name="understanding-the-differences-between-local-and-remote-execution"></a>Основные сведения об отличиях между локальным и удаленным выполнением

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


  Разработчики пакетов и администраторы должны знать, в чем состоят ограничения, связанные с местом выполнения пакета служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
-   **Пакет выполняется на том же компьютере, что и запустившая его программа**. Даже если программа загружает пакет, который хранится удаленно на другом сервере, пакет выполняется на локальном компьютере.  
  
-   **Вне среды разработки пакет может выполняться только на том компьютере, на котором установлены службы Integration Services**. Невозможно выполнять пакеты вне среды [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] на клиентском компьютере, если не установлены службы [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Условия вашей лицензии на [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] могут не допускать установку служб [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] на дополнительных компьютерах. Службы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] являются серверным компонентом и не могут устанавливаться на клиентских компьютерах. Чтобы выполнить пакеты с клиентского компьютера, необходимо запускать их таким способом, который позволяет гарантировать выполнение пакетов на сервере.  
  
 Дополнительные сведения о загрузке и выполнении сохраненного пакета см. в разделах:  
  
-   [Программная загрузка и запуск локального пакета](../../integration-services/run-manage-packages-programmatically/loading-and-running-a-local-package-programmatically.md)  
  
-   [Программная загрузка и запуск удаленного пакета](../../integration-services/run-manage-packages-programmatically/loading-and-running-a-remote-package-programmatically.md)  
  
 Дополнительные сведения о выполнении пакета и загрузке его выхода в пользовательскую программу см. в разделах:  
  
-   [Загрузка выхода локального пакета](../../integration-services/run-manage-packages-programmatically/loading-the-output-of-a-local-package.md)  
  
## <a name="see-also"></a>См. также:  
 [Программная загрузка и запуск локального пакета](../../integration-services/run-manage-packages-programmatically/loading-and-running-a-local-package-programmatically.md)   
 [Программная загрузка и запуск удаленного пакета](../../integration-services/run-manage-packages-programmatically/loading-and-running-a-remote-package-programmatically.md)   
 [Загрузка выхода локального пакета](../../integration-services/run-manage-packages-programmatically/loading-the-output-of-a-local-package.md)  
  
  

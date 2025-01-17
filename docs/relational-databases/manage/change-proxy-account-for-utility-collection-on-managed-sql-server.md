---
title: Изменение учетной записи-посредника для элементов сбора служебной программы на управляемом SQL Server | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
ms.assetid: ff37ba8b-a08c-4109-b6e2-5748c995a52c
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 401ae65cade806a435bbab3dc346cf7ea2c41e83
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68115480"
---
# <a name="change-proxy-account-for-utility-collection-on--managed-sql-server"></a>Изменение учетной записи-посредника для элементов сбора служебной программы на управляемом SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  В этом разделе описано, как изменить учетную запись-посредник для набора элементов сбора служебной программы на управляемом экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
##  <a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-change-the-proxy-account-for-the-utility-collection-set-on-a-managed-instance-of-sql-server"></a>Изменение учетной записи-посредника для набора элементов сбора служебной программы на управляемом экземпляре SQL Server  
  
1.  Удалите управляемый экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из программы [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   В **Обозревателе программ** среды SSMS щелкните узел **Управляемые экземпляры** .  
  
    -   В списке **обозревателя программ** щелкните правой кнопкой мыши имя экземпляра [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и выберите команду **Удалить управляемый экземпляр…** . Дополнительные сведения см. в разделе [Удаление экземпляра SQL Server из служебной программы SQL Server](../../relational-databases/manage/remove-an-instance-of-sql-server-from-the-sql-server-utility.md).  
  
2.  Снова зарегистрируйте экземпляр [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в программе [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

    -   В **обозревателе программ** среды SSMS щелкните правой кнопкой мыши узел **Управляемые экземпляры** и выберите команду **Добавить управляемый экземпляр…** .  
  
    -   Выполните предлагаемые мастером действия, указав новую учетную запись-посредник.  
  
## <a name="see-also"></a>См. также:  
 [Функции и задачи служебной программы SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md)   
 [Устранение неполадок служебной программы SQL Server](https://msdn.microsoft.com/library/f5f47c2a-38ea-40f8-9767-9bc138d14453)  
  
  

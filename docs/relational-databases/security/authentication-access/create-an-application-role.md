---
title: Создание роли приложения | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.approle.general.f1
helpviewer_keywords:
- application roles [SQL Server], creating
ms.assetid: 6b8da1f5-3d8e-4f88-b111-b915788b06f1
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4ca72f042d8a41501cef58e321654fa8c8374a88
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68094932"
---
# <a name="create-an-application-role"></a>Создание роли приложения
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  В этом разделе описывается создание роли приложения в [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] с помощью среды [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] или [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Роли приложения ограничивают доступ пользователя к базе данных за исключением указанных приложений. Роли приложения не имеют пользователей: список **Члены роли** не отображается, когда выбран пункт **Роль приложения** .  
  
> [!IMPORTANT]  
>  Сложность пароля проверяется при установке паролей для роли приложения. Приложения, которые используют роли приложения, должны хранить свои пароли. Пароли ролей приложения всегда должны храниться в зашифрованном виде.  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [безопасность](#Security)  
  
-   **Создание роли приложения с помощью:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="Security"></a> безопасность  
  
####  <a name="Permissions"></a> Permissions  
 Необходимо наличие разрешения ALTER ANY APPLICATION ROLE для этой базы данных.  
  
##  <a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
##### <a name="to-create-an-application-role"></a>Создание роли приложения  
  
1.  В обозревателе объектов разверните базу данных, в которой необходимо создать роль приложения.  
  
2.  Разверните папку **Безопасность** .  
  
3.  Разверните папку **Роли** .  
  
4.  Щелкните правой кнопкой мыши папку **Роли приложения** и выберите пункт **Создать роль приложения...** .  
  
5.  В диалоговом окне **Application Role - New** (Роль приложения — создание) на странице **Общие** введите новое имя для новой роли приложения в поле **Имя роли**.  
  
6.  В поле **Схема по умолчанию** укажите схему, к которой будут принадлежать объекты, создаваемые этой ролью, введя имена объектов. Или щелкните кнопку с многоточием **(…)** , чтобы открыть диалоговое окно **Поиск схемы**.  
  
7.  В поле **Пароль** введите пароль для новой роли. Повторно введите пароль в поле **Подтверждение пароля** .  
  
8.  В поле **Схемы, принадлежащие данной роли**выберите или просмотрите схемы, которые будут принадлежать этой роли. Схема может принадлежать только одной схеме или роли.  
  
9. [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

[!INCLUDE[freshInclude](../../../includes/paragraph-content/fresh-note-steps-feedback.md)]

### <a name="additional-options"></a>Дополнительные параметры  
 Кроме того, в диалоговом окне **Роль приложения — создать** имеются параметры на двух дополнительных страницах: **Защищаемые объекты** и **Расширенные свойства**.  
  
-   На странице **Защищаемые объекты** перечислены все возможные защищаемые объекты и разрешения на эти объекты, которые могут быть предоставлены для имени входа.  
  
-   Страница **Расширенные свойства** позволяет добавлять пользовательские свойства пользователям базы данных.  
  
##  <a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-create-an-application-role"></a>Создание роли приложения  
  
1.  В **обозревателе объектов**подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```  
    -- Creates an application role called "weekly_receipts" that has the password "987Gbv876sPYY5m23" and "Sales" as its default schema.  
  
    CREATE APPLICATION ROLE weekly_receipts   
        WITH PASSWORD = '987G^bv876sPY)Y5m23'   
        , DEFAULT_SCHEMA = Sales;  
    GO  
    ```  
  
 Дополнительные сведения см. в разделе [CREATE APPLICATION ROLE (Transact-SQL)](../../../t-sql/statements/create-application-role-transact-sql.md).  
  
  

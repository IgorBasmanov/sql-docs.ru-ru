---
title: Роли приложений | Документация Майкрософт
ms.custom: ''
ms.date: 08/06/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- application roles [SQL Server], about application roles
- principals [SQL Server], application roles
- credentials [SQL Server], roles
- application roles [SQL Server]
- roles [SQL Server], application
- permissions [SQL Server], roles
- users [SQL Server], application roles
- authentication [SQL Server], roles
- groups [SQL Server], roles
ms.assetid: dca18b8a-ca03-4b7f-9a46-8474d5b66f76
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b26765281f650a73e7c21e40ce63916eeb9fa21f
ms.sourcegitcommit: 9702dd51410dd610842d3576b24c0ff78cdf65dc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2019
ms.locfileid: "68841591"
---
# <a name="application-roles"></a>Роли приложений
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Роль приложения — это участник базы данных, позволяющий приложению выполняться со своими, подобными пользовательским, правами доступа. Роли приложений можно использовать для разрешения доступа к определенным данным только тем пользователям, которые подключены посредством конкретного приложения. В отличие от ролей баз данных, роли приложений не содержат элементов и по умолчанию находятся в неактивном состоянии. Роли приложений активируются с помощью процедуры **sp_setapprole**, которая требует указания пароля. Так как роли приложений являются участниками на уровне базы данных, они имеют доступ к другим базам данных только с разрешениями, предоставленными учетной записи пользователя **guest**в этих базах данных. Таким образом, любая база данных, в которой была отключена учетная запись пользователя **guest** , не будет доступна для ролей приложений в других базах данных.  
  
 В [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]роли приложений не имеют доступ к метаданным уровня сервера, так как они не связаны с участником на уровне сервера. Чтобы отключить это ограничение, позволив тем самым ролям приложений получать доступ к метаданным уровня сервера, установите глобальный флаг 4616. Дополнительные сведения см. в разделах [Флаги трассировки (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) и [DBCC TRACEON (Transact-SQL)](../../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md).  
  
## <a name="connecting-with-an-application-role"></a>Соединение с ролью приложения  
 Ниже представлены этапы процесса, при помощи которого роль приложения переключает контексты безопасности.  
  
1.  Пользователь выполняет клиентское приложение.  
  
2.  Клиентское приложение соединяется с экземпляром [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] в качестве пользователя.  
  
3.  Приложение выполняет хранимую процедуру **sp_setapprole** с использованием пароля, известного только этому приложению.  
  
4.  Если имя и пароль роли приложения достоверны, она становится активной.  
  
5.  На этом этапе соединение утрачивает разрешения пользователя и приобретает разрешения роли приложения.  

[!INCLUDE[freshInclude](../../../includes/paragraph-content/fresh-note-steps-feedback.md)]

 Разрешения, полученные через роль приложения, действуют в течение всего соединения.  
  
 В более ранних версиях [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]единственным способом для пользователя вновь приобрести свой исходный контекст безопасности после активации роли приложения было отсоединение и повторное соединение с [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Начиная с версии [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]процедура **sp_setapprole** имеет новый параметр, который создает файл cookie. Этот файл содержит сведения о контексте до того, как роль приложения переходит в активное состояние. Файл cookie может использоваться процедурой **sp_unsetapprole** для возврата сеанса в исходный контекст. Сведения об этом новом параметре и пример см. в разделе [sp_setapprole (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)в этих базах данных.  
  
> [!IMPORTANT]  
>  Параметр **encrypt** ODBC не поддерживается в **SqlClient**. При передаче конфиденциальных сведений по сети следует пользоваться протоколом SSL или IPSec для шифрования канала. Если необходимо сохранить учетные данные в клиентском приложении, следует зашифровать их при помощи функций API шифрования. В [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] и более поздних версиях параметр *password* хранится в виде одностороннего хэша.  
  
## <a name="related-tasks"></a>Связанные задачи  
  
|||  
|-|-|  
|Создание роли приложения.|[Создание роли приложения](../../../relational-databases/security/authentication-access/create-an-application-role.md) и [CREATE APPLICATION ROLE (Transact-SQL)](../../../t-sql/statements/create-application-role-transact-sql.md)|  
|Изменение роли приложения.|[ALTER APPLICATION ROLE (Transact-SQL)](../../../t-sql/statements/alter-application-role-transact-sql.md)|  
|Удаление роли приложения.|[DROP APPLICATION ROLE (Transact-SQL)](../../../t-sql/statements/drop-application-role-transact-sql.md)|  
|Использование роли приложения.|[sp_setapprole (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)|  
  
## <a name="see-also"></a>См. также:  
 [Обеспечение безопасности SQL Server](../../../relational-databases/security/securing-sql-server.md)  
  
  

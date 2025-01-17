---
title: Синхронизация пакетов R из файловой системы
description: Обновление библиотек R на SQL Server с более новыми версиями, установленными в файловой системе.
ms.prod: sql
ms.technology: machine-learning
ms.date: 06/13/2019
ms.topic: conceptual
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 4e0b6133bcecc553934cd657785ea9ee765c6fd9
ms.sourcegitcommit: 321497065ecd7ecde9bff378464db8da426e9e14
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68715075"
---
# <a name="r-package-synchronization-for-sql-server"></a>Синхронизация пакетов R для SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

Версия RevoScaleR, включенная в SQL Server 2017, включает возможность синхронизации коллекций пакетов R между файловой системой и экземпляром и базой данных, где используются пакеты.

Эта функция предназначена для упрощения резервного копирования коллекций пакетов R, связанных с SQL Serverными базами данных. С помощью этой функции администратор может восстановить не только базу данных, но и все пакеты R, которые использовались специалистами по обработке и анализу данных, работающим в этой базе данных.

В этой статье описывается функция синхронизации пакетов и использование функции [ркссинкпаккажес](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxsyncpackages) для выполнения следующих задач.

+ Синхронизация списка пакетов для всей базы данных SQL Server

+ Синхронизация пакетов, используемых отдельным пользователем или группой пользователей

+ Если пользователь переходит на другой SQL Server, можно создать резервную копию рабочей базы данных пользователя и восстановить ее на новом сервере, а пакеты для пользователя будут установлены в файловую систему на новом сервере в соответствии с требованиями R.

Например, синхронизацию пакетов можно использовать в следующих сценариях:

+ Администратор базы данных восстановил экземпляр SQL Server на новом компьютере и запрашивает у пользователей подключение из своих клиентов R и выполняет `rxSyncPackages` для обновления и восстановления своих пакетов.

+ Вы считаете, что пакет R в файловой системе поврежден, поэтому вы выполняете `rxSyncPackages` SQL Server.

## <a name="requirements"></a>Требования

Прежде чем можно будет использовать синхронизацию пакетов, необходимо иметь соответствующую версию Microsoft R или Machine Learning Server. Эта функция предоставляется в Microsoft R версии 9.1.0 или более поздней. 

Также необходимо включить [функцию управления пакетами](r-package-how-to-enable-or-disable.md) на сервере.

### <a name="determine-whether-your-server-supports-package-management"></a>Определение того, поддерживает ли сервер Управление пакетами

Эта функция доступна в SQL Server 2017 CTP 2 или более поздней версии.

Эту функцию можно добавить в экземпляр SQL Server 2016, обновив экземпляр для использования последней версии Microsoft R. Дополнительные сведения см. [в разделе Использование SqlBindR. exe для обновления SQL Server R Services](../install/upgrade-r-and-python.md).

### <a name="enable-the-package-management-feature"></a>Включение функции управления пакетами

Чтобы использовать синхронизацию пакетов, необходимо включить новую функцию управления пакетами на экземпляре SQL Server и в отдельных базах данных. Дополнительные сведения см. в разделе [Включение или отключение управления пакетами для SQL Server](r-package-how-to-enable-or-disable.md).

1. Администратор сервера включает функцию для экземпляра SQL Server.
2. Для каждой базы данных администратор предоставляет отдельным пользователям возможность установки или совместного использования пакетов R с помощью ролей базы данных.

После этого можно использовать функции RevoScaleR, такие как [рксинсталлпаккажес](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxinstallpackages) , для установки пакетов в базу данных.  Сведения о пользователях и пакетах, которые они могут использовать, хранятся в экземпляре SQL Server. 

При добавлении нового пакета с помощью функций управления пакетами обновляются как записи в SQL Server, так и файловая система. Эти сведения можно использовать для восстановления сведений о пакете для всей базы данных.

### <a name="permissions"></a>Разрешения

+ Пользователь, выполняющий функцию синхронизации пакетов, должен быть субъектом безопасности на экземпляре SQL Server и базе данных, в которой находятся пакеты.

+ Вызывающая функция должна быть членом одной из следующих ролей управления пакетами: **rpkgs-Shared** или **rpkgs-Private**.

+ Для синхронизации пакетов, помеченных как **Общие**, пользователь, выполняющий функцию, должен иметь членство в роли **rpkgs-Shared** , а перемещаемые пакеты должны быть установлены в общую библиотеку областей.

+ Чтобы синхронизировать пакеты, помеченные как **частные**, либо владелец пакета, либо администратор должен запустить функцию, а пакеты должны быть частными.

+ Чтобы синхронизировать пакеты от имени других пользователей, владелец должен быть членом роли **db_owner** базы данных.

## <a name="how-package-synchronization-works"></a>Как работает синхронизация пакетов

Чтобы использовать синхронизацию пакетов, вызовите [ркссинкпаккажес](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxsyncpackages), которая является новой функцией в [RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler). 

Для каждого вызова `rxSyncPackages`необходимо указать SQL Server экземпляр и базу данных. Затем следует либо составить список пакетов для синхронизации, либо указать область пакета.

1. Создайте SQL Server контекст вычислений с помощью `RxInSqlServer` функции. Если контекст вычислений не указан, используется текущий контекст вычислений.

2. Укажите имя базы данных в экземпляре в указанном контексте вычислений. Пакеты синхронизируются для каждой базы данных.

3. Укажите пакеты для синхронизации с помощью аргумента Scope.

    При использовании **закрытой** области синхронизируются только пакеты, принадлежащие указанному владельцу. Если указать **общую** область, все нечастные пакеты в базе данных синхронизируются. 
    
    При запуске функции без указания **частной** или **общей** области синхронизируются все пакеты.

4. Если команда выполнена успешно, существующие пакеты в файловой системе добавляются в базу данных с указанной областью и владельцем.

    Если файловая система повреждена, пакеты восстанавливаются на основе списка, сохраненного в базе данных.

    Если функция управления пакетами недоступна в целевой базе данных, возникает ошибка: "Функция управления пакетами не включена на SQL Server или слишком старая версия"

### <a name="example-1-synchronize-all-package-by-database"></a>Пример 1. Синхронизировать все пакеты по базе данных

Этот пример получает все новые пакеты из локальной файловой системы и устанавливает пакеты в базу данных [TestDB]. Поскольку владелец не определен, список включает все пакеты, которые были установлены для частных и общих областей.

```R
connectionString <- "Driver=SQL Server;Server=myServer;Database=TestDB;Trusted_Connection=True;"
computeContext <- RxInSqlServer(connectionString = connectionString )
rxSyncPackages(computeContext=computeContext, verbose=TRUE)
```

### <a name="example-2-restrict-synchronized-packages-by-scope"></a>Пример 2. Ограничить синхронизированные пакеты по области

В следующих примерах синхронизируются только пакеты в указанной области.

```R
#Shared scope
rxSyncPackages(computeContext=computeContext, scope="shared", verbose=TRUE)

#Private scope
rxSyncPackages(computeContext=computeContext, scope="private", verbose=TRUE)
```

### <a name="example-3-restrict-synchronized-packages-by-owner"></a>Пример 3. Ограничение синхронизированных пакетов по владельцам

В следующем примере показано, как синхронизировать только те пакеты, которые были установлены для конкретного пользователя. В этом примере пользователь определяется именем входа SQL, *User1*.

```R
rxSyncPackages(computeContext=computeContext, scope="private", owner = "user1", verbose=TRUE))
```

## <a name="related-resources"></a>Связанные ресурсы

[Управление пакетами R для SQL Server](install-additional-r-packages-on-sql-server.md)

---
title: Обзор SQL Server на Linux
description: В этой статье описывается работа SQL Server на Linux и приводятся ссылки для получения дополнительной информации.
author: VanMSFT
ms.author: vanto
ms.date: 04/23/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 9dcc6a90-0add-42c2-815b-862e4e2a21ac
ms.openlocfilehash: e3bd50cba4bcab81e7dcf00db9394704c5486160
ms.sourcegitcommit: db9bed6214f9dca82dccb4ccd4a2417c62e4f1bd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68105460"
---
# <a name="sql-server-on-linux"></a>SQL Server в Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

::: moniker range="= sql-server-2017 || = sqlallproducts-allversions"
Начиная с версии SQL Server 2017 возможна работа SQL Server на Linux. Несмотря на другую операционную систему, это то же ядро СУБД SQL Server с множеством схожих функций и служб.
::: moniker-end

::: moniker range=">= sql-server-ver15 || >= sql-server-linux-ver15"
Предварительная версия SQL Server 2019 работает на Linux. Несмотря на другую операционную систему, это то же ядро СУБД SQL Server с множеством схожих функций и служб. Дополнительные сведения об этом выпуске см. в статье [Новые возможности в предварительной версии SQL Server 2019 для Linux](../sql-server/what-s-new-in-sql-server-ver15.md#sql-server-on-linux).
::: moniker-end

::: moniker range="= sql-server-2017"
> [!TIP]
> [Предварительная версия SQL Server 2019](sql-server-linux-overview.md?view=sql-server-ver15) уже выпущена! Сведения о новых возможностях см. в статье [Новые возможности в предварительной версии SQL Server 2019 для Linux](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15#sql-server-on-linux).
::: moniker-end

::: moniker range="= sql-server-linux-2017"
> [!TIP]
> [Предварительная версия SQL Server 2019](sql-server-linux-overview.md?view=sql-server-linux-ver15) уже выпущена! Сведения о новых возможностях см. в статье [Новые возможности в предварительной версии SQL Server 2019 для Linux](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-linux-ver15#sql-server-on-linux).
::: moniker-end

::: moniker range="= sqlallproducts-allversions"
> [!TIP]
> Предварительная версия SQL Server 2019 уже выпущена! Сведения о новых возможностях см. в статье [Новые возможности в предварительной версии SQL Server 2019 для Linux](../sql-server/what-s-new-in-sql-server-ver15.md#sql-server-on-linux).
::: moniker-end

## <a name="install"></a>Установка

Чтобы начать работу, установите SQL Server на Linux, используя любое из следующих кратких руководств:

- [Установка в Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Установка в SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Установка в Ubuntu](quickstart-install-connect-ubuntu.md)
- [Запуск в Docker](quickstart-install-connect-docker.md)
- [Подготовка виртуальной машины SQL в Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json)

> [!NOTE]
> Docker поддерживает множество платформ, то есть вы можете запускать образ Docker в Linux, Mac и Windows.

## <a name="connect"></a>Подключение

После установки подключитесь к экземпляру SQL Server на компьютере с Linux. Вы можете устанавливать подключение как локально, так и удаленно, используя самые разные средства и драйверы. В кратком руководстве показано, как использовать программу командной строки [sqlcmd](sql-server-linux-setup-tools.md). Также можно использовать следующие средства:

| Инструмент | Учебник |
|-----|-----|
| Visual Studio Code (VS Code) | [Использование VS Code с SQL Server на Linux](sql-server-linux-develop-use-vscode.md) |
| SQL Server Management Studio (SSMS) | [Использование SSMS в Windows для подключения к SQL Server на Linux](sql-server-linux-manage-ssms.md) |
| SQL Server Data Tools (SSDT) | [Использование SSDT с SQL Server на Linux](sql-server-linux-develop-use-ssdt.md) |

## <a name="explore"></a>Просмотр

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

SQL Server 2017 использует одинаковое базовое ядро СУБД на всех поддерживаемых платформах, включая Linux. Благодаря этому многие существующие функции и возможности работают в Linux так же, как и на других платформах. В этой части документации некоторые из этих функций рассматриваются с точки зрения платформы Linux. Кроме того, отмечаются области, в которых платформа Linux предъявляет уникальные требования.

Если вы уже знакомы с SQL Server, изучите общие рекомендации и известные проблемы в [заметках об этом выпуске](sql-server-linux-release-notes.md). Также ознакомьтесь с [новыми возможностями в SQL Server на Linux](sql-server-linux-whats-new.md) и [SQL Server 2017 в целом](../sql-server/what-s-new-in-sql-server-2017.md).

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15"

[!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)] использует одинаковое базовое ядро СУБД на всех поддерживаемых платформах, включая Linux. Благодаря этому многие существующие функции и возможности работают в Linux так же, как и на других платформах. В этой части документации некоторые из этих функций рассматриваются с точки зрения платформы Linux. Кроме того, отмечаются области, в которых платформа Linux предъявляет уникальные требования.

Если вы уже знакомы с SQL Server на Linux, изучите общие рекомендации и известные проблемы в [заметках об этом выпуске](sql-server-linux-release-notes-2019.md). Также ознакомьтесь с [новыми возможностями в предварительной версии SQL Server 2019 на Linux](../sql-server/what-s-new-in-sql-server-ver15.md?view=sql-server-ver15).

::: moniker-end

<!--SQL Server All Versions-->
::: moniker range="=sqlallproducts-allversions"

SQL Server 2017 и [!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)] используют одинаковое базовое ядро СУБД на всех поддерживаемых платформах, включая Linux. Благодаря этому многие существующие функции и возможности работают в Linux так же, как и на других платформах. В этой части документации некоторые из этих функций рассматриваются с точки зрения платформы Linux. Кроме того, отмечаются области, в которых платформа Linux предъявляет уникальные требования.

Если вы уже знакомы с SQL Server на Linux, ознакомьтесь с заметками о выпуске:

- [Заметки о выпуске для SQL Server 2017](sql-server-linux-release-notes.md)
- [Заметки о выпуске предварительной версии SQL Server 2019](sql-server-linux-release-notes-2019.md)

Также ознакомьтесь с новыми возможностями:

- [Новые возможности в SQL Server 2017](sql-server-linux-whats-new.md)
- [Новые возможности в предварительной версии SQL Server 2019 на Linux](../sql-server/what-s-new-in-sql-server-ver15.md#sql-server-on-linux)

::: moniker-end

> [!TIP]
> Ответы на часто задаваемые вопросы об SQL Server на Linux см. в [этой статье](sql-server-linux-faq.md).

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

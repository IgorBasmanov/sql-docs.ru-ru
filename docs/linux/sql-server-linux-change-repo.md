---
title: Настройка репозиториев Linux для SQL Server 2017 и 2019
description: Проверьте и настройте репозитории исходного кода для SQL Server 2019 и SQL Server 2017 в Linux. Репозиторий исходного кода влияет на версию SQL Server, используемую во время установки и обновления.
author: VanMSFT
ms.author: vanto
ms.date: 02/11/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
zone_pivot_groups: ld2-linux-distribution
ms.openlocfilehash: 33616b9a7767156e4cfd69d233f7dcfe5fc080f6
ms.sourcegitcommit: db9bed6214f9dca82dccb4ccd4a2417c62e4f1bd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "67967522"
---
# <a name="configure-repositories-for-installing-and-upgrading-sql-server-on-linux"></a>Настройка репозиториев для установки и обновления SQL Server на Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

::: zone pivot="ld2-rhel"
В это статье описывается, как правильно настроить репозиторий для установки и обновления SQL Server 2017 и SQL Server 2019 в Linux. Вверху страницы в настоящее время выбрана ОС **Red Hat (RHEL)** .
::: zone-end

::: zone pivot="ld2-sles"
В это статье описывается, как правильно настроить репозиторий для установки и обновления SQL Server 2017 и SQL Server 2019 в Linux. Вверху страницы в настоящее время выбрана ОС **SUSE (SLES)** .
::: zone-end

::: zone pivot="ld2-ubuntu"
В это статье описывается, как правильно настроить репозиторий для установки и обновления SQL Server 2017 и SQL Server 2019 в Linux. Вверху страницы в настоящее время выбрана ОС **Ubuntu**.
::: zone-end

> [!TIP]
> Предварительная версия SQL Server 2019 теперь доступна. Чтобы опробовать ее, настройте новый репозиторий **mssql-server-preview**, как описано в этой статье. После этого выполните инструкции в [руководстве по установке](sql-server-linux-setup.md).

## <a id="repositories"></a> Репозитории

При установке SQL Server на Linux необходимо настроить репозиторий Майкрософт. Он используется для получения пакета ядра СУБД (**mssql-server**) и связанных с ним пакетов SQL Server. В настоящее время существует три основных репозитория:

| Хранилище | Имя | Описание |
|---|---|---|
| **Предварительная версия (2017)** | **mssql-server** | Репозиторий для SQL Server 2017 CTP и RC (больше не поддерживается). |
| **Предварительная версия (2019)** | **mssql-server-preview** | Репозиторий для предварительной версии и версии RC SQL Server 2019. |
| **Накопительный пакет обновления** | **mssql-server-2017** | Репозиторий для SQL Server 2017 с накопительным пакетом обновления. |
| **Выпуск для общего распространения (GDR)** | **mssql-server-2017-gdr** | Репозиторий выпуска SQL Server 2017 для общего распространения, предназначенный только для критических обновлений. |

## <a id="cuversusgdr"></a> Накопительный пакет обновления и GDR

Важно отметить, что для каждого дистрибутива имеются два основных типа репозиториев.

- **Накопительный пакет обновления**. Репозиторий накопительного пакета обновления содержит пакеты для основного выпуска SQL Server, а также все исправления ошибок и улучшения, добавленные с момента этого выпуска. Накопительные пакеты обновления связаны с определенной версий, например SQL Server 2017. Они выпускаются регулярно.

- **Выпуск для общего распространения (GDR)** . Репозиторий GDR содержит пакеты для основного выпуска SQL Server и только критические исправления и обновления для системы безопасности, добавленные с момента этого выпуска. Эти обновления также добавляются в следующий накопительный пакет обновления.

Каждый выпуск накопительного пакета обновления и GDR содержит полный пакет SQL Server и все предыдущие обновления для этого репозитория. Выполнить обновление с выпуска GDR на выпуск накопительного пакета обновления можно путем изменения настроенного репозитория для SQL Server. Можно также [перейти на использование любого более раннего выпуска](sql-server-linux-setup.md#rollback) в рамках основной версии (например, 2017).

> [!NOTE]
> Выполнить обновление с выпуска GDR на выпуск накопительного пакета обновления можно в любой момент путем изменения репозитория. Обновление с выпуска накопительного пакета обновления на выпуск GDR не поддерживается.

## <a name="configure-repositories"></a>Настройка репозиториев

::: zone pivot="ld2-rhel"
Чтобы настроить репозитории в Red Hat Enterprise Server (RHEL), выполните инструкции, приведенные в следующих разделах.
::: zone-end

::: zone pivot="ld2-sles"
Чтобы настроить репозитории в SUSE Linux Enterprise Server (SLES), выполните инструкции, приведенные в следующих разделах.
::: zone-end

::: zone pivot="ld2-ubuntu"
Чтобы настроить репозитории в Ubuntu, выполните инструкции, приведенные в следующих разделах.
::: zone-end

## <a name="check-for-previously-configured-repositories"></a>Проверка ранее настроенных репозиториев

<!--RHEL-->
::: zone pivot="ld2-rhel"
Сначала проверьте, есть ли уже зарегистрированный репозиторий SQL Server.

1. Просмотрите файлы в каталоге **/etc/yum.repos.d** с помощью следующей команды:

   ```bash
   sudo ls /etc/yum.repos.d
   ```

2. Найдите файл, который служит для настройки каталога SQL Server, например **mssql-server.repo**.

3. Выведите содержимое файла.

   ```bash
   sudo cat /etc/yum.repos.d/mssql-server.repo
   ```

4. Настроенный репозиторий указан в свойстве **name**. Его можно определить по таблице в разделе [Репозитории](#repositories) этой статьи.

::: zone-end

<!--SLES-->
::: zone pivot="ld2-sles"
Сначала проверьте, есть ли уже зарегистрированный репозиторий SQL Server.

1. Используйте команду **zypper info**, чтобы получить сведения о ранее настроенном репозитории.

   ```bash
   sudo zypper info mssql-server
   ```

2. Настроенный репозиторий указан в свойстве **Repository**. Его можно определить по таблице в разделе [Репозитории](#repositories) этой статьи.

::: zone-end

<!--Ubuntu-->
::: zone pivot="ld2-ubuntu"
Сначала проверьте, есть ли уже зарегистрированный репозиторий SQL Server.

1. Просмотрите содержимое файла **/etc/apt/sources.list**.

   ```bash
   sudo cat /etc/apt/sources.list
   ```

2. Найдите URL-адрес пакета mssql-server. Его можно определить по таблице в разделе [Репозитории](#repositories) этой статьи.

::: zone-end

## <a name="remove-old-repository"></a>Удаление старого репозитория

<!--RHEL-->
::: zone pivot="ld2-rhel"
При необходимости удалите старый репозиторий с помощью приведенной ниже команды.

```bash
sudo rm -rf /etc/yum.repos.d/mssql-server.repo
```

Предполагается, что в предыдущем разделе был определен файл **mssql-server.repo**.

::: zone-end

<!--SLES-->
::: zone pivot="ld2-sles"
При необходимости удалите старый репозиторий. В зависимости от типа ранее настроенного репозитория выполните одну из приведенных ниже команд.

| Хранилище | Команда для удаления |
|---|---|
| **Предварительная версия (2017)** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server'` |
| **Предварительная версия (2019)** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-preview'` |
| **Накопительный пакет обновления** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2017'` |
| **Выпуск для общего распространения (GDR)** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2017-gdr'`|

::: zone-end

<!--Ubuntu-->
::: zone pivot="ld2-ubuntu"
При необходимости удалите старый репозиторий. В зависимости от типа ранее настроенного репозитория выполните одну из приведенных ниже команд.

| Хранилище | Команда для удаления |
|---|---|
| **Предварительная версия (2017)** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server xenial main'` |
| **Предварительная версия (2019)** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview xenial main'` |
| **Накопительный пакет обновления** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017 xenial main'` | 
| **Выпуск для общего распространения (GDR)** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr xenial main'` |

::: zone-end

## <a name="configure-new-repository"></a>Настройка нового репозитория

<!--RHEL-->
::: zone pivot="ld2-rhel"
Настройте новый репозиторий, который будет использоваться для установки и обновления SQL Server. Чтобы настроить выбранный репозиторий, используйте одну из приведенных ниже команд.

| Хранилище | Версия | Command |
|---|---|---|
| **Предварительная версия (2019)** | 2019 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-preview.repo` |
| **Накопительный пакет обновления** | 2017 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo` |
| **Выпуск для общего распространения (GDR)** | 2017 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017-gdr.repo` |

::: zone-end

<!--SLES-->
::: zone pivot="ld2-sles"
Настройте новый репозиторий, который будет использоваться для установки и обновления SQL Server. Чтобы настроить выбранный репозиторий, используйте одну из приведенных ниже команд.

| Хранилище | Версия | Command |
|---|---|---|
| **Предварительная версия (2019)** | 2019 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-preview.repo` |
| **Накопительный пакет обновления** | 2017 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo` |
| **Выпуск для общего распространения (GDR)** | 2017 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017-gdr.repo` |

::: zone-end

<!--Ubuntu-->
::: zone pivot="ld2-ubuntu"
Настройте новый репозиторий, который будет использоваться для установки и обновления SQL Server.

1. Импортируйте открытые ключи GPG из репозитория.

   ```bash
   sudo curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. Чтобы настроить выбранный репозиторий, используйте одну из приведенных ниже команд.

   | Хранилище | Версия | Command |
   |---|---|---|
   | **Предварительная версия (2019)** | 2019 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-preview.list)"` |
   | **Накопительный пакет обновления** | 2017 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017.list)"` |
   | **Выпуск для общего распространения (GDR)** | 2017 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/16.04/mssql-server-2017-gdr.list)"` |

3. Выполните команду **apt-get update**.

   ```bash
   sudo apt-get update
   ```

::: zone-end

## <a name="next-steps"></a>Следующие шаги

Настроив нужный репозиторий, можно перейти к [установке](sql-server-linux-setup.md#platforms) или [обновлению](sql-server-linux-setup.md#upgrade) SQL Server и всех связанных пакетов из него.

::: zone pivot="ld2-rhel"
> [!IMPORTANT]
> Помните, что если вы решили использовать [краткое руководство для RHEL](quickstart-install-connect-red-hat.md), целевой репозиторий уже настроен. Не повторяйте этот шаг в руководстве. Это особенно важно в том случае, если вы настраиваете репозиторий GDR, так как в кратком руководстве используется репозиторий накопительного пакета обновления.
::: zone-end

::: zone pivot="ld2-sles"
> [!IMPORTANT]
> Помните, что если вы решили использовать [краткое руководство для SLES](quickstart-install-connect-suse.md), целевой репозиторий уже настроен. Не повторяйте этот шаг в руководстве. Это особенно важно в том случае, если вы настраиваете репозиторий GDR, так как в кратком руководстве используется репозиторий накопительного пакета обновления.
::: zone-end

::: zone pivot="ld2-ubuntu"
> [!IMPORTANT]
> Помните, что если вы решили использовать [краткое руководство для Ubuntu](quickstart-install-connect-ubuntu.md), целевой репозиторий уже настроен. Не повторяйте этот шаг в руководстве. Это особенно важно в том случае, если вы настраиваете репозиторий GDR, так как в кратком руководстве используется репозиторий накопительного пакета обновления.
::: zone-end

Дополнительные сведения об установке SQL Server 2017 на Linux см. в статье [Руководство по установке SQL Server на Linux](sql-server-linux-setup.md).

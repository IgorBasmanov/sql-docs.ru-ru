---
title: Использование распределенных транзакций с SQL Server в Docker
description: В этой статье описывается, как использовать координатор распределенных транзакций Майкрософт (MSDTC) для распределенных транзакций в контейнере SQL Server в Docker.
author: VanMSFT
ms.author: vanto
ms.date: 08/01/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: e4d9d52541b6f9c9ca87bcbe4dc1db3c4448725c
ms.sourcegitcommit: 728a4fa5a3022c237b68b31724fce441c4e4d0ab
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2019
ms.locfileid: "68770843"
---
# <a name="how-to-use-distributed-transactions-with-sql-server-on-docker"></a>Использование распределенных транзакций с SQL Server в Docker

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

В этой статье объясняется, как настроить контейнеры с SQL Server на Linux в Docker для распределенных транзакций.

Образы контейнеров SQL Server могут использовать координатор распределенных транзакций Майкрософт (MSDTC), необходимый для распределенных транзакций. Сведения о требованиях к связи для MSDTC см. в статье [Настройка координатора распределенных транзакций Майкрософт (MSDTC) в Linux](sql-server-linux-configure-msdtc.md). В этой статье описаны особые требования и сценарии, связанные с контейнерами SQL Server в Docker.

## <a name="configuration"></a>Конфигурация

Чтобы включить транзакцию MSDTC в контейнерах для Docker, необходимо задать две новые переменные среды:

- **MSSQL_RPC_PORT**: TCP-порт, к которому привязывается служба сопоставителя конечных точек RPC, ожидая передачи данных.  
- **MSSQL_DTC_TCP_PORT**: порт, который прослушивает служба MSDTC.

### <a name="pull-and-run"></a>Извлечение и запуск

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

В приведенном ниже примере показано, как использовать эти переменные среды для извлечения и запуска одиночного контейнера SQL Server 2017, настроенного для MSDTC. Это позволяет ему взаимодействовать с любым приложением на любых узлах.

```bash
docker run \
   -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
   -e 'MSSQL_RPC_PORT=135' -e 'MSSQL_DTC_TCP_PORT=51000' \
   -p 51433:1433 -p 135:135 -p 51000:51000  \
   -d mcr.microsoft.com/mssql/server:2017-latest
```

```PowerShell
docker run `
   -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
   -e "MSSQL_RPC_PORT=135" -e "MSSQL_DTC_TCP_PORT=51000" `
   -p 51433:1433 -p 135:135 -p 51000:51000  `
   -d mcr.microsoft.com/mssql/server:2017-latest
```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 || =sqlallproducts-allversions"

В приведенном ниже примере показано, как использовать эти переменные среды для извлечения и запуска контейнера предварительной версии SQL Server 2019, настроенного для MSDTC. Это позволяет ему взаимодействовать с любым приложением на любых узлах.

```bash
docker run \
   -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>' \
   -e 'MSSQL_RPC_PORT=135' -e 'MSSQL_DTC_TCP_PORT=51000' \
   -p 51433:1433 -p 135:135 -p 51000:51000  \
   -d mcr.microsoft.com/mssql/server:2019-CTP3.2-ubuntu
```

```PowerShell
docker run `
   -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=<YourStrong!Passw0rd>" `
   -e "MSSQL_RPC_PORT=135" -e "MSSQL_DTC_TCP_PORT=51000" `
   -p 51433:1433 -p 135:135 -p 51000:51000  `
   -d mcr.microsoft.com/mssql/server:2019-CTP3.2-ubuntu
```

::: moniker-end

> [!IMPORTANT]
> Предыдущая команда работает только для Docker в Linux. При использовании Docker в Windows узел Windows уже прослушивает порт 135. Вы можете удалить параметр `-p 135:135` для Docker в Windows, но тут есть несколько ограничений. Полученный контейнер невозможно будет использовать для распределенных транзакций, в которых участвует узел. Он может участвовать только в распределенных транзакциях между контейнерами Docker на узле.

В этой команде **служба сопоставителя конечных точек RPC** привязана к порту 135, а служба **MSDTC** привязана к порту 51000 в виртуальной сети Docker. Коммуникации TDS в SQL Server проходят через порт 1433 в виртуальной сети Docker. Эти порты были открыты извне для узла как порты TDS 51433, порт сопоставителя конечных точек RPC 135 и порт MSDTC 51000.

> [!NOTE]
> Сопоставитель конечных точек RPC и порт MSDTC необязательно должны совпадать на узле и в контейнере. Хотя порт сопоставителя конечных точек RPC был настроен на порте 135 в контейнере, он может быть сопоставлен с портом 13501 или с любым другим доступным портом на сервере узла.

## <a name="configure-the-firewall"></a>Настройка брандмауэра

Чтобы взаимодействовать с узлом и через него, необходимо также настроить брандмауэр на сервере узла для контейнеров. Откройте брандмауэр для всех портов, которые контейнер Docker открывает для внешнего взаимодействия. В предыдущем примере это порты 135, 51433 и 51000. Это порты на самом узле, а не порты, с которыми они сопоставляются в контейнере. Таким образом, если порт 51000 сопоставителя конечных точек RPC в контейнере был сопоставлен с портом 51001 узла, то для взаимодействия с узлом должен быть открыт порт 51001 (не 51000) в брандмауэре.  

В следующем примере показано, как создать эти правила в Ubuntu.

```bash
sudo ufw allow from any to any port 51433 proto tcp
sudo ufw allow from any to any port 51000 proto tcp
sudo ufw allow from any to any port 135 proto tcp
```

В следующем примере показано, как это можно сделать в Red Hat Enterprise Linux (RHEL).

```bash
sudo firewall-cmd --zone=public --add-port=51999/tcp --permanent
sudo firewall-cmd --zone=public --add-port=51433/tcp --permanent
sudo firewall-cmd --zone=public --add-port=135/tcp --permanent
sudo firewall-cmd --reload
```

## <a name="configure-port-routing-on-the-host"></a>Настройка маршрутизации портов на узле

В предыдущем примере, поскольку контейнер Docker сопоставляет RPC-порт 135 с портом 135 на узле, распределенные транзакции с этим узлом теперь должны работать без дальнейшей настройки. Обратите внимание, что в контейнере можно напрямую использовать порт 135, так как SQL Server выполняется в контейнере с повышенными привилегиями. Для SQL Server за пределами контейнера необходимо использовать другой эфемерный порт, а трафик к порту 135 должен перенаправляться на этот порт.

Однако если вы решили связать порт 135 контейнера с другим портом на узле, например 13500, необходимо настроить маршрутизацию портов на узле. Это позволяет контейнеру Docker участвовать в распределенных транзакциях с узлом и с другими внешними серверами. Дополнительные сведения см. в разделе [Настройка маршрутизации портов](sql-server-linux-configure-msdtc.md#configure-port-routing).

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о MSDTC на Linux см. в разделе [Сведения о настройке координатора распределенных транзакций (Майкрософт) (MSDTC) в Linux](sql-server-linux-configure-msdtc.md).

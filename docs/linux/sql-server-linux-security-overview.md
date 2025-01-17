---
title: Ограничения системы безопасности для SQL Server на Linux
description: В этой статье описываются ограничения SQL Server на Linux.
author: VanMSFT
ms.author: vanto
ms.date: 01/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 64da74cc-14bf-4636-a55e-8cc1fce2aaff
ms.openlocfilehash: 9f54197c8613293b36c1eb1ec362a8ed4db835e4
ms.sourcegitcommit: db9bed6214f9dca82dccb4ccd4a2417c62e4f1bd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68065118"
---
# <a name="security-limitations-for-sql-server-on-linux"></a>Ограничения системы безопасности для SQL Server на Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

SQL Server на Linux в настоящее время имеет указанные ниже ограничения.

* Применяется стандартная политика паролей. MUST_CHANGE — это единственный параметр, который можно настроить.  
* Расширенное управление ключами не поддерживается. 
* Использование ключей, хранящихся в Azure Key Vault, не поддерживается.
* SQL Server создает собственный самозаверяющий сертификат для шифрования подключений. В SQL Server можно настроить использование предоставляемого пользователем сертификата для TLS. 

Дополнительные сведения о функциях безопасности, доступных в SQL Server, см. в статье [Центр обеспечения безопасности для базы данных Azure SQL и SQL Server Database Engine](../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md).

## <a name="next-steps"></a>Следующие шаги

Описание стандартных задач по обеспечению безопасности см. в статье [Начало работы с функциями безопасности SQL Server на Linux](sql-server-linux-security-get-started.md). Скрипт для изменения номера TCP-порта и каталогов SQL Server, а также настройки флагов трассировки или параметров сортировки можно найти в статье [Настройка SQL Server в Linux с помощью средства mssql-conf](sql-server-linux-configure-mssql-conf.md).

---
title: Пакет дополнительных компонентов Azure для служб Integration Services (SSIS) | Документы Майкрософт
ms.custom: ''
ms.date: 05/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.SSIS.AZURE.F1
- SQL14.SSIS.AZURE.F1
ms.assetid: 31de555f-ae62-4f2f-a6a6-77fea1fa8189
author: janinezhang
ms.author: janinez
ms.openlocfilehash: 9241725a9f1da67ef93701b62c5cc4e8d9093a7a
ms.sourcegitcommit: a1adc6906ccc0a57d187e1ce35ab7a7a951ebff8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68892733"
---
# <a name="azure-feature-pack-for-integration-services-ssis"></a>Пакет дополнительных компонентов Azure для служб Integration Services (SSIS)

[!INCLUDE[ssis-appliesto](../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


Пакет дополнительных компонентов Azure для служб SQL Server Integration Services (SSIS) — это дополнение, которое предоставляет перечисленные на этой странице компоненты для подключения служб SSIS к Azure, передачи данных между Azure и локальными источниками данных и обработки данных, хранящихся в Azure.

[![Скачать пакет дополнительных компонентов служб SSIS для Azure](https://docs.microsoft.com/analysis-services/analysis-services/media/download.png)](https://www.microsoft.com/download/details.aspx?id=54798) **Скачать**

- Для SQL Server 2017: [Пакет дополнительных компонентов служб Microsoft SQL Server Integration Services 2017 для Azure](https://www.microsoft.com/download/details.aspx?id=54798)
- Для SQL Server 2016: [Пакет дополнительных компонентов служб Microsoft SQL Server Integration Services 2016 для Azure](https://www.microsoft.com/download/details.aspx?id=49492)
- Для SQL Server 2014: [Пакет дополнительных компонентов служб Microsoft SQL Server Integration Services 2014 для Azure](https://www.microsoft.com/download/details.aspx?id=47366)
- Для SQL Server 2012: [Пакет дополнительных компонентов служб Microsoft SQL Server Integration Services 2012 для Azure](https://www.microsoft.com/download/details.aspx?id=47367)

На страницах скачивания также приведены сведения о необходимых компонентах. SQL Server необходимо установить перед установкой пакета дополнительных компонентов Azure на сервере. В противном случае компоненты в составе пакета могут оказаться недоступными при развертывании пакетов в базе данных каталога служб SSIS (SSISDB) на сервере.

## <a name="components-in-the-feature-pack"></a>Компоненты в составе пакета дополнительных компонентов
-   Диспетчеры соединений

    -   [Диспетчер подключений Azure Data Lake Analytics](connection-manager/azure-data-lake-analytics-connection-manager.md)

    -   [Диспетчер подключений Azure Data Lake Store](../integration-services/connection-manager/azure-data-lake-store-connection-manager.md)
    
    -   [Диспетчер подключений Azure HDInsight](../integration-services/connection-manager/azure-hdinsight-connection-manager.md)

    -   [Диспетчер подключений Azure Resource Manager](../integration-services/connection-manager/azure-resource-manager-connection-manager.md)
    
    -   [Диспетчер подключений службы хранилища Azure](../integration-services/connection-manager/azure-storage-connection-manager.md)

    -   [Диспетчер подключений подписки Azure](../integration-services/connection-manager/azure-subscription-connection-manager.md)
    
-   Задания

    -   [Задача скачивания BLOB-объектов Azure](../integration-services/control-flow/azure-blob-download-task.md)

    -   [Задача передачи больших двоичных объектов Azure](../integration-services/control-flow/azure-blob-upload-task.md)

    -   [Задание Azure Data Lake Analytics](control-flow/azure-data-lake-analytics-task.md)

    -   [Задача «Файловая система» для Azure Data Lake](../integration-services/control-flow/azure-data-lake-store-file-system-task.md)

    -   [Задача создания кластера Azure HDInsight](../integration-services/control-flow/azure-hdinsight-create-cluster-task.md)

    -   [Задача удаления кластера Azure HDInsight](../integration-services/control-flow/azure-hdinsight-delete-cluster-task.md)
    
    -   [Задача Hive для Azure HDInsight](../integration-services/control-flow/azure-hdinsight-hive-task.md)

    -   [Задача Pig для Azure HDInsight](../integration-services/control-flow/azure-hdinsight-pig-task.md)

    -   [Задача отправки в хранилище данных Azure SQL](../integration-services/control-flow/azure-sql-dw-upload-task.md)

    -   [Задача "Гибкая работа с файлами"](../integration-services/control-flow/flexible-file-task.md)

-   Компоненты потока данных

    -   [Компонент Azure Blob Source](../integration-services/data-flow/azure-blob-source.md)

    -   [Назначение больших двоичных объектов Azure](../integration-services/data-flow/azure-blob-destination.md)
    
    -   [Источник Azure Data Lake Store](../integration-services/data-flow/azure-data-lake-store-source.md)
    
    -   [Цель Azure Data Lake Store](../integration-services/data-flow/azure-data-lake-store-destination.md)

    -   [Источник "Гибкая работа с файлами"](../integration-services/data-flow/flexible-file-source.md)

    -   [Назначение "Гибкая работа с файлами"](../integration-services/data-flow/flexible-file-destination.md)

-   BLOB-объект Azure, Azure Data Lake Store и перечислитель файлов Data Lake Storage 2-го поколения. См. раздел [Контейнер "цикл по каждому элементу"](../integration-services/control-flow/foreach-loop-container.md).

## <a name="use-tls-12"></a>Использование TLS 1.2

Версия TLS, используемая пакетом дополнительных компонентов Azure, соответствует параметрам системы .NET Framework.
Чтобы использовать TLS 1.2, добавьте значение `REG_DWORD` с именем `SchUseStrongCrypto` и данными `1` в следующих двух разделах реестра.

1. `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319`
2. `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319`

## <a name="dependency-on-java"></a>Зависимость от Java

Для использования некоторых функций требуется Java.
Архитектура сборки Java (32- или 64-разрядная) должна соответствовать архитектуре используемой среды выполнения SSIS.
Были протестированы следующие сборки Java:

- [Zulu OpenJDK 8u192](https://www.azul.com/downloads/zulu/zulu-windows/)
- [Среда выполнения Oracle Java SE 8u192](https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8-2177648.html)

### <a name="set-up-zulus-openjdk"></a>Настройка Zulu OpenJDK

1. Скачайте и извлеките ZIP-пакет установки.
2. Запустите `sysdm.cpl` из командной строки.
3. На вкладке **Дополнительно** выберите **Переменные среды**.
4. В разделе **Системные переменные** выберите **Создать**.
5. Введите `JAVA_HOME` в поле **Имя переменной**.
6. Выберите **Обзор каталога**, перейдите к извлеченной папке и выберите вложенную папку `jre`.
   Затем нажмите кнопку **ОК**. **Значение переменной** заполняется автоматически.
7. Нажмите кнопку **ОК**, чтобы закрыть диалоговое окно **Создание системной переменной**.
8. Нажмите кнопку **ОК**, чтобы закрыть диалоговое окно **Переменные среды**.
9. Нажмите кнопку **ОК**, чтобы закрыть диалоговое окно **Свойства системы**.

### <a name="set-up-zulus-openjdk-on-azure-ssis-integration-runtime"></a>Установка Zulu OpenJDK в Azure-SSIS Integration Runtime

Это действие должно выполняться посредством [интерфейса выборочной установки](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup) для Azure-SSIS Integration Runtime.
Допустим, используется `zulu8.33.0.1-jdk8.0.192-win_x64.zip`.
Контейнер больших двоичных объектов может быть организован указанным ниже образом.

~~~
main.cmd
install_openjdk.ps1
zulu8.33.0.1-jdk8.0.192-win_x64.zip
~~~

В качестве точки входа `main.cmd` запускает выполнение скрипта PowerShell `install_openjdk.ps1`, который, в свою очередь, извлекает `zulu8.33.0.1-jdk8.0.192-win_x64.zip` и задает `JAVA_HOME` соответствующим образом.

**main.cmd**

~~~
powershell.exe -file install_openjdk.ps1
~~~

**install_openjdk.ps1**

~~~
Expand-Archive zulu8.33.0.1-jdk8.0.192-win_x64.zip -DestinationPath C:\
[Environment]::SetEnvironmentVariable("JAVA_HOME", "C:\zulu8.33.0.1-jdk8.0.192-win_x64\jre", "Machine")
~~~

### <a name="set-up-oracles-java-se-runtime-environment"></a>Настройка среды выполнения Oracle Java SE

1. Скачайте и запустите EXE-установщик.
2. Следуйте указаниям установщика, чтобы завершить настройку.

## <a name="scenario-processing-big-data"></a>Сценарий: обработка больших данных
 Используйте соединитель Azure для выполнения следующих задач по обработке больших данных:

1.  Передача входных данных в хранилище больших двоичных объектов Azure с помощью задачи передачи больших двоичных объектов Azure.

2.  Создание кластера Azure HDInsight с помощью задачи создания кластера Azure HDInsight. Если вы хотите использовать собственный кластер, этот шаг является необязательным.

3.  Запуск задания Pig или Hive в кластере Azure HDInsight с помощью задачи по запуску задания Pig в Azure HDInsight или задачи по запуску задания Hive в Azure HDInsight.

4.  Удаление кластера Azure HDInsight после использования (если вы создавали кластер HDInsight по требованию на шаге 2) с помощью задачи удаления кластера Azure HDInsight.

5.  Скачивание выходных данных из хранилища больших двоичных объектов Azure с помощью задачи скачивания больших двоичных объектов Azure HDInsight.

![SSIS-AzureConnector-BigDataScenario](../integration-services/media/ssis-azureconnector-bigdatascenario.png)
 
## <a name="scenario-managing-data-in-the-cloud"></a>Сценарий: управление данными в облаке
 Используйте назначение больших двоичных объектов Azure в пакете SSIS для записи выходных данных в хранилище BLOB-объектов Azure или источник больших двоичных объектов Azure для чтения данных из хранилища BLOB-объектов Azure.

![SSIS-AzureConnector-CloudArchive-1](../integration-services/media/ssis-azureconnector-cloudarchive-1.png)
 
 ![SSIS-AzureConnector-CloudArchive-2](../integration-services/media/ssis-azureconnector-cloudarchive-2.png)

 Используйте контейнер цикла Foreach с перечислителем BLOB-объектов Azure для обработки данных в нескольких файлах больших двоичных объектов.

![SSIS-AzureConnector-CloudArchive-3](../integration-services/media/ssis-azureconnector-cloudarchive-3.png)
  

---
title: Запуск примера записной книжки | Документация Майкрософт
titleSuffix: SQL Server big data clusters
description: В этом руководстве демонстрируется загрузка и запуск примера записной книжки в кластере больших данных SQL Server 2019 (предварительная версия).
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 12/06/2018
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ab558194a67118719c144ea20f9e97496d2cb478
ms.sourcegitcommit: db9bed6214f9dca82dccb4ccd4a2417c62e4f1bd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "67957735"
---
# <a name="tutorial-run-a-sample-notebook-on-a-sql-server-big-data-cluster"></a>Руководство. Запуск примера записной книжки в кластере больших данных SQL Server

[!INCLUDE[tsql-appliesto-ssver15-xxxx-xxxx-xxx](../includes/tsql-appliesto-ssver15-xxxx-xxxx-xxx.md)]

В этом руководстве демонстрируется загрузка и запуск записной книжки Azure Data Studio в кластере больших данных SQL Server 2019 (предварительная версия). Такой подход позволяет специалистам по обработке и анализу данных выполнять код Python, R или Scala в кластере.

> [!TIP]
> При необходимости вы можете скачать и выполнить скрипт, содержащий команды из этого руководства. См. инструкции в [примерах Spark](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark) на сайте GitHub.

## <a id="prereqs"></a> Предварительные требования

- [Средства работы с большими данными](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **Расширение SQL Server 2019**
- [Загрузка примера данных в кластер больших данных](tutorial-load-sample-data.md)

## <a name="download-the-sample-notebook-file"></a>Скачивание файла с примером записной книжки

Чтобы загрузить файл с примером записной книжки **spark-sql.ipynb** в Azure Data Studio, выполните следующие инструкции.

1. Откройте командную строку Bash (Linux) или Windows PowerShell.

1. Перейдите в каталог, в который требуется скачать файл с примером записной книжки.

1. Выполните следующую команду **curl**, чтобы скачать файл с записной книжкой с сайта GitHub:

   ```bash
   curl 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/spark/data-loading/transform-csv-files.ipynb' -o transform-csv-files.ipynb
   ```

## <a name="open-the-notebook"></a>Открытие записной книжки

Выполните следующие действия, чтобы открыть файл с записной книжкой в Azure Data Studio:

1. В Azure Data Studio установите подключение к главному экземпляру в кластере больших данных. Дополнительные сведения см. в разделе [Подключение к кластеру больших данных](connect-to-big-data-cluster.md).

1. Дважды щелкните подключение шлюза HDFS/Spark в окне **Серверы**. Выберите **Открыть записную книжку**.

   ![Открытие записной книжки](media/tutorial-notebook-spark/azure-data-studio-open-notebook.png)

1. Дождитесь, пока будет заполнено поле **Ядро** и задан целевой контекст (**Присоединить к**). В поле **Ядро** укажите **PySpark3**, а в поле **Присоединить к** введите IP-адрес конечной точки вашего кластера больших данных.

   ![Заполнение полей "Ядро" и "Присоединить к"](media/tutorial-notebook-spark/set-kernel-and-attach-to.png)

## <a name="run-the-notebook-cells"></a>Запуск ячеек записной книжки

Чтобы запустить любую ячейку записной книжки, нажмите расположенную слева от нее кнопку воспроизведения. После того как выполнение ячейки будет завершено, в записной книжке будут показаны результаты.

![Запуск ячейки записной книжки](media/tutorial-notebook-spark/run-notebook-cell.png)

Последовательно запустите все ячейки в примере записной книжки. Дополнительные сведения об использовании записных книжек в кластерах больших данных SQL Server см. в следующих ресурсах:

- [Использование записных книжек в предварительной версии SQL Server 2019](notebooks-guidance.md)
- [Как управлять записными книжками в Azure Data Studio](notebooks-how-to-manage.md)

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о записных книжках:
> [!div class="nextstepaction"]
> [Дополнительные сведения о записных книжках](notebooks-guidance.md)

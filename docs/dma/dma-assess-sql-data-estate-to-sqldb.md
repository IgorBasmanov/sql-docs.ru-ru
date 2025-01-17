---
title: Оценка готовности SQL Serverного пространства данных для переноса в базу данных SQL Azure | Документация Майкрософт
description: Узнайте, как использовать Помощник по миграции данных для переноса SQL Serverного пространства данных для миграции в базу данных SQL Azure.
ms.custom: ''
ms.date: 07/16/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, on-premises SQL Server
ms.assetid: ''
author: HJToland3
ms.author: rajpo
manager: jroth
ms.openlocfilehash: b3f47cee5cc091c52faa98438d22b88a18a06f03
ms.sourcegitcommit: e821cd8e5daf95721caa1e64c2815a4523227aa4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68702822"
---
# <a name="assess-the-readiness-of-a-sql-server-data-estate-migrating-to-azure-sql-database-using-the-data-migration-assistant"></a>Оценка готовности SQL Serverного пространства данных для переноса в базу данных SQL Azure с помощью Помощник по миграции данных

Миграция сотен экземпляров SQL Server и тысяч баз данных в базу данных SQL Azure, предлагаемая нашей платформой как услуга (PaaS), является значительной задачей. Чтобы максимально упростить процесс, необходимо быть уверенным в том, что относительная готовность к миграции. Определение низкого уровня фруктов, включая серверы и базы данных, полностью готовые или требующие минимальных усилий для подготовки к миграции, упрощает и ускоряет работу.

В этой статье приводятся пошаговые инструкции по использованию [Помощник по миграции данных](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017) для суммирования результатов готовности и их последующей поверхности в центре [миграции Azure](https://portal.azure.com/?feature.customPortal=false#blade/Microsoft_Azure_Migrate/AmhResourceMenuBlade/overview) .


> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Data-Migration-Assistant/player?WT.mc_id=dataexposed-c9-niner]

## <a name="create-a-project-and-add-a-tool"></a>Создание проекта и Добавление средства

Настройте новый проект службы "миграция Azure" в подписке Azure, а затем добавьте средство.

Проект службы "миграция Azure" используется для хранения метаданных обнаружения, оценки и миграции, собранных из среды, для которой выполняется оценка или миграция. Кроме того, вы можете использовать проект для мониторинга обнаруженных ресурсов, а также для оркестрации оценки и миграции.

1. Войдите в портал Azure, выберите **все службы**, а затем выполните поиск по запросу "миграция Azure".
2. В разделе **службы**выберите **Миграция Azure**.

   ![Служба "миграция Azure" — Выбор службы](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-services.png)

3. На странице **Обзор** выберите **Оценка и миграция баз данных**.

   ![Служба "миграция Azure" — Запуск оценки](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-hub-assess.png)

4. В области **базы данных**в разделе **Приступая к работе**выберите **Добавить средства**.

   ![Служба "миграция Azure" — Добавление средств](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-add-tools.png)

5. На вкладке **Миграция проекта** выберите подписку Azure и группу ресурсов (если у вас еще нет группы ресурсов, создайте ее).
6. В разделе **сведения о проекте**укажите имя проекта и географию, в котором нужно создать проект.

    ![Диалоговое окно "миграция Azure — Добавление средства"](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-add-tool-dialog.png)

    Вы можете создать проект службы "миграция Azure" в любой из этих географических регионов.

    | **Geography**  | **Регион расположения хранилища** |
    | ------------- | ------------- |
    | Азия | Юго-Восточная Азия или Восточная Азия |
    | Европа | Южная Европа или Западная Европа |
    | United Kingdom | южная часть Соединенного Королевства или западная часть Соединенного Королевства |
    | США | Центральная часть США или Западная часть США 2 |

    Географический элемент, указанный для проекта, используется только для хранения метаданных, собранных из локальных виртуальных машин. Можно выбрать любой целевой регион для фактической миграции.

7. Нажмите кнопку **Далее**и добавьте средство оценки.

   > [!NOTE]
   > При создании проекта необходимо добавить хотя бы один инструмент оценки или миграции.

8. На **вкладке **Выбор инструмента для оценки** Azure выполните следующие действия. Оценка** базы данных отображается как средство оценки для добавления. Если средство оценки в настоящее время не требуется, установите флажок **пропустить Добавление средства оценки для Now** . Выберите **Далее**.

    ![Служба "миграция Azure" — Выбор средства оценки](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-select-assessment-tool.png)

9. На **вкладке **Выбор средства миграции** Azure выполните следующие действия. Миграция** базы данных отображается как средство миграции для добавления. Если средство миграции в настоящее время не требуется, установите флажок **пропустить Добавление средства миграции**. Выберите **Далее**.

    ![Служба "миграция Azure" — Выбор средства миграции](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-select-migration-tool.png)

10. На странице **Проверка и добавление инструментов**проверьте параметры и нажмите кнопку **Добавить инструменты**.

    ![Вкладка "миграция Azure" — "Проверка и добавление инструментов"](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-review-tools.png)

    После создания проекта можно выбрать дополнительные средства для оценки и миграции серверов и рабочих нагрузок, баз данных и веб-приложений.

## <a name="assess-and-upload-assessment-results"></a>Оценка и передача результатов оценки

После успешного создания проекта миграции в разделе **средства оценки**в службе " **миграция Azure" выполните следующие действия. Поле оценки** базы данных. инструкции по скачиванию и использованию средства помощник по миграции данных.

   ![Добавлено средство "миграция Azure — оценка"](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessment-tool-added.png)

1. Скачайте Помощник по миграции данных с помощью предоставленной ссылки, а затем установите ее на компьютере с доступом к экземплярам исходного SQL Server.
2. Запустите Помощник по миграции данных.

### <a name="create-an-assessment"></a>Создание оценки

1. В левой части **+** щелкните значок, а затем выберите **тип проекта** оценки.
2. Укажите имя проекта, а затем выберите типы исходного сервера и целевого сервера.

    Если вы обновляете локальный экземпляр SQL Server до более поздней версии SQL Server или для SQL Server, размещенного на виртуальной машине Azure, задайте для параметра Тип исходного и целевого сервера значение **SQL Server**. Задайте тип целевого сервера **управляемый экземпляр базы данных SQL Azure** для оценки готовности целевой базы данных SQL Azure (PaaS).

3. Выберите **Создать**.

   ![Интерфейс Помощник по миграции данных службы "миграция Azure"](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-interface.png)

### <a name="choose-assessment-options"></a>Выбор параметров оценки

1. Выберите тип отчета.

    Можно выбрать один или оба следующих типа отчетов:
    * Проверка совместимости базы данных
    * Проверить четность функций

   ![Экран "миграция Azure" — "Помощник по миграции данных" — "параметры оценки"](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-options-screen.png)

2. Выберите **Далее**.

### <a name="add-databases-to-assess"></a>Добавление баз данных для оценки

1. Выберите **Добавить источники** , чтобы открыть меню подключения.
2. Введите имя экземпляра SQL Server, выберите тип проверки подлинности, задайте правильные свойства соединения, а затем нажмите кнопку **подключить**.
3. Выберите базы данных для оценки, а затем нажмите кнопку **Добавить**.

   > [!NOTE]
   > Можно удалить несколько баз данных, выбрав их, удерживая клавишу Shift или CTRL, а затем выбрав Удалить источники. Можно также добавлять базы данных из нескольких экземпляров SQL Server с помощью кнопки Добавить источники.

4. Нажмите кнопку **Далее** , чтобы начать оценку.

   ![Служба "миграция Azure" — Помощник по миграции данных "Выбор источников"](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-select-sources-screen.png)

5. После завершения оценки выберите **отправить в**службу "миграция Azure".

   ![Экран "миграция Azure — Помощник по миграции данных" — "результаты проверки"](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-dma-review-results-screen.png)

6. Войдите на портал Azure.

   ![Экран "миграция Azure — Помощник по миграции данных" — "результаты проверки"](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-portal-signin.png)

7. Выберите подписку и проект службы "миграция Azure", в которые необходимо отправить результаты оценки, а затем нажмите кнопку **Отправить**.

   Дождитесь подтверждения отправки оценки.

## <a name="view-target-readiness-assessment-results"></a>Просмотр результатов оценки готовности к оценке

1. Войдите в портал Azure, выполните поиск по запросу "миграция Azure" и выберите " **Миграция Azure**".

   ![Служба "миграция Azure" — портал Azure — Поиск службы](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-portal-search.png)

2. Выберите **Оценка и миграция баз данных** , чтобы получить результаты оценки.

   ![Служба "миграция Azure" — Проверка результатов оценки](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-hub-assess.png)

    Вы можете просмотреть сводку о готовности SQL Server, убедиться, что вы являетесь в нужном проекте миграции. в противном случае используйте параметр изменить, чтобы выбрать другой проект миграции.

    Каждый раз при обновлении результатов оценки до проекта "миграция Azure" центр миграции Azure объединяет все результаты и предоставляет сводный отчет.  Можно выполнить несколько Помощник по миграции данныхных оценок параллельно и отправить результаты в единый проект миграции, чтобы получить консолидированный отчет о готовности.

   ![Служба "миграция Azure" — Проверка результатов готовности](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-review-readiness.png)

    **Оценка экземпляров базы данных**:  Количество экземпляров SQL Server, оцененных на данный момент.
    **Оценка баз данных**: Общее число баз данных, оцененных по одному или нескольким экземплярам SQL Server, **готовых для**базы данных SQL:  Количество баз данных, готовых к миграции в базу данных SQL Azure (PaaS).
    **Базы данных, готовые для виртуальной машины SQL Azure**:  Число баз данных состоит из одного или нескольких блокирований миграции в базу данных SQL Azure (PaaS), но готовы к миграции на виртуальные машины Azure SQL Server.

3. Выберите **оцененные экземпляры базы данных** , чтобы перейти к представлению SQL Server уровня экземпляра.

   ![Служба "миграция Azure" — Проверка готовности экземпляра](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessed-instances.png)

    Состояние готовности в процентах для каждого экземпляра SQL Server, переносимого в базу данных SQL Azure (PaaS), можно найти.

4. Выберите конкретный экземпляр, чтобы перейти к представлению готовность базы данных.

   ![Служба "миграция Azure" — Проверка готовности базы данных](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-assessed-databases.png)

    Можно увидеть количество блокирований миграции на каждую базу данных, рекомендуемый целевой объект для каждой базы данных в приведенном выше представлении.

5. Ознакомьтесь с результатами оценки DMA, чтобы получить дополнительные сведения о блокировках миграции.

   ![Миграция Azure — проверка блокирования миграции](../dma/media//dma-assess-sql-data-estate-to-sqldb/dms-azure-migrate-migration-blockers.png)

## <a name="see-also"></a>См. также

* [Помощник по миграции данных (DMA)](../dma/dma-overview.md)
* [Помощник по миграции данных: Параметры конфигурации](../dma/dma-configurationsettings.md)
* [Помощник по миграции данных: Рекомендации](../dma/dma-bestpractices.md)

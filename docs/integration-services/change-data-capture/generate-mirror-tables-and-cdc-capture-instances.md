---
title: Создание экземпляров зеркальных таблиц и отслеживание CDC | Документы Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- mirTab
ms.assetid: 260c1617-eecc-4007-a84d-3c5778ce46b6
author: janinezhang
ms.author: janinez
ms.openlocfilehash: dbefe9209072ec6e68e393bcef6f56360e6afe58
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68011386"
---
# <a name="generate-mirror-tables-and-cdc-capture-instances"></a>Создание экземпляров зеркальных таблиц и отслеживание CDC

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


  Страница формирования зеркальных таблиц используется для создания зеркальных таблиц для таблиц, включенных в экземпляр CDC.  
  
 Нажмите кнопку **Выполнить** , чтобы создать зеркальные таблицы. Процесс создания каждой таблицы отображается на экране, а по его окончании выдается сообщение, в котором говорится, была ли таблица создана успешно. Если возникли ошибки, нажмите кнопку **Сведения** , чтобы открыть диалоговое окно с описанием ошибки.  
  
 Если какую-либо таблицу создать не удалось, то можно либо продолжить выполнение, либо удалить таблицу, которую не удалось создать, а затем продолжить работу. После завершения работы мастера можно решить, следует ли исправить таблицу в базе данных-источнике Oracle или не использовать ее в экземпляре CDC. Если выбрано исправление таблицы, то ее можно будет добавить через [Edit Tables](../../integration-services/change-data-capture/edit-tables.md).  
  
 Нажмите кнопку **Далее** , чтобы открыть страницу [Finish](../../integration-services/change-data-capture/finish.md) .  
  
## <a name="see-also"></a>См. также:  
 [Как создать экземпляр базы данных изменений SQL Server](../../integration-services/change-data-capture/how-to-create-the-sql-server-change-database-instance.md)  
  
  

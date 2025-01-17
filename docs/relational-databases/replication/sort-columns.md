---
title: Сортировка столбцов | Документация Майкрософт
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.monitor.sortcolumns.f1
ms.assetid: 66b44b6c-10a5-4e3f-a97b-7568609c88ac
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2014||=sqlallproducts-allversions
ms.openlocfilehash: 0f7cca09018f9486c831e3803aedb5c969c422d1
ms.sourcegitcommit: 728a4fa5a3022c237b68b31724fce441c4e4d0ab
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2019
ms.locfileid: "68769503"
---
# <a name="sort-columns"></a>Сортировка столбцов
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]
  Диалоговое окно **Сортировка столбцов** позволяет сортировать сетки в мониторе репликации по значению одного или нескольких столбцов. (Отсортировать по одному столбцу можно, щелкнув его заголовок в сетке монитора репликации). Например, чтобы отсортировать подписки на вкладке **Все подписки** по состоянию, а затем по типу соединения, выполните следующие действия.  
  
1.  В первой строке сетки выберите **Состояние** в столбце **Имя столбца** и значение в столбце **Порядок сортировки** .  
  
2.  Во второй строке сетки выберите **Тип соединения** в столбце **Имя столбца** и значение в столбце **Порядок сортировки** .  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

## <a name="options"></a>Параметры  
 **Имя столбца**  
 Имя столбца, по которому нужно выполнить сортировку. Можно выполнить сортировку по одному или нескольким столбцам. Нельзя выполнить сортировку по столбцам **Текущая средняя производительность** и **Худшая производительность в данный момент** на вкладке **Публикации** из-за способа вычисления значений в этих столбцах.  
  
 **Порядок сортировки**  
 Укажите значение **По возрастанию** или **По убыванию**.  
  
 **Очистить все**  
 Удалите все строки из сетки сортировки. Для удаления одной строки выделите строку и нажмите клавишу «DELETE».  
  
## <a name="see-also"></a>См. также:  
 [Наблюдение за репликацией](../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  

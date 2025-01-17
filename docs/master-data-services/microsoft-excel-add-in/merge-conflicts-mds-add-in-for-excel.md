---
title: Слияние конфликтов (надстройка MDS для Excel) | Документы Майкрософт
ms.custom: microsoft-excel-add-in
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: cf95978f-a2c5-4325-8606-dbd4e88741b8
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 22c25126106c6b93d779b6c494af0bff7b5b08af
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68074565"
---
# <a name="merge-conflicts-mds-add-in-for-excel"></a>Слияние конфликтов (надстройка MDS для Excel)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  В надстройке [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] для Excel попытка публикации данных, измененных на сервере другим пользователем, завершится ошибкой из-за конфликта. Чтобы устранить эту ошибку, можно выполнить слияние конфликтов и повторно опубликовать изменения.  
  
## <a name="prerequisites"></a>предварительные требования  
 Для выполнения этой процедуры:  
  
-   Необходимо иметь разрешение на доступ к функциональной области **Обозреватель** .  
  
-   Как минимум, необходимо разрешение на обновление для конечного объекта модели для сущности, которую нужно обновить.  
  
-   Необходимо наличие данных, управляемых MDS, на активном листе в Excel.  
  
-   На активном листе должна возникнуть ошибка конфликта после попытки публикации изменений.  
  
### <a name="to-merge-conflicts"></a>Слияние конфликтов  
  
1.  На листе выберите строку или ячейку, содержащую ошибку конфликта.  
  
2.  В группе меню **Опубликовать и проверить** выберите пункт **Объединить конфликты** , чтобы открыть диалоговое окно **Объединить конфликты** .  
  
3.  В диалоговом окне **Объединить конфликты** можно выполнить одно из указанных ниже действий.  
  
    -   Выберите **Последний** и нажмите кнопку **Применить** , чтобы отменить ожидающие изменения и повторно загрузить последнюю версию с сервера.  
  
    -   Выберите **Исходный** и нажмите кнопку **Применить** , чтобы применить исходную версию на листе.  
  
    -   Выберите **Ваш** и нажмите кнопку **Применить** , чтобы сохранить существующие локальные изменения.  
  
4.  Нажав кнопку **Применить**, можно внести дополнительные изменения и опубликовать данные снова. Можно также нажать кнопку **Отменить** , чтобы отменить обновление и повторно загрузить последнюю версию с сервера.  
  
## <a name="see-also"></a>См. также  
 [Обзор: Импорт данных из Excel &#40;надстройка MDS для Excel&#41;](../../master-data-services/microsoft-excel-add-in/overview-importing-data-from-excel-mds-add-in-for-excel.md)  
  
  

---
title: Очистка элементов версии (службы Master Data Services) | Документы Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
ms.assetid: adecce2d-46bb-49ff-8be9-0b31b8dd3cb6
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 571ac34ad836bf25ad98f973f9fc33c9de499ba9
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68024650"
---
# <a name="purge-version-members-master-data-services"></a>Очистка элементов версии (Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  При удалении элемента в [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]он только деактивируется (удаляется обратимо). Данные по-прежнему находятся в базе данных. В этом разделе описывается, как очистить (навсегда удалить) все обратимо удаленные элементы и версии модели.  
  
## <a name="prerequisites"></a>предварительные требования  
 Чтобы выполнить эту процедуру:  
  
-   Необходимо иметь разрешение на доступ к функциональной области "Управление версиями".  
  
-   необходимо быть администратором модели. Дополнительные сведения см. в статье [Администраторы (службы Master Data Services)](../master-data-services/administrators-master-data-services.md).  
  
## <a name="to-purge-soft-deleted-members"></a>Очистка обратимо удаленных элементов  
  
1.  В среде [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]щелкните область **Управление версиями**.  
  
2.  На странице **Управление версиями** выберите модель, версию которой необходимо очистить. Отобразится список версий модели.  
  
3.  Выберите строку для версии, которую необходимо очистить.  
  
4.  Щелкните **Удалить элементы**.  
  
5.  В окне подтверждения нажмите кнопку "ОК".  
  
## <a name="additional-methods-to-purge-members"></a>Дополнительные методы очистки элементов  
 При очистке элементов версии безвозвратно удаляются обратимо удаленные элементы всех сущностей, которые относятся к выбранной версии. Как вариант, можно использовать метод промежуточного хранения на основе сущностей, который позволяет безвозвратно удалить только определенные элементы сущности. Кроме того, администраторы сущности с разрешением на доступ к функциональной области обозревателя могут очистить версию сущности на странице обозревателя сущностей.  
  
 Дополнительные сведения см. в разделе [Конечный элемент таблицы элементов (службы Master Data Services)](../master-data-services/leaf-member-staging-table-master-data-services.md)  
  
  

---
title: Удаление бизнес-правила (службы Master Data Services) | Документы Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- deleting business rules [Master Data Services]
- business rules [Master Data Services], deleting
ms.assetid: b97aa4f9-569f-451d-ad62-65b81f980299
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: ec560acba23c5423c295f569745da1f8d6bc86e8
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68094401"
---
# <a name="delete-a-business-rule-master-data-services"></a>Удаление бизнес-правила (службы Master Data Services)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  В [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]можно удалить бизнес-правило, которое больше не нужно.  
  
> [!NOTE]  
>  Чтобы предотвратить выполнение проверки данных на соответствие бизнес-правилу, можно исключить эти данные, а не удалить их. Дополнительные сведения см. в разделе [Исключение бизнес-правила (службы Master Data Services)](../master-data-services/exclude-a-business-rule-master-data-services.md).  
  
## <a name="prerequisites"></a>предварительные требования  
 Для выполнения этой процедуры:  
  
-   необходимо иметь разрешение на доступ к функциональной области **Администрирование системы** ;  
  
-   необходимо быть администратором модели. Дополнительные сведения см. в статье [Администраторы (службы Master Data Services)](../master-data-services/administrators-master-data-services.md).  
  
### <a name="to-delete-a-business-rule"></a>Удаление бизнес-правила  
  
1.  В [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]щелкните область **Администрирование системы**.  
  
2.  В строке меню выберите **Управление** и щелкните **Бизнес-правила**.  
  
3.  На странице **Бизнес-правила** в раскрывающемся списке **Модель** выберите модель.  
  
4.  Из раскрывающегося списка **Сущность** выберите сущность.  
  
5.  Из раскрывающегося списка **Тип элемента** выберите тип элемента, к которому будет применяться бизнес-правило.  
  
6.  В сетке щелкните строку бизнес-правила, которое необходимо удалить.  
  
7.  Щелкните **Удалить**.  
  
8.  В диалоговом окне подтверждения нажмите кнопку **ОК**. Значение в столбце **Состояние бизнес-правила** изменится на **Ожидается удаление**.  
  
9. Нажмите кнопку **Опубликовать все**.  
  
10. В диалоговом окне подтверждения нажмите кнопку **ОК**. Удаленное бизнес-правило больше не отображается в сетке.  
  
## <a name="see-also"></a>См. также  
 [Исключение бизнес-правила (службы Master Data Services)](../master-data-services/exclude-a-business-rule-master-data-services.md)   
 [Создание и публикация бизнес-правила (службы Master Data Services)](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)   
 [Бизнес-правила (службы Master Data Services)](../master-data-services/business-rules-master-data-services.md)  
  
  

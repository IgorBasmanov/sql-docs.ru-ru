---
title: Создание связей | Документы Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.createrelationships.f1
ms.assetid: 6ebd305f-ffd2-4a1d-b24c-e28c151b94f5
author: janinezhang
ms.author: janinez
ms.openlocfilehash: 8d9eb72289597b873a340815ea0e388b952de37c
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68112571"
---
# <a name="create-relationships"></a>Создание связей

[!INCLUDE[ssis-appliesto](../../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


  Используйте диалоговое окно **Создание связей** , чтобы изменить сопоставления между исходными столбцами и столбцами таблицы уточняющих запросов, настроенные в редакторе преобразования «Нечеткий уточняющий запрос», в редакторах преобразования «Уточняющий запрос» и «Уточняющий запрос термина».  
  
> [!NOTE]  
>  Диалоговое окно **Создание связей** отображает только списки **Входной столбец** и **Столбец подстановок** , если вызвано из редактора преобразования "Уточняющий запрос термина".  
  
 Дополнительные сведения о преобразованиях, использующих диалоговое окно **Создание связей** см. в разделах [Fuzzy Lookup Transformation](../../../integration-services/data-flow/transformations/fuzzy-lookup-transformation.md), [Lookup Transformation](../../../integration-services/data-flow/transformations/lookup-transformation.md)и [Term Lookup Transformation](../../../integration-services/data-flow/transformations/term-lookup-transformation.md).  
  
## <a name="options"></a>Параметры  
 **Входной столбец**  
 Выберите входной столбец из списка имеющихся входных столбцов.  
  
 **Столбец подстановок**  
 Выберите из списка доступных столбцов подстановок.  
  
 **Тип сопоставления**  
 Выберите нечеткое или четкое соответствие.  
  
 При использовании нечеткого соответствия строки рассматриваются как дубликаты, если они в значительной степени совпадают по всем столбцам, имеющим нечеткий тип соответствия. Для получения лучших результатов от нечеткого соответствия можно указать, чтобы некоторые столбцы использовали четкое соответствие вместо нечеткого. Например, если известно, что конкретный столбец не содержит ошибок или противоречий, то возможно указать для этого столбца четкое соответствие, таким образом, чтобы в качестве дубликатов рассматривались лишь те строки, которые содержат идентичные значения в этом столбце. Это увеличивает степень точности нечеткого соответствия по другим столбцам.  
  
 **Флаги сравнения**  
 Дополнительные сведения о параметрах сравнения строк см. в разделе [Сравнение строковых данных](../../../integration-services/data-flow/comparing-string-data.md).  
  
 **Минимальное подобие**  
 Установите порог подобия на уровне столбцов, используя ползунок. Чем ближе это значение к 1, тем ближе должно быть сходство между значением уточняющего столбца и исходным значением, чтобы они считались соответствующими. Увеличение этого порога может повысить скорость нахождения совпадений, так как количество рассматриваемых потенциальных записей будет меньше.  
  
 **Псевдоним выхода подобия**  
 Укажите имя для нового выходного столбца, который является подобным выбранному столбцу. Если оставить это значение пустым, то выходной столбец не будет создан.  
  
## <a name="see-also"></a>См. также:  
 [Справочник по сообщениям об ошибках служб Integration Services](../../../integration-services/integration-services-error-and-message-reference.md)   
 [Редактор преобразования "Нечеткий уточняющий запрос" (вкладка "Столбцы")](../../../integration-services/data-flow/transformations/fuzzy-lookup-transformation-editor-columns-tab.md)   
 [Редактор преобразования "Уточняющий запрос" (страница "Столбцы")](../../../integration-services/data-flow/transformations/lookup-transformation-editor-columns-page.md)   
 [Редактор преобразований "Уточняющий запрос термина" (вкладка "Уточняющий запрос термина")](../../../integration-services/data-flow/transformations/term-lookup-transformation-editor-term-lookup-tab.md)  
  
  

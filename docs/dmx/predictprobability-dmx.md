---
title: PredictProbability (расширения интеллектуального анализа данных) | Документация Майкрософт
ms.date: 06/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: dmx
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: bd8365b2d3aac82c184af549ef21952fd4c8e649
ms.sourcegitcommit: a1adc6906ccc0a57d187e1ce35ab7a7a951ebff8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68893906"
---
# <a name="predictprobability-dmx"></a>PredictProbability (расширения интеллектуального анализа данных)
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  Возвращает вероятность для указанного состояния.  
  
## <a name="syntax"></a>Синтаксис  
  
```  
  
PredictProbability(<scalar column reference>, [<predicted state>])  
```  
  
## <a name="applies-to"></a>Объект применения  
 Скалярный столбец.  
  
## <a name="return-type"></a>Тип возвращаемых данных  
 Скалярное значение.  
  
## <a name="remarks"></a>Примечания  
 Если прогнозируемое состояние не указано, используется состояние с наибольшей вероятностью, за исключением сегмента отсутствующих состояний. Чтобы включить контейнер отсутствующих состояний, задайте \<для прогнозируемого состояния > значение **INCLUDE_NULL**. Чтобы получить вероятность отсутствия состояний, задайте \<для прогнозируемого состояния > значение null.  
  
> [!NOTE]  
>  В некоторых моделях интеллектуального анализа данных значения вероятности не применяются, поэтому в таких моделях нельзя пользоваться этой функцией. Кроме того, значения вероятности для любого конкретного целевого значения рассчитываются по-разному или могут по-разному интерпретироваться в зависимости от типа модели, к которой обращен запрос. Дополнительные сведения о вычислении вероятности для конкретного типа модели см. в разделе "индивидуальный алгоритм" статьи [интеллектуального анализа &#40;содержимого Analysis Services — интеллектуальный&#41;анализ данных](https://docs.microsoft.com/analysis-services/data-mining/mining-model-content-analysis-services-data-mining).  
  
## <a name="examples"></a>Примеры  
 В следующем примере используется естественное прогнозируемое соединение для определения вероятности покупки велосипеда человеком на основе модели интеллектуального анализа данных TM-дерева принятия решений, а также определяется вероятность для этого прогноза. В данном примере имеется две функции PredictProbability — по одной для каждого возможного значения. Если опустить этот аргумент, функция возвратит вероятность наиболее вероятного значения.  
  
```  
SELECT  
  [Bike Buyer],  
  PredictProbability([Bike Buyer], 1) AS [Bike Buyer = Yes],  
  PredictProbability([Bike Buyer], 0) AS [Bike Buyer = No]  
FROM [TM Decision Tree]  
NATURAL PREDICTION JOIN  
(SELECT 28 AS [Age],  
  '2-5 Miles' AS [Commute Distance],  
  'Graduate Degree' AS [Education],  
  0 AS [Number Cars Owned],  
  0 AS [Number Children At Home]) AS t  
```  
  
 Пример результатов:  
  
|Покупатель велосипеда|Bike Buyer = Yes|Bike Buyer = No|  
|----------------|-----------------------|----------------------|  
|1|0.867074195848097|0.132755556974282|  
  
## <a name="see-also"></a>См. также  
 [Справочник по &#40;функциям&#41; DMX расширений интеллектуального анализа данных](../dmx/data-mining-extensions-dmx-function-reference.md)   
 [DMX &#40;-функции&#41;](../dmx/functions-dmx.md)   
 [DMX-функции &#40;общих прогнозирующих функций&#41;](../dmx/general-prediction-functions-dmx.md)  
  
  

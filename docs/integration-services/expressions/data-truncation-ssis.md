---
title: Усечение данных (службы SSIS) | Документы Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- truncating data
- data truncation [Integration Services]
- expressions [Integration Services], data truncation
- truncate options [Integration Services]
ms.assetid: 02461e15-49ca-445b-abb3-5c369c283ec2
author: janinezhang
ms.author: janinez
ms.openlocfilehash: c7b122bde62bdae597498c0cf0992ff1c0732c6a
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68081134"
---
# <a name="data-truncation-ssis"></a>Усечение данных (службы SSIS)

[!INCLUDE[ssis-appliesto](../../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]


  Преобразование значений из одного типа данных в другой может привести к их усечению.  
  
 Усечение может произойти в следующих случаях.  
  
-   Преобразование строковых данных из *DT_WSTR* в *DT_STR* той же длины, если исходная строка содержит двухбайтовые символы.  
  
-   Приведение целого числа из *DT_I4* в *DT_I2* — могут быть потеряны значащие цифры.  
  
-   Приведение целого числа без знака в целое число со знаком — могут быть потеряны значащие цифры.  
  
-   Приведение действительного числа из *DT_R8* в *DT_R4* — могут быть потеряны незначащие цифры.  
  
-   Приведение целого числа из *DT_I4* в *DT_R4* — могут быть потеряны незначащие цифры.  
  
 Средство оценки выражений определяет явные приведения, которые могут привести к усечению, и выдает предупреждение, если выражение прошло синтаксический анализ. Например, средство оценки выражений выдаст предупреждение, если строка, состоящая из 30 символов, будет преобразована в строку, состоящую из 20 символов.  
  
 Однако усечение не проверяется во время выполнения. Во время выполнения данные усекаются без предупреждения. Большинство адаптеров обработки данных и преобразований поддерживают сообщения об ошибках, которые позволяют обработать неправильно расположенные ошибочные строки.  
  
 Дополнительные сведения об обработке усечения данных см. в статье [Обработка ошибок в данных](../../integration-services/data-flow/error-handling-in-data.md).  
  
  

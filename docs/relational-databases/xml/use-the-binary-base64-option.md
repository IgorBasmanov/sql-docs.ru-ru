---
title: Использование параметра BINARY BASE64 | Документация Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- AUTO FOR XML mode, BINARY BASE64 option
ms.assetid: 86a7bb85-7f83-412a-b775-d2c379702fe9
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 84d715b1e90c54c549a255b3ee43f24f2e5556d3
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68039160"
---
# <a name="use-the-binary-base64-option"></a>Использование параметра BINARY BASE64
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Если в запросе задан аргумент BINARY BASE64, двоичные данные возвращаются в формате кодирования base64. По умолчанию, если аргумент BINARY BASE64 не задан, режим AUTO поддерживает кодирование URL-адресов двоичных данных. Это означает, что вместо двоичных данных возвращается ссылка на относительный URL-адрес виртуального корня базы данных, в которой выполнялся запрос. Эта ссылка может использоваться для доступа к действительным двоичным данным в последующих операциях при помощи запроса объекта базы данных SQLXML ISAPI. Для идентификации изображения запрос должен предоставить достаточно данных, например первичные ключевые столбцы.  
  
 При задании запроса, если используется псевдоним для двоичного столбца представления, псевдоним возвращается в виде URL-адреса двоичных данных. В последующих операциях псевдоним не имеет смысла, и URL-адрес не может использоваться для получения изображения. Поэтому не используйте псевдонимы при запросе представления в режиме FOR XML AUTO.  
  
 Например, в запросе SELECT приведение любого столбца к большому двоичному объекту (BLOB) делает этот столбец временной сущностью, которая теряет связанные с ней имя таблицы и имя столбца. Поэтому запросы в режиме AUTO будут формировать ошибки, так как они не будут знать, куда поместить этот объект в иерархии XML. Пример:  
  
```  
CREATE TABLE MyTable (Col1 int PRIMARY KEY, Col2 binary)  
INSERT INTO MyTable VALUES (1, 0x7);  
```  
  
 Следующий запрос вернет ошибку из-за приведения к типу BLOB:  
  
```  
SELECT Col1,  
CAST(Col2 as image) as Col2  
FROM MyTable  
FOR XML AUTO;  
```  
  
 Решение этой проблемы заключается в добавлении параметра BINARY BASE64 к предложению FOR XML. Если удалено приведение типов, запрос вернет ожидаемый результат:  
  
```  
SELECT Col1,  
CAST(Col2 as image) as Col2  
FROM MyTable  
FOR XML AUTO, BINARY BASE64;  
```  
  
 Результат:  
  
```  
<MyTable Col1="1" Col2="Bw==" />  
```  
  
## <a name="see-also"></a>См. также:  
 [Использование режима AUTO совместно с FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md)  
  
  

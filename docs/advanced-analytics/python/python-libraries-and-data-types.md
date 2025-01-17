---
title: Преобразование типов данных Python в SQL
description: Проверьте явные и неявные преобразования типов данных между Python и SQL Server в решениях для обработки и анализа данных и машинного обучения.
ms.prod: sql
ms.technology: machine-learning
ms.date: 12/10/2018
ms.topic: conceptual
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 690126098bbbd3ab26add51a0484f735120351de
ms.sourcegitcommit: 321497065ecd7ecde9bff378464db8da426e9e14
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68715786"
---
# <a name="data-type-mappings-between-python-and-sql-server"></a>Сопоставления типов данных между Python и SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

Для решений Python, которые выполняются в средстве интеграции Python в SQL Server Службы машинного обучения, проверьте список неподдерживаемых типов данных и преобразования типов данных, которые могут быть выполнены неявно при передаче данных между Python и SQL Server.

## <a name="python-version"></a>Версия Python

Подмножество функций RevoScaleR (rxLinMod, rxLogit, rxPredict, Рксдтрис, Рксбтрис, возможно, несколько других) предоставляется с помощью API Python с помощью нового пакета Python **revoscalepy**. Этот пакет можно использовать для работы с данными с помощью кадров данных Pandas, файлов Xdf-или запросов данных SQL.

Дополнительные сведения см. [в разделе Модуль revoscalepy в](ref-py-revoscalepy.md) справочнике по функциям SQL Server и [revoscalepy](https://docs.microsoft.com/r-server/python-reference/revoscalepy/revoscalepy-package).

Python поддерживает ограниченное число типов данных в сравнении с SQL Server. В результате при каждом использовании данных из SQL Server в скриптах Python данные могут быть неявно преобразованы в совместимый тип данных. Однако зачастую точное преобразование не может быть выполнено автоматически, и возвращается ошибка.

## <a name="python-and-sql-data-types"></a>Типы данных Python и SQL

В этой таблице перечислены предоставленные неявные преобразования. Другие типы данных не поддерживаются.

|SQLtype|Тип Python|
|-------|-----------|
|**bigint**|`numeric`|
|**binary**|`raw`|
|**bit**|`bool`|
|**char**|`str`|
|**float**|`float64`|
|**int**|`int32`|
|**nchar**|`str`|
|**nvarchar**|`str`|
|**nvarchar(max)**|`str`|
|**real**|`float32`|
|**smallint**|`int16`|
|**tinyint**|`uint8`|
|**varbinary**|`bytes`|
|**varbinary(max)**|`bytes`|
|**varchar(n)**|`str`|
|**varchar(max)**|`str`|

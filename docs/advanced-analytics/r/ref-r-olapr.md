---
title: Библиотека функций R для OLAP
description: Общие сведения о библиотеке функций OLAP в SQL Server 2016 служб R и SQL Server Службы машинного обучения с R.
ms.prod: sql
ms.technology: machine-learning
ms.date: 12/04/2018
ms.topic: conceptual
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 507bd04140880a3c15f1e72eed49c29ade56769c
ms.sourcegitcommit: 321497065ecd7ecde9bff378464db8da426e9e14
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68714998"
---
# <a name="olapr-r-library-in-sql-server"></a>OLAP (библиотека R в SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

**OLAP** — это библиотека Microsoft функций R, которая используется для запросов многомерных выражений к SQL Server ANALYSIS Services кубу OLAP. Функции не поддерживают все операции многомерных выражений, но можно создавать запросы, которые срезируют, кости, углубленную детализацию, свертку и сведение по измерениям. 

Этот пакет не предварительно загружен в сеанс R. Выполните следующую команду, чтобы загрузить библиотеку.

```R
library(olapR)
```

Эту библиотеку можно использовать для подключений к Analysis Services кубу OLAP во всех поддерживаемых версиях SQL Server. В настоящее время соединения с табличной моделью не поддерживаются.

## <a name="package-version"></a>Версия пакета

Текущая версия — 1.0.0 во всех продуктах Windows и загружаемых файлах, предоставляющих библиотеку.

## <a name="full-reference-documentation"></a>Полная справочная документация

Библиотека **OLAP** распространяется в нескольких продуктах Майкрософт, но использование этой библиотеки не отличается от того, где вы получаете библиотеку в SQL Server или другом продукте. Поскольку функции одинаковы, [Документация по отдельным функциям sqlrutils](https://docs.microsoft.com/machine-learning-server/r-reference/olapr/olapr) публикуется только в одном расположении в справочнике по [R](https://docs.microsoft.com/machine-learning-server/r-reference/introducing-r-server-r-package-reference) для Microsoft Machine Learning Server. Если существуют какие-либо поведения конкретного продукта, расхождения будут указаны на странице справки по функциям.

## <a name="availability-and-location"></a>Доступность и расположение

Этот пакет предоставляется в следующих продуктах, а также в нескольких образах виртуальных машин в Azure. Расположение пакета изменяется соответствующим образом.

Продукт | Location |
--------|----------|
SQL Server Службы машинного обучения (с интеграцией R) | C:\Program Files\Microsoft SQL Server\MSSQL14. MSSQLSERVER\R_SERVICES\library | 
Службы SQL Server 2016 R | C:\Program Files\Microsoft SQL Server\MSSQL13. MSSQLSERVER\R_SERVICES\library
Microsoft Machine Learning Server (R Server) | C:\Program Files\Microsoft\R_SERVER\library |
Microsoft R Client | C:\Program Филес\микрософт\р Client\R_SERVER\library |
Виртуальная машина для обработки и анализа данных (в Azure) | C:\Program Филес\микрософт\р Client\R_SERVER\library |
SQL Server виртуальная машина (в Azure) <sup>1</sup> | C:\Program Files\Microsoft SQL Server\MSSQL14. MSSQLSERVER\R_SERVICES\library |

<sup>1</sup> интеграция R необязательна в SQL Server. Библиотека OLAP будет установлена при добавлении компонента Машинное обучение или R во время настройки виртуальной машины.


## <a name="see-also"></a>См. также

[Создание запросов многомерных выражений с помощью средства OLAP](how-to-create-mdx-queries-using-olapr.md)

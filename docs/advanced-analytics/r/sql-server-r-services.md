---
title: Что такое SQL Server 2016 R Services?
titleSuffix: ''
description: Службы r — это функция в SQL Server 2016, которая дает возможность выполнять скрипты R с реляционными данными. Вы можете использовать пакеты и платформы с открытым исходным кодом, а также пакеты Microsoft R для прогнозной аналитики и машинного обучения. Скрипты выполняются в базе данных без перемещения данных за пределы SQL Server или по сети. В этой статье объясняются основы SQL Server R Services.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/12/2019
ms.topic: overview
author: dphansen
ms.author: davidph
monikerRange: =sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: 973c09be9cff6e66043b056e1a772ab8974cebb4
ms.sourcegitcommit: 12b7e3447ca2154ec2782fddcf207b903f82c2c0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/12/2019
ms.locfileid: "68957484"
---
# <a name="what-is-sql-server-2016-r-services"></a>Что такое SQL Server 2016 R Services?
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

Службы r — это функция в SQL Server 2016, которая дает возможность выполнять скрипты R с реляционными данными. Вы можете использовать пакеты и платформы с открытым исходным кодом, а также [пакеты Microsoft R](#packages) для прогнозной аналитики и машинного обучения. Скрипты выполняются в базе данных без перемещения данных за пределы SQL Server или по сети. В этой статье объясняются основы SQL Server R Services.

> [!Note]
> Службы R были переименованы в [службы машинного обучения](../what-is-sql-server-machine-learning.md) в SQL Server 2017 и более поздних версий и поддерживают Python и R.

## <a name="what-is-r-services"></a>Что такое службы R?

SQL Server R Services позволяет выполнять скрипты R в базе данных. С его помощью можно подготавливать и очищать данные, выполнять проектирование признаков, а также обучать, оценивать и развертывать модели машинного обучения в базе данных. Эта функция выполняет сценарии, в которых хранятся данные, и устраняет перемещение данных по сети на другой сервер.

Базовые распределения R включены в службы R Services. В дополнение к пакетам Microsoft [RevoScaleR](../r/ref-r-revoscaler.md), [MicrosoftML](../r/ref-r-microsoftml.md), [OLAP] можно использовать пакеты и платформы с открытым исходным кодом. /р/реф-р-олапр.МД) и [sqlrutils](../r/ref-r-sqlrutils.md) для r.

Службы r используют платформу расширяемости для выполнения скриптов R в SQL Server. Дополнительные сведения о том, как это работает:

+ [Инфраструктура расширяемости](../concepts/extensibility-framework.md)
+ [Расширение R](../concepts/extension-r.md)

## <a name="what-can-i-do-with-r-services"></a>Что можно сделать с помощью служб R Services?

Службы R можно использовать для создания и обучения моделей машинного обучения и модели глубокого обучения в SQL Server. Можно также развернуть существующие модели в службах R и использовать реляционные данные для прогнозов.

Примеры типов прогнозов, которые можно использовать SQL Server R Services для, включают:

|||
|-|-|
|Классификация и классификация|Автоматическое разделение отзывов клиентов на положительные и отрицательные категории|
|Регрессия/Прогнозирование непрерывных значений|Прогнозирование стоимости домов на основе размера и расположения|
|Обнаружение аномалий|Обнаружение мошеннических банковских транзакций |
|Рекомендации|Предложить продукты, которые могут быть приобретены через Интернет, на основе их предыдущих покупок|

### <a name="how-to-execute-r-scripts"></a>Выполнение скриптов R

Существует два способа выполнения сценариев R в службах R:

+ Наиболее распространенным способом является использование хранимой процедуры T-SQL [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

+ Можно также использовать предпочтительный клиент R и написать скрипты, которые принудительно отправляют выполнение (называемое *удаленным контекстом вычислений*) на удаленный SQL Server. Дополнительные сведения см. в статье [Настройка разработки клиента R для обработки и анализа данных](../r/set-up-a-data-science-client.md) .

<a name="packages"></a>

## <a name="r-packages"></a>Пакеты R

В дополнение к корпоративным пакетам Майкрософт можно использовать пакеты и платформы с открытым исходным кодом. Наиболее распространенные пакеты R с открытым кодом предварительно установлены в службах R. Также включены следующие пакеты R из корпорации Майкрософт:

| Пакет | Описание |
|-|-|
| [RevoScaleR](../r/ref-r-revoscaler.md) | Основной пакет для масштабируемых преобразований R. Data и манипулирования ими, Статистическая сводка, Визуализация и многие формы моделирования. Кроме того, функции в этом пакете автоматически распределяют рабочие нагрузки между доступными ядрами для параллельной обработки. |
| [MicrosoftML (R)](../r/ref-r-microsoftml.md) | Добавляет алгоритмы машинного обучения для создания пользовательских моделей для анализа текста, анализа изображений и анализа тональности. |
| [olapR](../r/ref-r-olapr.md) | Функции R, используемые для запросов многомерных выражений к SQL Server Analysis Services кубу OLAP. |
| [sqlrutils](../r/ref-r-sqlrutils.md) | Механизм использования скриптов R в хранимой процедуре T-SQL, регистрация этой хранимой процедуры в базе данных и выполнение хранимой процедуры из [среды разработки R](../r/set-up-a-data-science-client.md). |
| [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open (MRO) — это расширенное распространение R от корпорации Майкрософт. Это полная платформа с открытым кодом для статистического анализа и обработки и анализа данных. Он основан на и 100%, совместимых с R, и включает дополнительные возможности для повышения производительности и воспроизводимость. |

## <a name="how-do-i-get-started-with-rservices"></a>Разделы справки начать работу с Рсервицес?

1. [Установка служб R SQL Server 2016](../install/sql-r-services-windows-install.md)

1. Настройте средства разработки. Можно использовать:

    + [Azure Data Studio](../../azure-data-studio/what-is.md) или [SQL Server Management Studio (SSMS)](../../ssms/sql-server-management-studio-ssms.md) для использования T-SQL и хранимой процедуры [Sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) для выполнения скрипта R.
    + R на собственном ноутбуке разработки или рабочей станции для выполнения сценариев. Можно либо извлечь данные локально, либо отправить выполнение удаленно в SQL Server с помощью [RevoScaleR](../r/ref-r-revoscaler.md). Дополнительные сведения см. в статье [Настройка разработки клиента R для обработки и анализа данных](../r/set-up-a-data-science-client.md) .

1. Напишите первый скрипт R

    + Краткое руководство. [Выполнение скрипта "Hello World" в R](../tutorials/quickstart-r-run-using-tsql.md)
    + Краткое руководство. [Руководство по созданию модели прогнозирования на R](../tutorials/quickstart-r-create-predictive-model.md)
    + Учебник. [Использование R в T-SQL](../tutorials/sqldev-in-database-r-for-sql-developers.md): Просмотр данных, разработка признаков, обучение и развертывание моделей, создание прогнозов (серии из 5 частей)
    + Учебник. [Использование служб r в инструментах r](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md): Просмотр данных, создание графиков и графиков, выполнение технических характеристик, обучение и развертывание моделей, создание прогнозов (серии из шести частей)

## <a name="next-steps"></a>Следующие шаги

+ [Установка служб R SQL Server 2016](../install/sql-r-services-windows-install.md)
+ [Настройка клиента обработки и анализа данных для разработки на R](../r/set-up-a-data-science-client.md)
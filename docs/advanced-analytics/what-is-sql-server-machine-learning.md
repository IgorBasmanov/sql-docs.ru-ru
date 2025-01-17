---
title: Что такое Службы машинного обучения SQL Server (Python и R)?
titleSuffix: ''
description: Службы машинного обучения — это функция в SQL Server, которая дает возможность выполнять скрипты Python и R с реляционными данными. Вы можете использовать пакеты и платформы с открытым исходным кодом, а также пакеты Microsoft Python и R для прогнозной аналитики и машинного обучения. Скрипты выполняются в базе данных без перемещения данных за пределы SQL Server или по сети. В этой статье объясняются основы SQL Server Службы машинного обучения.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/07/2019
ms.topic: overview
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 4a1a9a3b0f712458466051ce2c67c0a725ef0a76
ms.sourcegitcommit: 12b7e3447ca2154ec2782fddcf207b903f82c2c0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/12/2019
ms.locfileid: "68957440"
---
# <a name="what-is-sql-server-machine-learning-services-python-and-r"></a>Что такое Службы машинного обучения SQL Server (Python и R)?
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

Службы машинного обучения — это функция в SQL Server, которая дает возможность выполнять скрипты Python и R с реляционными данными. Вы можете использовать пакеты и платформы с открытым исходным кодом, а также [пакеты Microsoft Python и R](#packages) для прогнозной аналитики и машинного обучения. Скрипты выполняются в базе данных без перемещения данных за пределы SQL Server или по сети. В этой статье объясняются основы SQL Server Службы машинного обучения.

В базе данных SQL Azure [службы машинного обучения](https://docs.microsoft.com/azure/sql-database/sql-database-machine-learning-services-overview) в настоящее время находится в общедоступной предварительной версии.

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
> [!NOTE]
> Сведения о запуске Java в SQL Server см. в [документации по расширениям языка](../language-extensions/language-extensions-overview.md).
::: moniker-end

## <a name="what-is-machine-learning-services"></a>Что такое Службы машинного обучения?

SQL Server Службы машинного обучения позволяет выполнять скрипты Python и R в базе данных. С его помощью можно подготавливать и очищать данные, выполнять проектирование признаков, а также обучать, оценивать и развертывать модели машинного обучения в базе данных. Эта функция выполняет сценарии, в которых хранятся данные, и устраняет перемещение данных по сети на другой сервер.

Базовые распределения Python и R включены в Службы машинного обучения. Вы можете использовать пакеты и платформы с открытым исходным кодом, такие как PyTorch, TensorFlow и scikit-учиться, в дополнение к пакетам Microsoft [revoscalepy](python/ref-py-revoscalepy.md) и [microsoftml](python/ref-py-microsoftml.md) для Python, а также [RevoScaleR](r/ref-r-revoscaler.md), [microsoftml](r/ref-r-microsoftml.md), [OLAP ](r/ref-r-olapr.md)и [Sqlrutils](r/ref-r-sqlrutils.md) для R.

Службы машинного обучения использует платформу расширяемости для выполнения скриптов Python и R в SQL Server. Дополнительные сведения о том, как это работает:

+ [Инфраструктура расширяемости](concepts/extensibility-framework.md)
+ [Расширение Python](concepts/extension-python.md)
+ [Расширение R](concepts/extension-r.md)

## <a name="what-can-i-do-with-machine-learning-services"></a>Что можно сделать с помощью Службы машинного обучения?

Службы машинного обучения можно использовать для создания и обучения моделей машинного обучения и глубокого обучения в SQL Server. Можно также развернуть существующие модели, чтобы Службы машинного обучения и использовать реляционные данные для прогнозов.

Примеры типов прогнозов, которые можно использовать SQL Server Службы машинного обучения для, включают:

|||
|-|-|
|Классификация и классификация|Автоматическое разделение отзывов клиентов на положительные и отрицательные категории|
|Регрессия/Прогнозирование непрерывных значений|Прогнозирование стоимости домов на основе размера и расположения|
|Обнаружение аномалий|Обнаружение мошеннических банковских транзакций |
|Рекомендации|Предложить продукты, которые могут быть приобретены через Интернет, на основе их предыдущих покупок|

### <a name="how-to-execute-python-and-r-scripts"></a>Выполнение скриптов Python и R

Существует два способа выполнения скриптов Python и R в Службы машинного обучения.

+ Наиболее распространенным способом является использование хранимой процедуры T-SQL [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md).

+ Вы также можете использовать предпочтительный клиент Python или R и написать скрипты, которые принудительно записывают выполнение (называемое *удаленным контекстом вычислений*) на удаленный SQL Server. Дополнительные сведения см. в статье Настройка клиента обработки и анализа данных для разработки на языке [Python](python/setup-python-client-tools-sql.md) и разработки на языке [R](r/set-up-a-data-science-client.md) .

<a name="packages"></a>

## <a name="python-and-r-packages"></a>Пакеты Python и R

В дополнение к корпоративным пакетам Майкрософт можно использовать пакеты и платформы с открытым исходным кодом. Наиболее распространенные пакеты Python и R с открытым кодом предварительно установлены в Службы машинного обучения. Также включены следующие пакеты Python и R из корпорации Майкрософт:

| Язык | Пакет | Описание |
|-|-|-|
| Python | [revoscalepy](python/ref-py-revoscalepy.md) | Основной пакет для масштабируемого Python. Преобразования и манипулирования данными, Статистическая сводка, Визуализация и многие виды моделирования. Кроме того, функции в этом пакете автоматически распределяют рабочие нагрузки между доступными ядрами для параллельной обработки. |
| Python | [microsoftml](python/ref-py-microsoftml.md) | Добавляет алгоритмы машинного обучения для создания пользовательских моделей для анализа текста, анализа изображений и анализа тональности. | 
| R | [RevoScaleR](r/ref-r-revoscaler.md) | Основной пакет для масштабируемых преобразований R. Data и манипулирования ими, Статистическая сводка, Визуализация и многие формы моделирования. Кроме того, функции в этом пакете автоматически распределяют рабочие нагрузки между доступными ядрами для параллельной обработки. |
| R | [MicrosoftML (R)](r/ref-r-microsoftml.md) | Добавляет алгоритмы машинного обучения для создания пользовательских моделей для анализа текста, анализа изображений и анализа тональности. |
| R | [olapR](r/ref-r-olapr.md) | Функции R, используемые для запросов многомерных выражений к SQL Server Analysis Services кубу OLAP. |
| R | [sqlrutils](r/ref-r-sqlrutils.md) | Механизм использования скриптов R в хранимой процедуре T-SQL, регистрация этой хранимой процедуры в базе данных и выполнение хранимой процедуры из [среды разработки R](r/set-up-a-data-science-client.md). |
| R | [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open (MRO) — это расширенное распространение R от корпорации Майкрософт. Это полная платформа с открытым кодом для статистического анализа и обработки и анализа данных. Он основан на и 100%, совместимых с R, и включает дополнительные возможности для повышения производительности и воспроизводимость. |

## <a name="how-do-i-get-started-with-machine-learning-services"></a>Разделы справки начать работу с Службы машинного обучения?

1. [Установка SQL Server Службы машинного обучения](install/sql-machine-learning-services-windows-install.md)

1. Настройте средства разработки. Можно использовать:

    + [Azure Data Studio](../azure-data-studio/what-is.md) или [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-ssms.md) для использования T-SQL и хранимой процедуры [Sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) для выполнения скрипта Python или R.
    + Python или R на собственном ноутбуке разработки или рабочей станции для выполнения сценариев. Можно либо извлечь данные локально, либо отправить выполнение удаленно в SQL Server с помощью [revoscalepy](python/ref-py-revoscalepy.md) и [RevoScaleR](r/ref-r-revoscaler.md). Дополнительные сведения см. в статье Настройка клиента обработки и анализа данных для разработки на языке [Python](python/setup-python-client-tools-sql.md) и разработки на языке [R](r/set-up-a-data-science-client.md) .

1. Напишите первый скрипт Python или R

    + Краткое руководство. Выполнение скрипта "Hello World" [в Python](tutorials/quickstart-python-run-using-t-sql.md) или [в R](tutorials/quickstart-r-run-using-tsql.md)
    + Краткое руководство. Создание прогнозной модели [на языке Python](tutorials/quickstart-python-train-score-in-tsql.md) или [в R](tutorials/quickstart-r-create-predictive-model.md)
    + Учебник. [Использование Python в T-SQL](tutorials/sqldev-in-database-python-for-sql-developers.md): Просмотр данных, разработка признаков, обучение и развертывание моделей, создание прогнозов (серии из 5 частей)
    + Учебник. [Использование R в T-SQL](tutorials/sqldev-in-database-r-for-sql-developers.md): Просмотр данных, разработка признаков, обучение и развертывание моделей, создание прогнозов (серии из 5 частей)
    + Учебник. [Использование службы машинного обучения в инструментах R](tutorials/walkthrough-data-science-end-to-end-walkthrough.md): Просмотр данных, создание графиков и графиков, выполнение технических характеристик, обучение и развертывание моделей, создание прогнозов (серии из шести частей)

## <a name="next-steps"></a>Следующие шаги

+ [Установка SQL Server Службы машинного обучения](install/sql-machine-learning-services-windows-install.md)
+ Настройка клиента обработки и анализа данных для [разработки](python/setup-python-client-tools-sql.md) на языке Python и разработки на языке [R](r/set-up-a-data-science-client.md)

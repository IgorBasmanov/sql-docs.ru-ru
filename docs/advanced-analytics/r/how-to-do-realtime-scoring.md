---
title: Создание прогнозов и прогнозов с помощью моделей машинного обучения
description: Используйте rxPredict или sp_rxPredict для оценки в реальном времени или ПРОГНОЗИРУЮЩИе T-SQL для собственной оценки прогнозов и прогнозирования в R и Python в SQL Server Машинное обучение.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/30/2018
ms.topic: conceptual
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: d01be0f7d7a18091b965ad73b9bf035558b34864
ms.sourcegitcommit: 321497065ecd7ecde9bff378464db8da426e9e14
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68715689"
---
# <a name="how-to-generate-forecasts-and-predictions-using-machine-learning-models-in-sql-server"></a>Как создавать прогнозы и прогнозы с помощью моделей машинного обучения в SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

Использование существующей модели для прогнозирования или прогнозирования результатов новых входных данных — это основная задача в машинном обучении. В этой статье перечислены подходы к формированию прогнозов в SQL Server. Между подходами используются внутренние методологии обработки для высокоскоростных прогнозов, скорость которых основана на инкрементном сокращении зависимостей времени выполнения. Меньшее число зависимостей означает более быстрые прогнозы.

При использовании внутренней инфраструктуры обработки (оценка в реальном времени или в машинном обходе) используются требования к библиотеке. Функции должны быть из библиотек Майкрософт. Код R или Python, вызывающий функции с открытым кодом или сторонними разработчиками, не поддерживается в C++ CLR или расширениях.

В следующей таблице перечислены платформы оценки для прогнозирования и прогнозирования. 

| Метод           | Интерфейс         | Требования к библиотеке | Скорость обработки |
|-----------------------|-------------------|----------------------|----------------------|
| Платформа расширяемости | [rxPredict (R)](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxpredict) <br/>[rx_predict (Python)](https://docs.microsoft.com/machine-learning-server/python-reference/revoscalepy/rx-predict) | Нет. Модели могут основываться на любой функции R или Python | Сотни миллисекунд. <br/>Загрузка среды выполнения имеет фиксированные затраты, усреднение от трех до 600 миллисекунд до оценки новых данных. |
| [Расширение CLR для оценки в реальном времени](../real-time-scoring.md) | [sp_rxPredict](https://docs.microsoft.com//sql/relational-databases/system-stored-procedures/sp-rxpredict-transact-sql) в сериализованной модели | CЕРВЕРНЫЙ RevoScaleR, MicrosoftML <br/>Python: revoscalepy, microsoftml | Десятки миллисекунд, в среднем. |
| [Собственное расширение C++ оценки](../sql-native-scoring.md) | [Прогнозирование функции T-SQL](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql) для сериализованной модели | CЕРВЕРНЫЙ RevoScaleR <br/>Python: revoscalepy | В среднем менее 20 миллисекунд. | 

Скорость обработки, а не вещества выходных данных, является отличительной особенностью. При условии, что одни и те же функции и входные данные не должны меняться в зависимости от используемого подхода.

Модель должна быть создана с помощью поддерживаемой функции, затем сериализована в поток необработанных байтов, сохраненный на диск, или сохранен в двоичном формате в базе данных. С помощью хранимой процедуры или T-SQL можно загружать и использовать двоичную модель без дополнительной нагрузки на язык R или Python, что приводит к ускорению выполнения при создании оценок прогнозов для новых входных данных.

Значение CLR и C++ Extensions близко к самому ядру СУБД. Собственный язык ядра СУБД — C++это, то есть расширения, написанные при C++ запуске с меньшим количеством зависимостей. Расширения CLR, напротив, зависят от .NET Core. 

Как и следовало ожидать, в этих средах выполнения влияют на поддержку платформы. Собственные расширения ядра СУБД работают в любом месте, где поддерживается реляционная база данных: Windows, Linux, Azure. Расширения CLR с требованием .NET Core в настоящее время являются только Windows.

## <a name="scoring-overview"></a>Обзор оценки

_Оценка_ состоит из двух этапов. Во-первых, вы указываете уже обученную модель для загрузки из таблицы. Во-вторых, передайте новые входные данные в функцию, чтобы создать прогнозные значения (или _оценки_). Входные данные часто являются запросом T-SQL, возвращающим либо табличные, либо отдельные строки. Можно вычислить одно значение столбца, представляющее вероятность, или вывести несколько значений, таких как доверительный интервал, ошибка или другие полезные дополнения к прогнозу.

Потратив шаг назад, общий процесс подготовки модели и последующего создания оценок можно обобщать следующим образом:

1. Создание модели с помощью поддерживаемого алгоритма. Поддержка зависит от выбранной методологии оценки.
2. Обучение модели.
3. Сериализует модель с помощью специального двоичного формата.
3. Сохраните модель в SQL Server. Обычно это означает сохранение сериализованной модели в SQL Server таблице.
4. Вызовите функцию или хранимую процедуру, указав модель и входные данные в качестве параметров.

Если входные данные содержат много строк данных, то обычно бывает быстрее вставлять прогнозные значения в таблицу в рамках процесса оценки. Создание одной оценки является более типичным в ситуации, когда вы получаете входные значения из формы или запроса пользователя и возвращаете оценку клиентскому приложению. Чтобы повысить производительность при формировании последовательных оценок, SQL Server может кэшировать модель, чтобы ее можно было загрузить в память.

## <a name="compare-methods"></a>Методы сравнения

Чтобы сохранить целостность основных процессов ядра СУБД, поддержка R и Python включается в двух архитектурах, которые изолируют языковую обработку из обработки RDBMS. Начиная с SQL Server 2016, корпорация Майкрософт добавила платформу расширяемости, которая позволяет выполнять скрипты R из T-SQL. В SQL Server 2017 добавлена интеграция с Python. 

Платформа расширяемости поддерживает любые операции, которые можно выполнять в R или Python, от простых функций до обучения сложных моделей машинного обучения. Однако архитектура с двумя процессами требует вызова внешнего процесса R или Python для каждого вызова, независимо от сложности операции. Когда Рабочая нагрузка загружает предварительно обученную модель из таблицы и сравнивает ее с данными, уже имеющимися в SQL Server, издержки на вызов внешних процессов добавляют задержку, которая может быть неприемлемой в определенных обстоятельствах. Например, для обнаружения мошенничества требуется высокая оценка.

Чтобы увеличить скорость оценки для таких сценариев, как обнаружение мошенничества, SQL Server добавлены встроенные библиотеки C++ оценки как и расширения CLR, устраняющие издержки в процессах запуска R и Python.

[**Оценка в режиме реального времени**](../real-time-scoring.md) была первым решением для повышения производительности. В ранних версиях SQL Server 2017 и более поздних обновлений для SQL Server 2016, оценка в реальном времени зависит от библиотек CLR, которые подпадают под обработку R и Python по функциям, управляемым корпорацией Майкрософт, в RevoScaleR, MicrosoftML (R), revoscalepy и microsoftml (Python). Библиотеки CLR вызываются с помощью хранимой процедуры **sp_rxPredict** для формирования оценок из любого поддерживаемого типа модели без вызова среды выполнения R или Python.

[**Собственная оценка**](../sql-native-scoring.md) — это функция SQL Server 2017, реализованная как собственная C++ Библиотека, но только для моделей RevoScaleR и revoscalepy. Это самый быстрый и более безопасный подход, но поддерживает меньший набор функций относительно других методологий.

## <a name="choose-a-scoring-method"></a>Выбор метода оценки

Требования к платформе часто определяют, какую методологию оценки следует использовать.

| Версия и платформа продукта | Метод |
|------------------------------|-------------|
| SQL Server 2017 в Windows, SQL Server 2017 Linux и база данных SQL Azure | **Собственная оценка** с помощью прогноза T-SQL |
| SQL Server 2017 (только Windows), SQL Server 2016 R Services с пакетом обновления 1 (SP1) или более поздней версии | **Оценка в реальном времени** с помощью\_хранимой процедуры rxPredict SP |

Мы рекомендуем использовать собственную оценку с помощью функции PREDICT. Для использования\_SP rxPredict необходимо включить интеграцию с SQLCLR. Прежде чем включать этот параметр, примите во внимание последствия безопасности.

## <a name="serialization-and-storage"></a>Сериализация и хранение

Чтобы использовать модель с любыми параметрами быстрой оценки, сохраните модель с помощью специального сериализованного формата, который был оптимизирован для повышения эффективности размера и оценки.

+ Вызовите [ркссериализемодел](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxserializemodel) , чтобы записать поддерживаемую модель в формат **RAW** .
+ Вызовите [рксунсериализемодел](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxserializemodel), чтобы воспроизводить модель для использования в другом коде R или для просмотра модели.

**Использование SQL**

Из кода SQL можно обучить модель с помощью [sp_execute_external_script](https://docs.microsoft.com//sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql)и непосредственно вставлять обученные модели в таблицу в столбце типа **varbinary (max)** . Простой пример см. в разделе [Создание модели предитиве в R](../tutorials/rtsql-create-a-predictive-model-r.md) .

**Использование языка R**

Из кода R вызовите функцию [рксвритеобжект](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxwriteobject) из пакета RevoScaleR, чтобы записать модель непосредственно в базу данных. Функция **рксвритеобжект ()** может извлекать объекты R из источника данных ODBC, например SQL Server, или записывать объекты в SQL Server. API моделируется после простого хранилища "ключ-значение".
  
При использовании этой функции сначала необходимо сериализовать модель с помощью [ркссериализемодел](https://docs.microsoft.com/r-server/r-reference/revoscaler/rxserializemodel) . Затем задайте для аргумента *сериализации* в **рксвритеобжект** значение false, чтобы избежать повторения шага сериализации.

Сериализация модели в двоичный формат полезна, но не требуется, если оценка прогнозов выполняется с помощью среды выполнения R и Python в платформе расширяемости. Можно сохранить модель в формате необработанного байта в файл, а затем считывать данные из файла в SQL Server. Этот параметр может быть полезен при перемещении или копировании моделей между средами.

## <a name="scoring-in-related-products"></a>Оценка в связанных продуктах

Если вы используете изолированный [сервер](r-server-standalone.md) или [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server), у вас есть другие варианты, кроме хранимых процедур и функций T-SQL для быстрого создания прогнозов. Как изолированный сервер, так и Machine Learning Server поддерживают концепцию *веб-службы* для развертывания кода. Вы можете объединить предварительно обученную модель R или Python в качестве веб-службы, которая вызывается во время выполнения для вычисления новых входных данных. Дополнительные сведения см. в следующих статьях:

+ [Что такое веб-службы в Machine Learning Server?](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services)
+ [Что такое эксплуатация?](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)
+ [Развертывание модели Python в качестве веб-службы с помощью azureml-Model-Management-SDK](https://docs.microsoft.com/machine-learning-server/operationalize/python/quickstart-deploy-python-web-service)
+ [Публикация блока кода R или модели в режиме реального времени в качестве новой веб-службы](https://docs.microsoft.com/machine-learning-server/r-reference/mrsdeploy/publishservice)
+ [пакет mrsdeploy для R](https://docs.microsoft.com/machine-learning-server/r-reference/mrsdeploy/mrsdeploy-package)


## <a name="see-also"></a>См. также

+ [ркссериализемодел](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxserializemodel)  
+ [рксреалтимескоринг](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/rxrealtimescoring)
+ [SP-rxPredict](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-rxpredict-transact-sql)
+ [ПРОГНОЗИРОВАНИЕ T-SQL](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql)
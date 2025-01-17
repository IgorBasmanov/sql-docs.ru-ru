---
title: Краткое руководство по "Hello World" базовому выполнению кода R в T-SQL
description: Краткое руководство по скрипту R в SQL Server. Изучите основы вызова скрипта R с помощью системной хранимой процедуры sp_execute_external_script в упражнении Hello-World.
ms.prod: sql
ms.technology: machine-learning
ms.date: 04/04/2019
ms.topic: quickstart
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: ac9537e3e13313e6ca0094a75b0e72c7c8ee80c2
ms.sourcegitcommit: 321497065ecd7ecde9bff378464db8da426e9e14
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68714757"
---
# <a name="quickstart-hello-world-r-script-in-sql-server"></a>Краткое руководство. Скрипт R "Hello World" в SQL Server 
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

В этом кратком руководстве вы узнаете основные понятия, запустив "Hello World" Скрипт R inT-SQL с введением в системную хранимую процедуру **sp_execute_external_script** . 

## <a name="prerequisites"></a>предварительные требования

Предыдущее краткое руководство, [Проверка наличия R в SQL Server](quickstart-r-verify.md), содержит сведения и ссылки для настройки среды R, необходимой для этого краткого руководства.

## <a name="basic-r-interaction"></a>Простое взаимодействие R

Код R можно запустить в базе данных SQL двумя способами:

+ Добавьте скрипт R в качестве аргумента системной хранимой процедуры [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql).
+ В [удаленном клиенте R](https://docs.microsoft.com/sql/advanced-analytics/r/set-up-a-data-science-client)подключитесь к базе данных SQL и выполните код, используя базу данных SQL в качестве контекста вычислений.

Следующее упражнение сосредоточено на первой модели взаимодействия: как передать код R в хранимую процедуру.

1. Запустите простой сценарий, чтобы увидеть, как выполняется сценарий R в базе данных SQL.

    ```sql
    EXECUTE sp_execute_external_script
    @language = N'R',
    @script = N'
    a <- 1
    b <- 2
    c <- a/b
    d <- a*b
    print(c(c, d))
    '
    ```

2. При условии, что все настроено правильно, вычисляется правильный результат, а функция `print` R возвращает результат в окно **сообщения** .

    **Результаты**

    ```text
    STDOUT message(s) from external script: 
    0.5 2
    ```

    Хотя получение сообщений **stdout** полезно при тестировании кода, чаще приходится возвращать результаты в табличном формате, чтобы его можно было использовать в приложении или записывать в таблицу. См [. Краткое руководство. Для получения дополнительных сведений обрабатывайте входные](rtsql-working-with-inputs-and-outputs.md) и выходные данные с помощью R в SQL Server.

Помните, что все, `@script` что находится внутри аргумента, должно быть допустимым кодом R.

## <a name="run-a-hello-world-script"></a>Выполнение скрипта Hello World

В следующем упражнении выполняются другие простые скрипты R.

```sql
EXEC sp_execute_external_script
  @language =N'R',
  @script=N'OutputDataSet<-InputDataSet',
  @input_data_1 =N'SELECT 1 AS hello'
  WITH RESULT SETS (([Hello World] int));
GO
```

К этим хранимым процедурам относятся следующие входные данные:

+ *@language* параметр определяет расширение языка для вызова, в данном случае R.
+ *@script* параметр определяет команды, передаваемые в среду выполнения R. Весь скрипт R должен быть заключен в этом аргументе в виде текста в Юникоде. Также можно добавить текст в переменную типа **nvarchar**, а затем вызвать ее.
+ *@input_data_1* данные, возвращаемые запросом, передаются в среду выполнения R, которая возвращает данные для SQL Server в виде кадра данных.
+ Предложение WITH RESULTSET SETS определяет схему возвращаемой таблицы данных для SQL Server, добавляя "Hello World" в качестве имени столбца, **int** для типа данных.

**Результаты**

| Всем привет |
|-------------|
| 1 |

## <a name="next-steps"></a>Следующие шаги

Теперь, когда вы выполнили пару простых скриптов R, внимательно рассмотрим структурирование входов и выходов.

> [!div class="nextstepaction"]
> [QuickStart Обработку входных и выходных данных](quickstart-r-inputs-and-outputs.md)

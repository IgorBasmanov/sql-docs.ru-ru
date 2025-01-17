---
title: Руководство по RevoScaleR функциям — SQL Server Машинное обучение
description: В этом руководстве описано, как вызывать функции RevoScaleR с помощью интеграции SQL Server Машинное обучение R.
ms.prod: sql
ms.technology: machine-learning
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 4db5debf4ba71f29a8870c8674a5422e9ffd334a
ms.sourcegitcommit: 321497065ecd7ecde9bff378464db8da426e9e14
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68714886"
---
# <a name="tutorial-use-revoscaler-r-functions-with-sql-server-data"></a>Учебник. Использование функций R RevoScaleR с данными SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

[RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler) — это пакет Microsoft R, предоставляющий распределенную и параллельную обработку для рабочих нагрузок обработки и анализа данных и машинного обучения. Для разработки R в SQL Server **RevoScaleR** является одним из основных встроенных пакетов, с помощью функций для создания объектов источников данных, установки контекста вычислений, управления пакетами и, что более важно: работа с данными закончена, из импорта в Визуализация и анализ. Алгоритмы Машинное обучение в SQL Server имеют зависимость от источников данных **RevoScaleR** . Учитывая важность **RevoScaleR**, важно знать, когда и как вызывать ее функции — важный навык. 

В этом учебнике из нескольких частей вы предоставили ряд функций **RevoScaleR** для задач, связанных с обработкой и анализом данных. В процессе вы узнаете, как создавать удаленный контекст вычислений, перемещать данные между локальными и удаленными контекстами вычислений и выполнять код на языке R на удаленном SQL Server. Кроме того, вы узнаете, как анализировать и отображать данные как локально, так и на удаленном сервере, а также как создавать и развертывать модели.

## <a name="prerequisites"></a>предварительные требования

+ [SQL Server службы машинного обучения](../install/sql-machine-learning-services-windows-install.md) с помощью функции R или [SQL Server R Services (в базе данных)](../install/sql-r-services-windows-install.md)
  
+ [Разрешения базы данных](../security/user-permission.md) и имя входа пользователя базы данных SQL Server

+ [Среда SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)

+ Интегрированная среда разработки, например RStudio или встроенное средство RGUI, входящее в R

Для переключения между локальными и удаленными контекстами вычислений требуются две системы. Локальная — это обычно Рабочая станция разработки с суффицент мощностью для рабочих нагрузок обработки и анализа данных. В этом случае в качестве удаленного используется SQL Server с включенным компонентом R. 

Переключение контекстов вычислений производится в соответствии с одинаковой версией **RevoScaleR** в локальных и удаленных системах. На локальной рабочей станции можно получить пакеты **RevoScaleR** и связанные с ними поставщики, установив Microsoft R Client.

Если необходимо разместить клиент и сервер на одном компьютере, установите второй набор библиотек Microsoft R для отправки сценария R с удаленного клиента. Не используйте библиотеки R, установленные в программных файлах экземпляра SQL Server. В частности, если используется один компьютер, то для поддержки операций клиента и сервера необходима библиотека **RevoScaleR** в обоих расположениях.

+ C:\Program Филес\микрософт\р Client\R_SERVER\library\RevoScaleR 
+ C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR

Инструкции по настройке клиента см. в разделе [Настройка клиента обработки и анализа данных для разработки на R](../r/set-up-a-data-science-client.md).


## <a name="r-development-tools"></a>Средства разработки R

Разработчики r обычно используют IDE для написания и отладки кода R. Вот несколько советов:

- **Инструменты R для Visual Studio** (RTVS) — это бесплатный подключаемый модуль, обеспечивающий технологию IntelliSense, отладку и поддержку Microsoft R. Его можно использовать с R Server и SQL Server Службы машинного обучения. Чтобы скачать эти средства, перейдите на страницу [Средства R для Visual Studio](https://www.visualstudio.com/vs/rtvs/).

- **RStudio** — одна из наиболее популярных сред для разработки на языке R. Дополнительные сведения см. в [https://www.rstudio.com/products/RStudio/](https://www.rstudio.com/products/RStudio/)разделе.

- Основные инструменты R (R. exe, RTerm. exe, RScripts. exe) также устанавливаются по умолчанию при установке R в SQL Server или клиенте R. Если вы не хотите устанавливать интегрированную среду разработки, можно использовать встроенные инструменты R для выполнения кода в этом руководстве.

Помните, что **RevoScaleR** требуется как на локальном, так и на удаленном компьютерах. Вы не можете пройти это руководство с помощью универсальной установки RStudio или другой среды, в которой отсутствуют библиотеки Microsoft R. Дополнительные сведения см. в разделе [Настройка клиента обработки и анализа данных](../r/set-up-a-data-science-client.md).

## <a name="summary-of-tasks"></a>Сводка задач

+ Данные изначально получаются из CSV- или XDF-файлов. Данные импортируются в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с помощью функций в пакете **RevoScaleR** .
+ Обучение модели и оценка выполняется с помощью [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] контекста вычислений. 
+ Используйте функции **RevoScaleR** для создания новых [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] таблиц, чтобы сохранить результаты оценки.
+ Создавайте графики как на сервере, так и в локальном контексте вычислений.
+ Обучить модель данных в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] базе данных, запуская R [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в экземпляре.
+ Извлеките подмножество данных и сохраните их в виде файла Xdf-для повторного использования при анализе на локальной рабочей станции.
+ Получите новые данные для оценки, открыв соединение ODBC с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] базой данных. Оценка выполняется на локальной рабочей станции.
+ Создайте пользовательскую функцию R и запустите ее в контексте вычислений сервера, чтобы выполнить моделирование.

## <a name="next-steps"></a>Следующие шаги

> [!div class="nextstepaction"]
> [Занятие 1. Создание базы данных и разрешений](deepdive-work-with-sql-server-data-using-r.md)
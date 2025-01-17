---
title: Выполняющаяся в памяти OLTP (оптимизация в памяти) | Документация Майкрософт
ms.custom: ''
ms.date: 08/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
helpviewer_keywords:
- In-Memory OLTP
- memory-optimized tables
ms.assetid: e1d03d74-2572-4a55-afd6-7edf0bc28bdb
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9ab822203fa55e32a3b310f6c5a3ecfe3269d8ed
ms.sourcegitcommit: 321497065ecd7ecde9bff378464db8da426e9e14
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68715911"
---
# <a name="in-memory-oltp-and-memory-optimization"></a>Выполняющаяся в памяти OLTP (оптимизация в памяти)

[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] может значительно улучшить производительность обработки транзакций, приема и загрузки данных, а также оптимизировать сценарии с использованием временных данных.  Базовый код и сведения, необходимые для быстрого тестирования вашей собственной оптимизированной для памяти таблицы и скомпилированной в собственном коде хранимой процедуры, см. в статье:
 -  [Краткое руководство 1. Технологии выполнения OLTP в памяти для повышения производительности службы Transact-SQL](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md).  
 
На YouTube доступен [**17-минутный видеоролик**](#anchorname-17minute-video), поясняющий выполняемую в памяти OLTP в SQL Server и демонстрирующий преимущества с точки зрения производительности.

Дополнительные сведения о выполняющейся в памяти OLTP и обзор сценариев, в которых эта технология может значительно улучшать производительность:

- [Общие сведения и сценарии использования](../../relational-databases/in-memory-oltp/overview-and-usage-scenarios.md)
 
 Обратите внимание на то, что [!INCLUDE[hek_2](../../includes/hek-2-md.md)] представляет собой технологию [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] для повышения производительности при обработке транзакций. Сведения о технологии [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , повышающей производительность при обработке отчетов и аналитических запросов, см. в статье [Руководство по индексам columnstore](../../relational-databases/indexes/columnstore-indexes-overview.md).
  
 В [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] и [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], а также в [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] для выполняющейся в памяти OLTP было внесено несколько улучшений. Контактная зона Transact-SQL увеличена для облегчения переноса приложений баз данных. Добавлена поддержка выполнения операций ALTER для оптимизированных для памяти таблиц и скомпилированных в собственном коде хранимых процедур, чтобы упростить обслуживание приложений.
  
> [!NOTE]  
>  **Попробуйте продукт**  
>   
>  Выполняющаяся в памяти OLTP доступна для эластичных пулов и баз данных SQL Azure уровня "Премиум" и "Критически важный для бизнеса". Сведения о том, как приступить к работе с выполняющейся в памяти OLTP и индексами columnstore в Базе данных SQL Azure, см. в статье [Приступая к работе с In-Memory (в режиме предварительной версии) в базе данных SQL](https://azure.microsoft.com/documentation/articles/sql-database-in-memory/).  
  

## <a name="in-this-section"></a>В этом разделе  
 Здесь приведен список статей с описанием.  
  
|Раздел|Описание|  
|-----------|-----------------|  
|[Краткое руководство 1. Технологии выполнения OLTP в памяти для повышения производительности службы Transact-SQL](../../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md)|Подробные сведения о выполняющейся в памяти OLTP.|
|[Общие сведения и сценарии использования](../../relational-databases/in-memory-oltp/overview-and-usage-scenarios.md)|Общие сведения о выполняющейся в памяти OLTP и сценарии, в которых эта технология может улучшать производительность.|
|[Требования для использования таблиц, оптимизированных для памяти](../../relational-databases/in-memory-oltp/requirements-for-using-memory-optimized-tables.md)|Перечисляются требования к программному обеспечению и оборудованию, а также рекомендации по использованию оптимизированных для памяти таблиц.|  
|[Примеры кода In-Memory OLTP](../../relational-databases/in-memory-oltp/in-memory-oltp-code-samples.md)|Содержит образцы кода, которые показывают, как создавать и использовать оптимизированные для памяти таблицы.|  
|[Таблицы, оптимизированные для памяти](../../relational-databases/in-memory-oltp/memory-optimized-tables.md)|Дает вводное описание таблиц, оптимизированных для памяти.|  
|[Основные сведения о табличных переменных, оптимизированных для памяти](https://msdn.microsoft.com/library/bd102e95-53e2-4da6-9b8b-0e4f02d286d3)|Пример кода, показывающий, как использовать переменную оптимизированной для памяти таблицы вместо традиционной табличной переменной для уменьшения использования базы данных tempdb.|  
|[Индексы для оптимизированных для памяти таблиц](https://msdn.microsoft.com/library/86805eeb-6972-45d8-8369-16ededc535c7)|Содержит базовое описание индексов, оптимизированных для памяти.|  
|[Скомпилированные в собственном коде хранимые процедуры](../../relational-databases/in-memory-oltp/natively-compiled-stored-procedures.md)|Содержит базовое описание хранимых процедур, скомпилированных в собственном коде.|  
|[Управление памятью для компонента "Выполняющаяся в памяти OLTP"](https://msdn.microsoft.com/library/d82f21fa-6be1-4723-a72e-f2526fafd1b6)|Основные сведения об управлении памятью системы.|  
|[Создание и управление хранилищем для оптимизированных для памяти объектов](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)|Описывает файлы данных и разностные файлы, в которых хранятся сведения о транзакциях в оптимизированных для памяти таблицах.|  
|[Резервное копирование и восстановление оптимизированных для памяти таблиц](https://msdn.microsoft.com/library/3f083347-0fbb-4b19-a6fb-1818d545e281)|Описывает резервное копирование, восстановление и восстановление оптимизированных для памяти таблиц.|  
|[Поддержка Transact-SQL для выполняющейся в памяти OLTP](../../relational-databases/in-memory-oltp/transact-sql-support-for-in-memory-oltp.md)|Описывает поддержку службой [!INCLUDE[tsql](../../includes/tsql-md.md)] службы [!INCLUDE[hek_2](../../includes/hek-2-md.md)].|  
|[Поддержка высокого уровня доступности в базах данных OLTP в памяти](../../relational-databases/in-memory-oltp/high-availability-support-for-in-memory-oltp-databases.md)|Описывает группы доступности и отказоустойчивые кластеры в службе [!INCLUDE[hek_2](../../includes/hek-2-md.md)].|  
|[Поддержка SQL Server для In-Memory OLTP](../../relational-databases/in-memory-oltp/sql-server-support-for-in-memory-oltp.md)|Описание нового и обновленного синтаксиса и функций, поддерживающих оптимизированные для памяти таблицы.|  
|[Миграция в выполняющуюся в памяти OLTP](../../relational-databases/in-memory-oltp/migrating-to-in-memory-oltp.md)|Рассматриваются способы миграции дисковых таблиц в оптимизированные для памяти таблицы.|  
| &nbsp; | &nbsp; |

## <a name="links-to-other-websites"></a>Ссылки на другие веб-сайты

В этом разделе приводятся ссылки на другие веб-сайты, содержащие сведения о выполняющейся в памяти OLTP в SQL Server.

- [**Видеоролик**, поясняющий выполняемую в памяти OLTP и демонстрирующий преимущества с точки зрения производительности](#anchorname-17minute-video)

- [In-Memory OLTP Performance Demo v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)

-   [Техническая документация по внутренней структуре выполняющейся в памяти OLTP SQL Server](https://msdn.microsoft.com/library/mt764316.aspx)  

-   [Сравнение функций SQL Server In-Memory OLTP и columnstore](https://download.microsoft.com/download/D/0/0/D0075580-6D72-403D-8B4D-C3BD88D58CE4/SQL_Server_2016_In_Memory_OLTP_and_Columnstore_Comparison_White_Paper.pdf)

-   Новые возможности In-Memory OLTP в SQL Server 2016 [Часть 1](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2015/11/12/in-memory-oltp-whats-new-in-sql2016-ctp3/) и [Часть 2](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/03/25/whats-new-for-in-memory-oltp-in-sql-server-2016-since-ctp3/)
  
-   [Выполняемая в памяти OLTP — общие замечания по шаблонам рабочей нагрузки и миграции](https://msdn.microsoft.com/library/dn673538.aspx)  
  
-   [Блог о выполняющейся в памяти OLTP](https://go.microsoft.com/fwlink/?LinkId=311696)  

## <a name="anchorname-17minute-video"></a>17-минутное видео, с указателем

- _Название видео:_ **Выполняющаяся в памяти OLTP в SQL Server 2016**
- _Дата публикации:_ 2019-03-10
- _Продолжительность:_ 17:32
- _Размещено:_ Хос де Бруижн, старший руководитель программы по SQL Server

### <a name="demo-can-be-downloaded"></a>Демонстрацию можно скачать

На отметке времени 08:09 демонстрация в видео выполняется дважды. Вы можете скачать готовую к запуску демонстрацию производительности, которая используется в видео, по следующей ссылке:

- [In-Memory OLTP Performance Demo v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)

Ниже приведена общая последовательность действий, которые показаны в видео.

1. Сначала демонстрация запускается с обычной таблицей.
2. Далее показана оптимизированная для памяти версия таблицы, которая создается и заполняется несколькими щелчками мыши в SQL Server Management Studio (SSMS.exe).
3. После этого демонстрация выполняется повторно с использованием таблицы, оптимизированной для памяти. Фиксируется существенное улучшение скорости.

### <a name="index-to-each-section-in-the-video"></a>Указатель разделов видео

| Ссылка на временную метку | Заголовок раздела |
| :------------- | :------------ |
| A.&nbsp; [00:00](https://www.youtube.com/watch?v=l5l5eophmK4&t=0) | Начало. |
| <br/>B.&nbsp; [00:56](https://www.youtube.com/watch?v=l5l5eophmK4&t=56) | <br/>Почему выполняющаяся в памяти OLTP имеет значение для клиентов. |
| &nbsp; &nbsp; [01:03](https://www.youtube.com/watch?v=l5l5eophmK4&t=63) | Современное оборудование требует современной архитектуры системы баз данных. |
| &nbsp; &nbsp; [02:10](https://www.youtube.com/watch?v=l5l5eophmK4&t=130) | Взрывной рост объема создаваемых данных. Операции должны выполняться мгновенно (с низкой задержкой). |
| &nbsp; &nbsp; [03:19](https://www.youtube.com/watch?v=l5l5eophmK4&t=199) | Сокращение совокупной стоимости владения: больше возможностей при использовании существующих ресурсов. |
| <br/>C.&nbsp; [03:33](https://www.youtube.com/watch?v=l5l5eophmK4&t=213) | <br/>Что такое выполняющаяся в памяти OLTP.<br/>Оптимизация производительности с использованием оптимизированной для памяти технологии. |
| &nbsp; &nbsp; [05:03](https://www.youtube.com/watch?v=l5l5eophmK4&t=303) | Повышение скорости обработки транзакций до 30 раз. |
| &nbsp; &nbsp; [05:22](https://www.youtube.com/watch?v=l5l5eophmK4&t=322) | Полная надежность: данные сохраняются в случае сбоев сервера. |
| &nbsp; &nbsp; [06:15](https://www.youtube.com/watch?v=l5l5eophmK4&t=375) | Полная интеграция в SQL Server. Не требуется изучать новые языки и инструменты. |
| &nbsp; &nbsp; [07:22](https://www.youtube.com/watch?v=l5l5eophmK4&t=442) | Впервые выпущена в SQL Server 2014, существенно улучшена в 2016. |
| &nbsp; &nbsp; [07:58](https://www.youtube.com/watch?v=l5l5eophmK4&t=558) | Доступна также в базе данных SQL Microsoft Azure (в облаке). |
| <br/>D.&nbsp; [08:09](https://www.youtube.com/watch?v=l5l5eophmK4&t=489) | <br/>Демонстрация производительности.<br/> Запуск демонстрации с обычной таблицей. |
| &nbsp; &nbsp; [09:11](https://www.youtube.com/watch?v=l5l5eophmK4&t=551) | Контекстное меню SSMS: **Отчеты** &gt; **об анализе производительности транзакции** |
| &nbsp; &nbsp; [10:38](https://www.youtube.com/watch?v=l5l5eophmK4&t=638) | Контекстное меню SSMS: **Помощник по оптимизации памяти**<br/> &nbsp; &nbsp; Фактическое создание оптимизированной для памяти таблицы из обычной таблицы и перенос данных. |
| &nbsp; &nbsp; [11:28](https://www.youtube.com/watch?v=l5l5eophmK4&t=688) | Повторный запуск демонстрации, улучшение скорости в 45 раз. |
| <br/>E.&nbsp; [12:17](https://www.youtube.com/watch?v=l5l5eophmK4&t=737) | <br/>Более простая в использовании выполняющаяся в памяти OLTP в SQL Server 2016 (по сравнению с 2014). |
| &nbsp; &nbsp; [12:43](https://www.youtube.com/watch?v=l5l5eophmK4&t=763) | Упрощенный анализ для обеспечения миграции приложений. |
| &nbsp; &nbsp; [13:03](https://www.youtube.com/watch?v=l5l5eophmK4&t=783) | Упрощение миграции приложений с помощью расширенной поддержки языка Transact-SQL (например, с использованием внешних ключей и триггеров). |
| &nbsp; &nbsp; [13:56](https://www.youtube.com/watch?v=l5l5eophmK4&t=836) | Улучшение управляемости.<br/> &nbsp; &nbsp; Например, изменение схемы и индексов, автоматическое обновление статистики. |
| <br/>F.&nbsp; [14:46](https://www.youtube.com/watch?v=l5l5eophmK4&t=886) | <br/>Улучшенная масштабируемость. |
| &nbsp; &nbsp; [15:12](https://www.youtube.com/watch?v=l5l5eophmK4&t=912) | Большие оптимизированные для памяти таблицы (до 2 ТБ на базу данных). |
| &nbsp; &nbsp; [15:34](https://www.youtube.com/watch?v=l5l5eophmK4&t=934) | Еще более эффективное масштабирование. |
| &nbsp; &nbsp; [16:41](https://www.youtube.com/watch?v=l5l5eophmK4&t=1001) | Больше возможностей при использовании существующих ресурсов. |
| <br/>G.&nbsp; [16:53](https://www.youtube.com/watch?v=l5l5eophmK4&t=1013) | <br/>Заключительные комментарии. (Заканчивается на 17:32.) |
| &nbsp; | &nbsp; |

## <a name="see-also"></a>См. также раздел  
 [Функции базы данных](../../relational-databases/database-features.md)  
  
  

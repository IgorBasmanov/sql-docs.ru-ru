---
title: Производительность интеграции со средой CLR | Документация Майкрософт
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- common language runtime [SQL Server], performance
- common language runtime [SQL Server], compilation process
- performance [CLR integration]
ms.assetid: 7ce2dfc0-4b1f-4dcb-a979-2c4f95b4cb15
author: rothja
ms.author: jroth
ms.openlocfilehash: a4cd3b8f186f1ade85f4ed4533b0549bcd449a69
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68134953"
---
# <a name="clr-integration-architecture----performance"></a>Архитектура интеграции со средой CLR — производительность
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  В этом разделе рассматриваются некоторые варианты проектирования, которые повышают производительность [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] интеграция с [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework среда CLR (CLR).  
  
## <a name="the-compilation-process"></a>Процесс компиляции  
 Во время компиляции выражений SQL, когда обнаруживается ссылка на управляемую процедуру [!INCLUDE[msCoName](../../includes/msconame-md.md)] создается заглушка на промежуточном языке (MSIL). Заглушка содержит программный код для упаковки параметров процедуры из [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в среду CLR, вызова функции и возвращения результата. Этот связующий код основан на типе параметра и его направлении (входной, выходной, передача по ссылке).  
  
 Связующий код позволяет проводить оптимизацию для конкретного типа и гарантирует эффективное обеспечение семантики [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] таких свойств, как допустимость значений NULL, ограничивающие аспекты, передача параметров по значению и стандартная обработка исключений. Создавая код для конкретных типов аргументов, можно избежать приведения типов и нагрузки по созданию объектов-оболочек (этот процесс называют также «упаковкой») при вызове, пересекающем границы процессов.  
  
 Затем созданная заглушка компилируется в собственный код и оптимизируется для конкретной аппаратной архитектуры, на которой выполняется [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], с использованием служб JIT-компиляции среды CLR. JIT-службы вызываются на уровне методов и позволяют управляющему окружению [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] создавать единый объект компиляции между СУБД [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и выполнением в среде CLR. После компиляции заглушки результирующий указатель на функцию становится реализацией этой функции времени выполнения. Такой подход к созданию кода гарантирует отсутствие лишних расходов по вызову функций, связанных с отражением или доступом к метаданным во время выполнения.  
  
### <a name="fast-transitions-between-sql-server-and-clr"></a>Быстрые переходы между СУБД SQL Server и средой CLR  
 Процесс компиляции возвращает указатель на функцию, с помощью которого можно вызвать функцию во время выполнения из машинного кода. Для определяемых пользователем скалярных функций этот вызов функции происходит по строкам. Для максимального снижения стоимости перехода между [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и средой CLR все инструкции, содержащие вызов управляемого кода, проделывают начальный шаг для выявления целевого домена приложения. Этот шаг идентификации снижает стоимость перехода для каждой строки.  
  
## <a name="performance-considerations"></a>Вопросы производительности  
 Далее проводится краткое перечисление факторов производительности, специфичных для интеграции со средой CLR и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Более подробные сведения можно найти в "[с помощью интеграции со средой CLR в SQL Server 2005](https://go.microsoft.com/fwlink/?LinkId=50332)" на сайте MSDN. Общие сведения о производительности управляемого кода можно найти в "[повышение производительности приложений .NET и масштабируемости](https://go.microsoft.com/fwlink/?LinkId=50333)" на сайте MSDN.  
  
### <a name="user-defined-functions"></a>Определяемые пользователем функции  
 Функции CLR используют ускоренный путь вызова по сравнению с определяемыми пользователем функциями в [!INCLUDE[tsql](../../includes/tsql-md.md)]. Кроме того, производительность управляемого кода заметно выше, чем у [!INCLUDE[tsql](../../includes/tsql-md.md)], в том, что касается процедурного кода, вычислений и операций со строками. Функции CLR, которые требуют большого объема вычислений и не требуют доступа к данным, лучше писать в управляемом коде. Однако функции [!INCLUDE[tsql](../../includes/tsql-md.md)] осуществляют доступ к данным более эффективно, чем функции интеграции со средой CLR.  
  
### <a name="user-defined-aggregates"></a>Определяемые пользователем статистические функции  
 Управляемый код может значительно опережать по производительности статическую обработку на основе курсора. Управляемый код обычно работает несколько медленнее, чем встроенные агрегатные функции [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если существует собственная встроенная агрегатная функция SQL Server, рекомендуется использовать ее. В случаях, когда нужные статистические вычисления не поддерживаются встроенными функциями, из соображений производительности можно использовать созданную пользователем статистическую функцию среды CLR, реализованную на основе курсора.  
  
### <a name="streaming-table-valued-functions"></a>Функции потока с табличным значением  
 Часто бывает нужно, чтобы в результате вызова функции приложение вернуло таблицу. Например, в качестве части операции импорта приложение читает табличные данные из файла; нужно преобразовать их из формата величин с разделителями-запятыми в реляционное представление. Обычно это достигается с помощью материализации и заполнения таблицы результатов до ее использования вызывающим объектом. Интеграция CLR в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] использует новый механизм расширяемости — функции потока с табличным значением (streaming table-valued function, STVF). Управляемые функции потока, возвращающие табличное значение, по производительности опережают реализации на основе расширенных хранимых процедур.  
  
 Потоковые, функции, возвращающие **IEnumerable** интерфейс. **IEnumerable** есть методы для навигации результирующего набора, возвращенного функцией STVF. При вызове Возвращающей возвращенного **IEnumerable** подключен напрямую к плану запроса. Вызовы план запроса **IEnumerable** методы при необходимости для получения строк. Такая модель итерации позволяет провести немедленную обработку результатов сразу после получения первой строки, не ожидая заполнения всей таблицы. Она также существенно снижает затраты памяти на вызов функции.  
  
### <a name="arrays-vs-cursors"></a>Сравнение массивов и. Курсоры  
 Если курсорам [!INCLUDE[tsql](../../includes/tsql-md.md)] нужно перемещаться по данным, которые проще реализовать как массив, использование управляемого кода принесет существенный выигрыш в производительности.  
  
### <a name="string-data"></a>Строковые данные  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] символьные данные, такие как **varchar**, может быть типа SqlString или SqlChars в управляемых функций. Переменные типа SqlString создают в памяти экземпляр всего значения целиком. Переменные типа SqlChars обеспечивают потоковый интерфейс, который позволяет добиться более высокой производительности и масштабируемости, так как не создает в памяти экземпляра всего значения сразу. Это особенно важно для типов больших объектов (LOB). Кроме того, XML-данных может осуществляться через интерфейс потоковой передачи, возвращенный **SqlXml.CreateReader()** .  
  
### <a name="clr-vs-extended-stored-procedures"></a>Сравнение CLR и. Расширенные хранимые процедуры  
 API-интерфейсы Microsoft.SqlServer.Server, позволяющие управляемым процедурам отсылать результирующие наборы обратно клиенту, имеют более высокую производительность, чем API-интерфейсы служб Open Data Services (ODS), используемые расширенными хранимыми процедурами. Кроме того, такие как типы данных поддержки API-интерфейсы System.Data.SqlServer **xml**, **varchar(max)** , **nvarchar(max)** , и **varbinary(max)** , включенная в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], тогда как в ODS API-не были расширены для поддержки новых типов данных.  
  
 При использовании управляемого кода [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] управляет использованием ресурсов памяти, потоков и синхронизации. Это происходит потому, что управляемые API-интерфейсы, предоставляющие доступ к этим ресурсам, реализованы поверх диспетчера ресурсов [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], напротив, не может управлять использованием ресурсов расширенных хранимых процедур. Например, если расширенная хранимая процедура потребляет слишком много ресурсов процессора или памяти, в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] нет никаких средств, чтобы это обнаружить или управлять этим. Напротив, при использовании управляемого кода [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] может увидеть, что конкретный поток давно не отдает управление, и заставить задачу отдать управление, чтобы можно было планировать выполнение других задач. Поэтому использование управляемого кода позволяет лучше масштабировать выполнение и оптимизировать использование системных ресурсов.  
  
 Управляемый код может вызывать дополнительные расходы на поддержку среды выполнения и проверки безопасности. Это происходит, например, при выполнении внутри [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], когда требуются многочисленные переходы от управляемого кода к собственному и обратно (потому что [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] вынужден проводить дополнительное обслуживание настроек конкретных потоков при переходе к машинному коду и обратно). Следовательно, расширенные хранимые процедуры могут выполняться значительно быстрее управляемого кода внутри [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в случаях, когда требуются многочисленные переходы от управляемого к машинному коду и обратно.  
  
> [!NOTE]  
>  Не рекомендуется разрабатывать новые расширенные хранимые процедуры, поскольку эта функциональная возможность устарела.  
  
### <a name="native-serialization-for-user-defined-types"></a>Собственная сериализация для определяемых пользователем типов  
 Определяемые пользователем типы (UDT) представляют собой механизм расширения скалярной системы типов. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] реализован формат сериализации для определяемого пользователем типа — **Format.Native**. Во время компиляции исследуется структура типа, а затем создается код MSIL, настраиваемый для данного конкретного определения типа.  
  
 Собственная сериализация используется в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] по умолчанию. Сериализация, определяемая пользователем, вызывает для сериализации метод, указанный автором типа. **Format.Native** следует использовать по возможности для повышения производительности.  
  
### <a name="normalization-of-comparable-udts"></a>Нормализация сравнимых определяемых пользователем типов  
 Операции отношения, например сортировка и сравнение определяемых пользователем типов, работают непосредственно с двоичным представлением значения. Для этого на диске хранится нормализованное (двоичное, упорядоченное) представление состояния определяемого пользователем типа.  
  
 Нормализация имеет два преимущества: ее использование значительно «удешевляет» операцию сравнения, так как избавляет от необходимости создания экземпляра типа и вызова метода; кроме того, нормализация создает двоичное представление определяемого пользователем типа, что позволяет строить гистограммы, индексы и гистограммы для значений этого типа. Поэтому производительность операций с нормализованными определяемыми пользователем типами почти такая же, как у операций, не требующих вызова методов, над встроенными типами.  
  
### <a name="scalable-memory-usage"></a>Масштабируемое использование памяти  
 Чтобы управляемая сборка мусора в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] имела высокую производительность и хорошо масштабировалась, следует избегать выделения памяти одним большим блоком. Выделенные области памяти размером больше 88 КБ помещаются в кучу для больших объектов, для которой сборка мусора работает гораздо медленнее и хуже масштабируется, чем для небольших областей памяти. Например, если нужно выделить память для большого многомерного массива, лучше выделить память под массив массивов (разреженный массив).  
  
## <a name="see-also"></a>См. также  
 [Определяемые пользователем типы в CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)  
  
  

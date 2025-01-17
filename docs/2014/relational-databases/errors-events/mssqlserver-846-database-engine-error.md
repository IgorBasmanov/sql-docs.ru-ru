---
title: MSSQLSERVER_846 | Документация Майкрософт
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- 846 (Database Engine error)
ms.assetid: ccf367eb-06b0-42b8-b4d6-2b88f4a502d3
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 840898cb41274a1871e5d087e2be1d2ab9e39e23
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/15/2019
ms.locfileid: "62913191"
---
# <a name="mssqlserver846"></a>MSSQLSERVER_846
    
## <a name="details"></a>Сведения  
  
|||  
|-|-|  
|Название продукта|SQL Server|  
|Идентификатор события|846|  
|Источник события|MSSQLSERVER|  
|Компонент|SQLEngine|  
|Символическое имя|Н/Д|  
|Текст сообщения|Истекло время ожидания кратковременной блокировки буфера — тип %d, базовая точка %p, страница %d:%d, stat %#x, идентификатор базы данных: %d, идентификатор единицы распределения: %I64d%ls, задача 0x%p: %d, время ожидания %d, флаги 0x%I64x, задача-владелец 0x%p. Ожидание прекращено.|  
  
## <a name="explanation"></a>Объяснение  
 Возможно, компьютер не отвечает ("завис"), истекло время ожидания, либо происходит какой-то сбой в тот момент, когда [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] записывает ошибок кратковременной блокировки буфера в журнал ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Если в сообщении в поле состояния указано значение 0x04, то [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ожидает операцию ввода-вывода. Кроме того, может быть получено сообщение [MSSQLSERVER_833](mssqlserver-833-database-engine-error.md) в журнале ошибок [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Если в сообщении в поле состояния указано значение stat 0x04 off, это означает серьезное состязание на странице. Если объектом является страница данных, это может быть связано с неэффективной разработкой кода. Если страница не является страницей данных, то причиной ошибки могут быть узкие места серверов, например недостаток ресурсов оборудования.  
  
## <a name="user-action"></a>Действие пользователя  
 Для решения проблемы в зависимости от среды выполнение одного или нескольких следующих шагов может сократить количество сообщений об ошибках или исключить их.  
  
-   Определите наличие узких мест оборудования. При необходимости обновите оборудование, чтобы оно поддерживало требования среды к конфигурации, запросам и нагрузке. Дополнительные сведения об узких местах см. в статье [Выявление узких мест](../performance/identify-bottlenecks.md).  
  
-   Проверьте все зарегистрированные в журнале ошибки и запустите программу диагностики, предоставляемую поставщиком оборудования.  
  
-   Убедитесь, что жесткие диски не сжаты. Хранение данных или файлов журнала на сжатых дисках не поддерживается. Дополнительные сведения о физических файлах см. в статье [Файлы и файловые группы базы данных](../databases/database-files-and-filegroups.md).  
  
-   Проверьте, перестанут ли возникать сообщения об ошибках после отключения следующих параметров.  
  
    -   Параметр конфигурации SQL Server «повышение приоритета».  
  
    -   Параметр «использование упрощенных пулов» (в режиме волокон).  
  
    -   Параметр «set working set size».  
  
    > [!NOTE]  
    >  Приведенные выше параметры зачастую ухудшают производительность, если их значения по умолчанию отличаются от OFF. Дополнительные сведения см. в статье [Параметры конфигурации сервера (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
-   Настройте запросы таким образом, чтобы система потребляла меньший объем ресурсов. Настройка производительности поможет снизить нагрузку на систему и сократить время отклика отдельных запросов.  
  
-   Присвойте параметру AUTO_SHRINK значение OFF для снижения затрат на изменение размера базы данных.  
  
-   Убедитесь, что приращения, заданные с помощью параметра FILEGROWTH, велики настолько, чтобы выполняться достаточно редко. Запланируйте задание проверки доступного места на диске в базах данных, затем задайте увеличение размера базы данных в периоды наименьшей нагрузки.  
  
  

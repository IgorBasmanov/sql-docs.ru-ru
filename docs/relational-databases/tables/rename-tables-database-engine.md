---
title: Переименование таблиц (ядро СУБД) | Документация Майкрософт
ms.custom: ''
ms.date: 02/23/2018
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- table renaming [SQL Server]
- table names [SQL Server]
- tables [SQL Server], Visual Database Tools
- renaming tables
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c8a9fd43e62bda03b8f994e2a8639524b4bea645
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68016334"
---
# <a name="rename-tables-database-engine"></a>Переименование таблиц (компонент Database Engine)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

Переименование таблицы в SQL Server или базе данных SQL Azure.

Чтобы переименовать таблицу в хранилище данных SQL Azure или в Parallel Data Warehouse, используйте инструкцию [RENAME OBJECT](../../t-sql/statements/rename-transact-sql.md). 
  
> [!CAUTION]  
>  Каждое переименование таблицы следует тщательно планировать. Если существующие запросы, представления, определяемые пользователем функции, хранимые процедуры или программы ссылаются на таблицу, изменение имени таблицы сделает эти объекты недействительными.  
  
 **В этом разделе**  
  
-   **Перед началом работы**  
  
     [Ограничения](#Restrictions)  
  
     [безопасность](#Security)  
  
-   **Переименование таблицы с использованием:**  
  
     [Среда SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Перед началом  
  
###  <a name="Restrictions"></a> Ограничения  
 Переименование таблицы не приводит к автоматическому переименованию ссылок на эту таблицу. Необходимо вручную изменить все объекты, которые ссылаются на переименованную таблицу. Например, если переименована таблица и на эту таблицу имеется ссылка в триггере, то необходимо изменить триггер, указав новое имя таблицы. Используйте представление каталога [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) , чтобы составить список зависимостей для таблицы перед переименованием.  
  
###  <a name="Security"></a> безопасность  
  
####  <a name="Permissions"></a> Permissions  
 Требуется разрешение ALTER на таблицу.  
  
##  <a name="SSMSProcedure"></a> Использование среды SQL Server Management Studio  
  
#### <a name="to-rename-a-table"></a>Переименование таблицы  
  
1.  В обозревателе объектов правой кнопкой мыши щелкните таблицу, имя которой хотите переименовать, и в контекстном меню выберите пункт **Конструирование** .  
  
2.  В меню **Просмотр** выберите команду **Свойства**.  
  
3.  В поле **Имя** окна **Свойства** введите новое имя таблицы.  
  
4.  Чтобы отменить это действие, нажмите клавишу ESC перед тем, как выйти из этого поля.  
  
5.  В меню **Файл** выберите команду **Сохранить** _имя_таблицы_.  

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

##  <a name="TsqlProcedure"></a> Использование Transact-SQL  
  
#### <a name="to-rename-a-table"></a>Переименование таблицы  
  
1.  В **обозревателе объектов**подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  На стандартной панели выберите пункт **Создать запрос**.  
  
3.  В следующем примере столбец `SalesTerritory` в таблице `SalesTerr` переименовывается в `Sales` . Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    EXEC sp_rename 'Sales.SalesTerritory', 'SalesTerr';  
    ```  
  
 Дополнительные примеры см. в статье [sp_rename (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md).  
  
  

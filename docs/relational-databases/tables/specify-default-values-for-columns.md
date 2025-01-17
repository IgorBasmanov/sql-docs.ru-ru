---
title: Указание значений по умолчанию для столбцов | Документация Майкрософт
ms.custom: ''
ms.date: 02/20/2019
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], defaults
- default values
ms.assetid: 64514aed-b846-407b-992e-cf813f9a1a91
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5e00c22ed3b7728d906ab2a143ca30f2eec8c7ee
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68016280"
---
# <a name="specify-default-values-for-columns"></a>Указание значений по умолчанию для столбца

[!INCLUDE[tsql-appliesto-ss2016-all-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

Можно использовать [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], чтобы указать значение по умолчанию, которое будет включено в столбец таблицы. Можно использовать обозреватель объектов в пользовательском интерфейсе или основные элементы управления для отправки [!INCLUDE[tsql](../../includes/tsql-md.md)].

Если значение по умолчанию не задано столбцу и пользователь оставляет столбец пустым, происходит следующее:

- если активирована поддержка значений NULL, в столбец вставляется значение NULL;

- если поддержка значений NULL не активирована, столбец остается пустым, но пользователь не сможет сохранить строку, пока не предоставит какое-либо значение.

## <a name="Restrictions"></a> Ограничения

Перед началом работы необходимо учесть следующие ограничения:

- Если данные, введенные в поле **Значение по умолчанию** , заменяют связанное со столбцом значение по умолчанию (которое отображается без скобок), то будет предложено отменить привязку значения по умолчанию и заменить его новым значением.

- При вводе текстовых строк заключайте их в одинарные кавычки ('); не используйте двойные кавычки ("), потому что они зарезервированы для идентификаторов.

- Чтобы задать численное значение по умолчанию, введите число без одинарных кавычек.

- Чтобы задать объект или функцию, введите имя объекта или функции без двойных кавычек.

### <a name="Security"></a> Разрешения безопасности

Для выполнения действий, описанных в этой статье, требуется разрешение ALTER для таблицы.

## <a name="SSMSProcedure"></a> Использование SSMS для определения значения по умолчанию

Можно использовать обозреватель объектов, чтобы указать значение по умолчанию для столбца таблицы.

### <a name="object-explorer"></a>обозревателе объектов

1. В **Обозревателе объектов**щелкните правой кнопкой мыши таблицу со столбцами, масштаб которых необходимо изменить, и выберите пункт **Конструктор**.

2. Выберите столбец, для которого нужно задать значение по умолчанию.

3. На вкладке **Свойства столбца** введите новое значение по умолчанию в свойстве **Значение по умолчанию или привязка** .

   > [!NOTE]
   > Чтобы задать численное значение по умолчанию, введите число. В случае объекта или функции нужно ввести его или ее имя. Чтобы задать алфавитно-цифровое значение по умолчанию, введите его, заключив в одинарные кавычки.

4. В меню **Файл** выберите пункт **Сохранить** _имя_таблицы_.

[!INCLUDE[freshInclude](../../includes/paragraph-content/fresh-note-steps-feedback.md)]

## <a name="TsqlProcedure"></a> Использование Transact-SQL для определения значения по умолчанию

Есть несколько способов указать значение по умолчанию для столбца с помощью среды SSMS для отправки T-SQL.

### <a name="alter-table-t-sql"></a>ALTER TABLE (T-SQL)

1. В **обозревателе объектов**подключитесь к экземпляру компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)].

2. На стандартной панели выберите пункт **Создать запрос**.

3. Скопируйте следующий пример в окно запроса и нажмите кнопку **Выполнить**.

   ```sql
   CREATE TABLE dbo.doc_exz (column_a INT, column_b INT); -- Allows nulls.
   GO
   INSERT INTO dbo.doc_exz (column_a) VALUES (7);
   GO
   ALTER TABLE dbo.doc_exz
     ADD CONSTRAINT col_b_def
     DEFAULT 50 FOR column_b;
   GO
   ```

<!--
The following two T-SQL code examples were offered by 'nycdotnet' (Steve) via public PR 1660, Feb 2019.
-->

### <a name="create-table-t-sql"></a>CREATE TABLE (T-SQL)

```sql
    CREATE TABLE dbo.doc_exz (
      column_a INT,
      column_b INT DEFAULT 50);
```

### <a name="named-constraint-t-sql"></a>CONSTRAINT (T-SQL) с именем

```sql
    CREATE TABLE dbo.doc_exz (
      column_a INT,
      column_b INT CONSTRAINT DF_doc_exz_column_b DEFAULT 50);
```

Дополнительные сведения см. в разделе [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md).

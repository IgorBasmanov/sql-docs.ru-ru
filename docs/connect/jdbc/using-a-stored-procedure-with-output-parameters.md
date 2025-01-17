---
title: Использование хранимой процедуры с параметрами вывода | Документы Майкрософт
ms.custom: ''
ms.date: 07/11/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 1c006f27-7e99-43d5-974c-7b782659290c
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 9ee3a8d6b0a4c6514864a5990a87de9d732684d8
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67916495"
---
# <a name="using-a-stored-procedure-with-output-parameters"></a>Использование хранимых процедур с выходными параметрами

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Вызываемая хранимая процедура [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] — это процедура, возвращающая один или несколько параметров OUT, т. е. параметров, используемых хранимой процедурой для возврата данных вызывающему приложению. Драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] содержит класс [SQLServerCallableStatement](../../connect/jdbc/reference/sqlservercallablestatement-class.md), который может быть использован для вызова этого типа хранимых процедур и обработки возвращаемых ими данных.

При вызове хранимой процедуры этого типа с помощью драйвера JDBC следует использовать escape-последовательность SQL `call` совместно с методом [prepareCall](../../connect/jdbc/reference/preparecall-method-sqlserverconnection.md) класса [SQLServerConnection](../../connect/jdbc/reference/sqlserverconnection-class.md). Ниже приводится синтаксис escape-последовательности `call` с параметрами OUT:

`{call procedure-name[([parameter][,[parameter]]...)]}`

> [!NOTE]  
> Дополнительные сведения о escape-последовательностях SQL см. в разделе [использование escape](../../connect/jdbc/using-sql-escape-sequences.md)-последовательностей SQL.

При создании escape-последовательности `call` укажите параметры OUT при помощи символа "?". (символ вопросительного знака (?)). Этот символ выполняет роль заполнителя для значений параметра, которые будут возвращены из хранимой процедуры. Чтобы указать значение параметра OUT, необходимо указать тип данных всех параметров с помощью метода [registerOutParameter](../../connect/jdbc/reference/registeroutparameter-method-sqlservercallablestatement.md) класса SQLServerCallableStatement до выполнения хранимой процедуры.

Значение, указываемое для параметра OUT в методе registerOutParameter, должно представлять собой один из типов данных JDBC, содержащихся в java.sql.Types, который, в свою очередь, выполняет сопоставление с одним из собственных типов данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Дополнительные сведения о JDBC и [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] типах данных см. в разделе [Основные сведения о типах данных драйвера JDBC](../../connect/jdbc/understanding-the-jdbc-driver-data-types.md).

При передаче значения методу registerOutParameter для параметра OUT необходимо указать не только тип данных, который будет использоваться для параметра, но также порядковое размещение или имя параметра в хранимой процедуре. Например, если в хранимой процедуре имеется один параметр OUT, то первое порядковое значение будет 1, а второе порядковое значение — 2.

> [!NOTE]  
> Драйвер JDBC не поддерживает использование типов данных CURSOR, SQLVARIANT, TABLE и TIMESTAMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в качестве параметров OUT.

Для примера создайте следующую хранимую процедуру в образце базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)]:

```sql
CREATE PROCEDURE GetImmediateManager  
   @employeeID INT,  
   @managerID INT OUTPUT  
AS  
BEGIN  
   SELECT @managerID = ManagerID
   FROM HumanResources.Employee
   WHERE EmployeeID = @employeeID  
END
```

Эта хранимая процедура возвращает один параметр OUT (managerID), являющийся целым числом, на основе указанного параметра IN (employeeID), которое также является целым числом. Значение, возвращаемое в параметре OUT, представляет собой ManagerID, основанный на EmployeeID, который содержится в таблице HumanResources.Employee.

В следующем примере открытое соединение с образцом базы данных [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal_md.md)] передается в функцию, а метод [execute](../../connect/jdbc/reference/execute-method-sqlserverstatement.md) используется для вызова хранимой процедуры GetImmediateManager:

```java
public static void executeStoredProcedure(Connection con) throws SQLException {  
    try(CallableStatement cstmt = con.prepareCall("{call dbo.GetImmediateManager(?, ?)}");) {  
        cstmt.setInt(1, 5);  
        cstmt.registerOutParameter(2, java.sql.Types.INTEGER);  
        cstmt.execute();  
        System.out.println("MANAGER ID: " + cstmt.getInt(2));  
    }  
}
```

В примере для определения параметров используются порядковые местоположения. Или можно определить параметр при использовании имени вместо порядкового местоположения. В следующем примере кода изменяется предыдущий пример для иллюстрации использования именованных параметров в приложении Java. Обратите внимание, что имена параметров соответствуют именам параметров в определении хранимой процедуры:

```java
public static void executeStoredProcedure(Connection con) throws SQLException {  
    try(CallableStatement cstmt = con.prepareCall("{call dbo.GetImmediateManager(?, ?)}"); ) {  
        cstmt.setInt("employeeID", 5);  
        cstmt.registerOutParameter("managerID", java.sql.Types.INTEGER);  
        cstmt.execute();  
        System.out.println("MANAGER ID: " + cstmt.getInt("managerID"));  
    }  
}
```

> [!NOTE]  
> В этих примерах для выполнения хранимой процедуры используется метод Execute класса SQLServerCallableStatement. Он используется, поскольку хранимая процедура не возвратила результирующий набор. Если она возвратила результирующий набор, будет использован метод [executeQuery](../../connect/jdbc/reference/executequery-method-sqlserverstatement.md).

Хранимые процедуры могут возвращать счетчики обновлений и несколько результирующих наборов. Драйвер [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] соответствует спецификации JDBC 3.0, которая определяет, что множественные результирующие наборы и счетчики обновления должны быть получены до получения параметров OUT. То есть приложение должно извлечь все объекты ResultSet и счетчики обновления перед извлечением параметров OUT с помощью методов CallableStatement. Getter. В противном случае объекты ResultSet и счетчики обновления, которые не были извлечены, будут потеряны при извлечении параметров OUT. Дополнительные сведения о количестве обновлений и нескольких результирующих наборах см. в разделе [использование хранимой процедуры с числом обновлений](../../connect/jdbc/using-a-stored-procedure-with-an-update-count.md) и [Использование нескольких результирующих наборов](../../connect/jdbc/using-multiple-result-sets.md).

## <a name="see-also"></a>См. также:

[Использование инструкций с хранимыми процедурами](../../connect/jdbc/using-statements-with-stored-procedures.md)

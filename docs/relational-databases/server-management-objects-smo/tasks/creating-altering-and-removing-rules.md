---
title: Создание, изменение и удаление правил | Документация Майкрософт
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- rules [SMO]
ms.assetid: 16981459-524e-4b39-a899-4370eaf763cc
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bf1075d29ee070e9ca3cf15e30e26552e22effa0
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68115125"
---
# <a name="creating-altering-and-removing-rules"></a>Создание, изменение и удаление правил
[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

  В SMO правила представлены объектом <xref:Microsoft.SqlServer.Management.Smo.Rule>. Правило определяется свойством <xref:Microsoft.SqlServer.Management.Smo.DefaultRuleBase.TextBody%2A>, которое является текстовой строкой, содержащей выражение условия, использующее операторы или предикаты, например IN, LIKE или BETWEEN. Правило не может ссылаться на столбцы или другие объекты базы данных. В правило могут входить встроенные функции, не ссылающиеся на объекты базы данных.  
  
 Определение в свойстве <xref:Microsoft.SqlServer.Management.Smo.DefaultRuleBase.TextBody%2A> должно содержать переменную, ссылающуюся на введенное значение данных. Для представления значения при создании правила можно использовать любое имя или символ, но первым символом должна быть \@ символов.  
  
## <a name="example"></a>Пример  
 Чтобы использовать какой-либо из представленных примеров кода, нужно выбрать среду, шаблон и язык программирования, с помощью которых будет создаваться приложение. Дополнительные сведения см. в разделе [Visual C создайте&#35; проекта SMO в Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="creating-altering-and-removing-a-rule-in-visual-basic"></a>Создание, изменение и удаление правила на языке Visual Basic  
 Данный образец кода показывает, как создать правило, добавить его в столбец, изменить свойства объекта <xref:Microsoft.SqlServer.Management.Smo.Rule>, отсоединить его от столбца и удалить его.  
  
 **Dim** инструкции для <xref:Microsoft.SqlServer.Management.Smo.Rule> указывается с полным путем к сборке, чтобы избежать неоднозначности с <xref:Microsoft.SqlServer.Management.Smo.Rule> объект в сборке System.Data.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Reference the AdventureWorks2012 database.
Dim db As Database
db = srv.Databases("AdventureWorks2012")
'Declare a Table object variable and reference the Product table.
Dim tb As Table
tb = db.Tables("Product", "Production")
'Define a Rule object variable by supplying the parent database, name and schema in the constructor. 
'Note that the full namespace must be given for the Rule type to differentiate it from other Rule types.
Dim ru As Microsoft.SqlServer.Management.Smo.Rule
ru = New Rule(db, "TestRule", "Production")
'Set the TextHeader and TextBody properties to define the rule.
ru.TextHeader = "CREATE RULE [Production].[TestRule] AS"
ru.TextBody = "@value BETWEEN GETDATE() AND DATEADD(year,4,GETDATE())"
'Create the rule on the instance of SQL Server.
ru.Create()
'Bind the rule to a column in the Product table by supplying the table, schema, and 
'column as arguments in the BindToColumn method.
ru.BindToColumn("Product", "SellEndDate", "Production")
'Unbind from the column before removing the rule from the database.
ru.UnbindFromColumn("Product", "SellEndDate", "Production")
ru.Drop()
```
  
## <a name="creating-altering-and-removing-a-rule-in-visual-c"></a>Создание, изменение и удаление правила на языке Visual C#  
 Данный образец кода показывает, как создать правило, добавить его в столбец, изменить свойства объекта <xref:Microsoft.SqlServer.Management.Smo.Rule>, отсоединить его от столбца и удалить его.  
  
 **Dim** инструкции для <xref:Microsoft.SqlServer.Management.Smo.Rule> указывается с полным путем к сборке, чтобы избежать неоднозначности с <xref:Microsoft.SqlServer.Management.Smo.Rule> объект в сборке System.Data.  
  
```csharp  
{  
            //Connect to the local, default instance of SQL Server.   
            Server srv;  
            srv = new Server();  
            //Reference the AdventureWorks2012 database.   
            Database db;  
            db = srv.Databases["AdventureWorks2012"];  
  
            //Define a Rule object variable by supplying the parent database, name and schema in the constructor.   
            //Note that the full namespace must be given for the Rule type to differentiate it from other Rule types.   
            Microsoft.SqlServer.Management.Smo.Rule ru;  
            ru = new Rule(db, "TestRule", "Production");  
            //Set the TextHeader and TextBody properties to define the rule.   
            ru.TextHeader = "CREATE RULE [Production].[TestRule] AS";  
            ru.TextBody = "@value BETWEEN GETDATE() AND DATEADD(year,4,GETDATE())";  
            //Create the rule on the instance of SQL Server.   
            ru.Create();  
            //Bind the rule to a column in the Product table by supplying the table, schema, and   
            //column as arguments in the BindToColumn method.   
            ru.BindToColumn("Product", "SellEndDate", "Production");  
            //Unbind from the column before removing the rule from the database.   
            ru.UnbindFromColumn("Product", "SellEndDate", "Production");  
            ru.Drop();  
  
        }  
```  
  
## <a name="creating-altering-and-removing-a-rule-in-powershell"></a>Создание, изменение и удаление правила в PowerShell  
 Данный образец кода показывает, как создать правило, добавить его в столбец, изменить свойства объекта <xref:Microsoft.SqlServer.Management.Smo.Rule>, отсоединить его от столбца и удалить его.  
  
 **Dim** инструкции для <xref:Microsoft.SqlServer.Management.Smo.Rule> указывается с полным путем к сборке, чтобы избежать неоднозначности с <xref:Microsoft.SqlServer.Management.Smo.Rule> объект в сборке System.Data.  
  
```powershell   
# Set the path context to the local, default instance of SQL Server and get a reference to AdventureWorks2012  
CD \sql\localhost\default\databases  
$db = get-item Adventureworks2012  
  
# Define a Rule object variable by supplying the parent database, name and schema in the constructor.   
$ru = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Rule `  
-argumentlist $db, "TestRule", "Production"  
  
#Set the TextHeader and TextBody properties to define the rule.   
$ru.TextHeader = "CREATE RULE [Production].[TestRule] AS"  
$ru.TextBody = "@value BETWEEN GETDATE() AND DATEADD(year,4,GETDATE())"  
  
#Create the rule on the instance of SQL Server.   
$ru.Create()  
  
# Bind the rule to a column in the Product table by supplying the table, schema, and   
# column as arguments in the BindToColumn method.   
$ru.BindToColumn("Product", "SellEndDate", "Production")  
  
#Unbind from the column before removing the rule from the database.   
$ru.UnbindFromColumn("Product", "SellEndDate", "Production")  
$ru.Drop()  
```  
  
## <a name="see-also"></a>См. также  
 <xref:Microsoft.SqlServer.Management.Smo.Rule>  
  
  

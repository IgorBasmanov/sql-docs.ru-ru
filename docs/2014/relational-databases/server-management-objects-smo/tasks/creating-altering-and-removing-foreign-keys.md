---
title: Создание, изменение и удаление внешних ключей | Документация Майкрософт
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- foreign keys [SMO]
ms.assetid: d43c8dca-bb6b-4a41-8a79-c96fd546fc91
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 4f8c7474d645de0ba8b8c94beed44ee7c02d33de
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/15/2019
ms.locfileid: "63021879"
---
# <a name="creating-altering-and-removing-foreign-keys"></a>Создание, изменение и удаление внешних ключей
  В иерархии управляющих объектов [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (SMO) внешние ключи представлены объектом <xref:Microsoft.SqlServer.Management.Smo.ForeignKey>.  
  
 Чтобы создать в SMO внешний ключ, необходимо указать таблицу, в которой внешний ключ определен в конструкторе объекта <xref:Microsoft.SqlServer.Management.Smo.ForeignKey>. В этой таблице надо выбрать хотя бы один столбец, который будет внешним ключом. Для этого создайте объектную переменную <xref:Microsoft.SqlServer.Management.Smo.ForeignKeyColumn> и укажите имя столбца, который станет внешним ключом. Теперь укажите таблицу и столбец, на которые будут выполняться ссылки. Воспользуйтесь методом <xref:Microsoft.SqlServer.Management.Smo.ForeignKeyColumnCollection.Add%2A> для добавления столбца в свойство объекта `Columns`.  
  
 Столбцы, представляющие внешний ключ, перечислены в свойстве `Columns` объекта <xref:Microsoft.SqlServer.Management.Smo.ForeignKey>. Первичный ключ, на который ссылается внешний ключ, представлен свойством <xref:Microsoft.SqlServer.Management.Smo.ForeignKey.ReferencedKey%2A>, которое находится в таблице, указанной в свойстве <xref:Microsoft.SqlServer.Management.Smo.ForeignKey.ReferencedTable%2A>.  
  
## <a name="example"></a>Пример  
 Чтобы использовать какой-либо из представленных примеров кода, нужно выбрать среду, шаблон и язык программирования, с помощью которых будет создаваться приложение. Дополнительные сведения см. в разделе [Создание проекта SMO на Visual Basic в Visual Studio .NET](../../../database-engine/dev-guide/create-a-visual-basic-smo-project-in-visual-studio-net.md) или [Visual C создайте&#35; проекта SMO в Visual Studio .NET](../how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="creating-altering-and-removing-a-foreign-key-in-visual-basic"></a>Создание, изменение и удаление внешнего ключа на языке Visual Basic  
 Этот пример кода показывает, как создать связь по внешнему ключу между одним или несколькими столбцами одной таблицы и первичным ключевым столбцом другой таблицы.  
  
<!-- TODO: review snippet reference  [!CODE [SMO How to#SMO_VBForeignKeys1](SMO How to#SMO_VBForeignKeys1)]  -->  
  
## <a name="creating-altering-and-removing-a-foreign-key-in-visual-c"></a>Создание, изменение и удаление внешнего ключа на языке Visual C#  
 Этот пример кода показывает, как создать связь по внешнему ключу между одним или несколькими столбцами одной таблицы и первичным ключевым столбцом другой таблицы.  
  
```  
{  
            //Connect to the local, default instance of SQL Server.   
            Server srv;  
            srv = new Server();  
            //Reference the AdventureWorks2012 database.   
            Database db;  
            db = srv.Databases["AdventureWorks2012"];  
            //Declare another Table object variable and reference the EmployeeDepartmentHistory table.   
            Table tbea;  
            tbea = db.Tables["EmployeeDepartmentHistory", "HumanResources"];  
            //Define a Foreign Key object variable by supplying the EmployeeDepartmentHistory as the parent table and the foreign key name in the constructor.   
            ForeignKey fk;  
            fk = new ForeignKey(tbea, "test_foreignkey");  
            //Add BusinessEntityID as the foreign key column.   
            ForeignKeyColumn fkc;  
            fkc = new ForeignKeyColumn(fk, "BusinessEntityID", "BusinessEntityID");  
            fk.Columns.Add(fkc);  
            //Set the referenced table and schema.   
            fk.ReferencedTable = "Employee";  
            fk.ReferencedTableSchema = "HumanResources";  
            //Create the foreign key on the instance of SQL Server.   
            fk.Create();  
        }  
```  
  
## <a name="creating-altering-and-removing-a-foreign-key-in-powershell"></a>Создание, изменение и удаление внешнего ключа в PowerShell  
 Этот пример кода показывает, как создать связь по внешнему ключу между одним или несколькими столбцами одной таблицы и первичным ключевым столбцом другой таблицы.  
  
```  
# Set the path context to the local, default instance of SQL Server and to the  
#database tables in Adventureworks2012  
CD \sql\localhost\default\databases\AdventureWorks2012\Tables\  
  
#Get reference to the FK table  
$tbea = get-item HumanResources.EmployeeDepartmentHistory  
  
# Define a Foreign Key object variable by supplying the EmployeeDepartmentHistory  
# as the parent table and the foreign key name in the constructor.   
$fk = New-Object -TypeName Microsoft.SqlServer.Management.SMO.ForeignKey `  
-argumentlist $tbea, "test_foreignkey"  
  
#Add BusinessEntityID as the foreign key column.   
$fkc = New-Object -TypeName Microsoft.SqlServer.Management.SMO.ForeignKeyColumn `  
-argumentlist $fk, "BusinessEntityID", "BusinessEntityID"  
$fk.Columns.Add($fkc)  
  
#Set the referenced table and schema.   
$fk.ReferencedTable = "Employee"  
$fk.ReferencedTableSchema = "HumanResources"  
  
#Create the foreign key on the instance of SQL Server.   
$fk.Create()  
```  
  
## <a name="sample-foreign-keys-primary-keys-and-unique-constraint-columns"></a>Образец. Внешние ключи, первичные ключи и столбцы с ограничением уникальности  
 В этом образце показано, как:  
  
-   Создать внешний ключ для существующего объекта.  
  
-   Создать первичный ключ.  
  
-   Создать столбец с ограничением уникальности.  
  
 Версия на языке C#:  
  
```  
// compile with:   
// /r:Microsoft.SqlServer.Smo.dll   
// /r:microsoft.sqlserver.management.sdk.sfc.dll   
// /r:Microsoft.SqlServer.ConnectionInfo.dll  
// /r:Microsoft.SqlServer.SqlEnum.dll  
  
using Microsoft.SqlServer.Management.Smo;  
using Microsoft.SqlServer.Management.Sdk.Sfc;  
using Microsoft.SqlServer.Management.Common;  
using System;  
  
public class A {  
   public static void Main() {  
      Server svr = new Server();  
      Database db = new Database(svr, "TESTDB");  
      db.Create();  
  
      // PK Table  
      Table tab1 = new Table(db, "Table1");  
  
      // Define Columns and add them to the table  
      Column col1 = new Column(tab1, "Col1", DataType.Int);  
  
      col1.Nullable = false;  
      tab1.Columns.Add(col1);  
      Column col2 = new Column(tab1, "Col2", DataType.NVarChar(50));  
      tab1.Columns.Add(col2);  
      Column col3 = new Column(tab1, "Col3", DataType.DateTime);  
      tab1.Columns.Add(col3);  
  
      // Create the ftable  
      tab1.Create();  
  
      // Define Index object on the table by supplying the Table1 as the parent table and the primary key name in the constructor.  
      Index pk = new Index(tab1, "Table1_PK");  
      pk.IndexKeyType = IndexKeyType.DriPrimaryKey;  
  
      // Add Col1 as the Index Column  
      IndexedColumn idxCol1 = new IndexedColumn(pk, "Col1");  
      pk.IndexedColumns.Add(idxCol1);  
  
      // Create the Primary Key  
      pk.Create();  
  
      // Create Unique Index on the table  
      Index unique = new Index(tab1, "Table1_Unique");  
      unique.IndexKeyType = IndexKeyType.DriUniqueKey;  
  
      // Add Col1 as the Unique Index Column  
      IndexedColumn idxCol2 = new IndexedColumn(unique, "Col2");  
      unique.IndexedColumns.Add(idxCol2);  
  
      // Create the Unique Index  
      unique.Create();  
  
      // Create Table2                    
      Table tab2 = new Table(db, "Table2");  
      Column col21 = new Column(tab2, "Col21", DataType.NChar(20));  
      tab2.Columns.Add(col21);  
      Column col22 = new Column(tab2, "Col22", DataType.Int);  
      tab2.Columns.Add(col22);  
      tab2.Create();  
  
      // Define a Foreign Key object variable by supplying the Table2 as the parent table and the foreign key name in the constructor.   
      ForeignKey fk = new ForeignKey(tab2, "Table2_FK");  
  
      // Add Col22 as the foreign key column.   
      ForeignKeyColumn fkc = new ForeignKeyColumn(fk, "Col22", "Col1");  
      fk.Columns.Add(fkc);  
      fk.ReferencedTable = "Table1";  
  
      // Create the foreign key on the instance of SQL Server.   
      fk.Create();                    
  
      // Get list of Foreign Keys on Table2  
      foreach (ForeignKey f in tab2.ForeignKeys) {  
            Console.WriteLine(f.Name + " " + f.ReferencedTable + " " + f.ReferencedKey);  
      }  
  
      // Get list of Foreign Keys referencing table1  
      foreach (Table tab in db.Tables) {  
         if (tab == tab1)  
            continue;  
            foreach (ForeignKey f in tab.ForeignKeys) {  
               if (f.ReferencedTable.Equals(tab1.Name))  
                  Console.WriteLine(f.Name + " " + f.Parent.Name);  
            }  
      }  
   }  
}  
```  
  
 Версия на языке Visual Basic:  
  
```  
' compile with:   
' /r:Microsoft.SqlServer.Smo.dll   
' /r:microsoft.sqlserver.management.sdk.sfc.dll   
' /r:Microsoft.SqlServer.ConnectionInfo.dll  
' /r:Microsoft.SqlServer.SqlEnum.dll  
  
Imports Microsoft.SqlServer.Management.Smo  
Imports Microsoft.SqlServer.Management.Sdk.Sfc  
Imports Microsoft.SqlServer.Management.Common  
  
Public Class A  
   Public Shared Sub Main()  
      Dim svr As New Server()  
      Dim db As New Database(svr, "TESTDB")  
      db.Create()  
  
      ' PK Table  
      Dim tab1 As New Table(db, "Table1")  
  
      ' Define Columns and add them to the table  
      Dim col1 As New Column(tab1, "Col1", DataType.Int)  
  
      col1.Nullable = False  
      tab1.Columns.Add(col1)  
      Dim col2 As New Column(tab1, "Col2", DataType.NVarChar(50))  
      tab1.Columns.Add(col2)  
      Dim col3 As New Column(tab1, "Col3", DataType.DateTime)  
      tab1.Columns.Add(col3)  
  
      ' Create the ftable  
      tab1.Create()  
  
      ' Define Index object on the table by supplying the Table1 as the parent table and the primary key name in the constructor.  
      Dim pk As New Index(tab1, "Table1_PK")  
      pk.IndexKeyType = IndexKeyType.DriPrimaryKey  
  
      ' Add Col1 as the Index Column  
      Dim idxCol1 As New IndexedColumn(pk, "Col1")  
      pk.IndexedColumns.Add(idxCol1)  
  
      ' Create the Primary Key  
      pk.Create()  
  
      ' Create Unique Index on the table  
      Dim unique As New Index(tab1, "Table1_Unique")  
      unique.IndexKeyType = IndexKeyType.DriUniqueKey  
  
      ' Add Col1 as the Unique Index Column  
      Dim idxCol2 As New IndexedColumn(unique, "Col2")  
      unique.IndexedColumns.Add(idxCol2)  
  
      ' Create the Unique Index  
      unique.Create()  
  
      ' Create Table2                    
      Dim tab2 As New Table(db, "Table2")  
      Dim col21 As New Column(tab2, "Col21", DataType.NChar(20))  
      tab2.Columns.Add(col21)  
      Dim col22 As New Column(tab2, "Col22", DataType.Int)  
      tab2.Columns.Add(col22)  
      tab2.Create()  
  
      ' Define a Foreign Key object variable by supplying the Table2 as the parent table and the foreign key name in the constructor.   
      Dim fk As New ForeignKey(tab2, "Table2_FK")  
  
      ' Add Col22 as the foreign key column.   
      Dim fkc As New ForeignKeyColumn(fk, "Col22", "Col1")  
      fk.Columns.Add(fkc)  
      fk.ReferencedTable = "Table1"  
  
      ' Create the foreign key on the instance of SQL Server.   
      fk.Create()  
  
      ' Get list of Foreign Keys on Table2  
      For Each f As ForeignKey In tab2.ForeignKeys  
         Console.WriteLine((f.Name + " " + f.ReferencedTable & " ") + f.ReferencedKey)  
      Next  
  
      ' Get list of Foreign Keys referencing table1  
      For Each tab As Table In db.Tables  
         If (tab.Name.Equals(tab1.Name)) Then  
            Continue For  
         End If  
         For Each f As ForeignKey In tab.ForeignKeys  
            If f.ReferencedTable.Equals(tab1.Name) Then  
               Console.WriteLine(f.Name + " " + f.Parent.Name)  
            End If  
         Next  
      Next  
   End Sub  
End Class  
```  
  
  

---
title: Создание, изменение и удаление баз данных | Документация Майкрософт
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- databases [SMO]
- databases [SMO], creating
- databases [SMO], modifying
- databases [SMO], deleting
ms.assetid: fcfb3ec2-7556-4f72-971a-501295892cb0
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a79d800a307683471422bdd75ce1db0c9bf6d8f1
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67904664"
---
# <a name="creating-altering-and-removing-databases"></a>Создание, изменение и удаление баз данных
[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md](../../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

  В SMO база данных представлена объектом <xref:Microsoft.SqlServer.Management.Smo.Database>.  
  
 Чтобы изменить или удалить ее, необязательно создавать объект <xref:Microsoft.SqlServer.Management.Smo.Database>. На базу данных можно сослаться с помощью коллекции.  
  
## <a name="example"></a>Пример  
 Чтобы использовать какой-либо из представленных примеров кода, нужно выбрать среду, шаблон и язык программирования, с помощью которых будет создаваться приложение. Дополнительные сведения см. в разделе [Visual C создайте&#35; проекта SMO в Visual Studio .NET](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
## <a name="creating-altering-and-removing-a-database-in-visual-basic"></a>Создание, изменение и удаление базы данных на языке Visual Basic  
 В этом примере кода создается новая база данных. Для этой базы данных автоматически создаются файлы и файловые группы.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Define a Database object variable by supplying the server and the database name arguments in the constructor.
Dim db As Database
db = New Database(srv, "Test_SMO_Database")
'Create the database on the instance of SQL Server.
db.Create()
'Reference the database and display the date when it was created.
db = srv.Databases("Test_SMO_Database")
Console.WriteLine(db.CreateDate)
'Remove the database.
db.Drop()
```
  
## <a name="creating-altering-and-removing-a-database-in-visual-c"></a>Создание, изменение и удаление базы данных на языке Visual C#  
 В этом примере кода создается новая база данных. Для этой базы данных автоматически создаются файлы и файловые группы.  
  
```csharp  
{  
                //Connect to the local, default instance of SQL Server.   
                Server srv;  
                srv = new Server();  
                //Define a Database object variable by supplying the server and the database name arguments in the constructor.   
                Database db;  
                db = new Database(srv, "Test_SMO_Database");  
                //Create the database on the instance of SQL Server.   
                db.Create();  
                //Reference the database and display the date when it was created.   
                db = srv.Databases["Test_SMO_Database"];  
                Console.WriteLine(db.CreateDate);  
                //Remove the database.   
                db.Drop();  
            }  
```  
  
## <a name="creating-altering-and-removing-a-database-in-powershell"></a>Создание, изменение и удаление баз данных в PowerShell  
 В этом примере кода создается новая база данных. Для этой базы данных автоматически создаются файлы и файловые группы.  
  
```powershell  
#Get a server object which corresponds to the default instance  
cd \sql\localhost\  
$srv = get-item default  
  
#Create a new database  
$db = New-Object -TypeName Microsoft.SqlServer.Management.Smo.Database -argumentlist $srv, "Test_SMO_Database"  
$db.Create()  
  
#Reference the database and display the date when it was created.   
$db = $srv.Databases["Test_SMO_Database"]  
$db.CreateDate  
  
#Drop the database  
$db.Drop()  
```  
  
## <a name="see-also"></a>См. также  
 <xref:Microsoft.SqlServer.Management.Smo.Database>  
  
  

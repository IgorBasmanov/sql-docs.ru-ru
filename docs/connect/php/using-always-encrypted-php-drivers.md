---
title: Использование функции Always Encrypted с драйверами PHP для SQL Server | Документы Майкрософт
ms.date: 01/08/2018
ms.prod: sql
ms.prod_service: connectivity
ms.custom: ''
ms.technology: connectivity
ms.topic: conceptual
author: v-kaywon
ms.author: v-kaywon
manager: v-mabarw
ms.openlocfilehash: 08165bf0f60265e34f6b8022fd00e11dda47f672
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: MTE75
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68258643"
---
# <a name="using-always-encrypted-with-the-php-drivers-for-sql-server"></a>Использование функции Always Encrypted с драйверами PHP для SQL Server
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

## <a name="applicable-to"></a>Применимо к
 -   Драйверы Майкрософт версии 5.2 для PHP для SQL Server
 
## <a name="introduction"></a>Введение

В этой статье описано, как разрабатывать приложения PHP с помощью [драйвера PHP Driver for SQL Server](../../connect/php/Microsoft-php-driver-for-sql-server.md) с [Always Encrypted (ядро СУБД)](../../relational-databases/security/encryption/always-encrypted-database-engine.md).

Функция Always Encrypted позволяет шифровать конфиденциальные данные в клиентских приложениях, не раскрывая данные или ключи шифрования для SQL Server или Базы данных SQL Azure. Драйвер с поддержкой Always Encrypted, такой как драйвер OLE DB для SQL Server, выполняет прозрачное шифрование и расшифровку конфиденциальных данных в клиентском приложении. Драйвер автоматически определяет, какие параметры запроса соответствуют важным столбцам базы данных (защищенным с помощью Always Encrypted), и шифрует значения этих параметров перед передачей данных в SQL Server или Базу данных SQL Azure. Аналогичным образом драйвер прозрачно расшифровывает данные, полученные из зашифрованных столбцов базы в результатах запроса. Дополнительные сведения см. в разделе [Always Encrypted (ядро СУБД)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Драйверы PHP для SQL Server используют драйвер ODBC для SQL Server для шифрования конфиденциальных данных.

## <a name="prerequisites"></a>предварительные требования

 -   Настройте функцию постоянного шифрования в базе данных. В процесс настройки входят действия по подготовке ключей Always Encrypted и настройке шифрования для выбранных столбцов базы данных. Если в базе данных постоянное шифрование еще не настроено, следуйте инструкциям в разделе [Приступая к работе с постоянным шифрованием](../../relational-databases/security/encryption/always-encrypted-database-engine.md#getting-started-with-always-encrypted). В частности база данных должна содержать определения метаданных для главного ключа столбца (CMK), ключа шифрования столбца (CEK) и таблицы с одним или несколькими столбцами, зашифрованными с помощью этого ключа CEK.
 -   Убедитесь, что на компьютере разработки установлен драйвер ODBC для SQL Server версии 17 или выше. Дополнительные сведения см. в разделе [драйвер ODBC для SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md).

## <a name="enabling-always-encrypted-in-a-php-application"></a>Включение функции Always Encrypted в приложении PHP

Самым простым способом включения шифрования параметров для зашифрованных столбцов и расшифровки результатов запросов является установка для ключевого слова строки подключения `ColumnEncryption` значения `Enabled`. Ниже приведены примеры включения Always Encrypted в драйверах SQLSRV и PDO_SQLSRV:

SQLSRV:
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled");
$conn = sqlsrv_connect($server, $connectionInfo);
```

PDO_SQLSRV:
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```

Включенной функции Always Encrypted недостаточно для успешного шифрования или расшифровки. Вам нужно убедиться в следующем:
 -   Приложение имеет разрешения VIEW ANY COLUMN MASTER KEY DEFINITION и VIEW ANY COLUMN ENCRYPTION KEY DEFINITION для базы данных, необходимые для доступа к метаданным о ключах Always Encrypted в базе данных. См. подробнее о [разрешениях базы данных](../../relational-databases/security/encryption/always-encrypted-database-engine.md#database-permissions).
 -   Приложение может обращаться к CMK для защиты ключей CEK для запрашиваемых зашифрованных столбцов. Это требование зависит от поставщика хранилища ключей, в котором хранится CMK. См. подробнее о [работе с хранилищами главных ключей столбцов](#working-with-column-master-key-stores).

## <a name="retrieving-and-modifying-data-in-encrypted-columns"></a>Получение и изменение данных в зашифрованных столбцах

После включения Always Encrypted для подключения можно использовать стандартные API SQLSRV (см. [Справочник по API драйвера sqlsrv](../../connect/php/sqlsrv-driver-api-reference.md)) или PDO_SQLSRV API (см. Справочник по API [драйвера PDO_SQLSRV](../../connect/php/pdo-sqlsrv-driver-reference.md)) для извлечения или изменения данных в зашифрованных столбцах базы данных. Если приложение имеет необходимые разрешения для базы данных и может обращаться к главному ключу столбца, драйвер будет шифровать все параметры запроса, затрагивающие зашифрованные столбцы, и расшифровывать данные, извлекаемые из зашифрованных столбцов. Для приложения это все выполняется незаметно, как если бы столбцы не были зашифрованы.

Если функция Always Encrypted не включена, выполнение запросов с параметрами, предназначенными для зашифрованных столбцов, завершится ошибкой. Данные по-прежнему могут извлекаться из зашифрованных столбцов, пока для них не будут указаны параметры, предназначенные для зашифрованных столбцов. При этом драйвер не будет применять расшифровку, и приложение будет получать двоичные зашифрованные данные (в виде массивов байтов).

В приведенной ниже таблице описывается поведение запросов в зависимости от того, включена ли функция Always Encrypted:

|Характеристика запроса|Постоянное шифрование включено, и приложение может получать доступ к ключам и метаданным ключей|Постоянное шифрование включено, и приложение не может получать доступ к ключам и метаданным ключей|Постоянное шифрование отключено|
|---|---|---|---|
|Параметры, предназначенные для зашифрованных столбцов.|Значения параметров прозрачно шифруются.|Ошибка|Ошибка|
|Извлечение данных из зашифрованных столбцов без параметров, предназначенных для зашифрованных столбцов.|Результаты из зашифрованных столбцов прозрачно расшифровываются. Приложение получает значения столбца в виде обычного текста. |Ошибка|Результаты из зашифрованных столбцов не расшифровываются. Приложение получает зашифрованные значения в виде массивов байтов.|
 
В следующих примерах показано получение и изменение данных в зашифрованных столбцах. В этом примере предполагается, что таблица имеет следующую схему. Столбцы SSN и BirthDate зашифрованы.
```
CREATE TABLE [dbo].[Patients](
 [PatientId] [int] IDENTITY(1,1),
 [SSN] [char](11) COLLATE Latin1_General_BIN2
 ENCRYPTED WITH (ENCRYPTION_TYPE = DETERMINISTIC,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL,
 [FirstName] [nvarchar](50) NULL,
 [LastName] [nvarchar](50) NULL,
 [BirthDate] [date]
 ENCRYPTED WITH (ENCRYPTION_TYPE = RANDOMIZED,
 ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256',
 COLUMN_ENCRYPTION_KEY = CEK1) NOT NULL
 PRIMARY KEY CLUSTERED ([PatientId] ASC) ON [PRIMARY])
 GO
```

### <a name="data-insertion-example"></a>Пример вставки данных

В следующих примерах показано, как использовать драйверы SQLSRV и PDO_SQLSRV для вставки строки в таблицу пациента. Обратите внимание на следующие моменты.
 -   В образце кода нет ничего, связанного с шифрованием. Драйвер автоматически обнаруживает и шифрует значения параметров SSN и BirthDate, которые предназначены для зашифрованных столбцов. В этом случае шифрование является прозрачным для приложения.
 -   Значения, вставляемые в столбцы базы данных, включая зашифрованные столбцы, передаются как связанные параметры. Несмотря на то, что при отправке значений в незашифрованные столбцы использовать параметры необязательно (но настоятельно рекомендуется, так как это помогает предотвратить внедрение кода SQL), они требуются для значений, предназначенных для зашифрованных столбцов. Если значения, вставленные в столбцы SSN или BirthDate, были переданы в качестве внедренных в инструкцию запроса литералов, выполнение запроса завершится ошибкой, так как драйвер не пытается шифровать или иным образом обрабатывать литералы в запросах. В результате сервер отклонит их как несовместимые с зашифрованными столбцами.
 -   При вставке значений с помощью параметров привязки тип SQL, идентичный типу данных целевого столбца, или, преобразование которого в тип данных целевого столбца поддерживается, должно передаваться в базу данных. Это требование обусловлено тем, что Always Encrypted поддерживает недостаточное число преобразований типов (Дополнительные сведения см. в разделе [Always encrypted (ядро СУБД)](../../relational-databases/security/encryption/always-encrypted-database-engine.md)). Два драйвера PHP, SQLSRV и PDO_SQLSRV, имеют механизм, помогающий пользователю определить тип значения SQL. Таким образом, пользователю не нужно явно предоставлять тип SQL.
  -   Для драйвера SQLSRV пользователь имеет два варианта:
   -   Используйте драйвер PHP, чтобы определить и установить правильный тип SQL. В этом случае пользователь должен использовать `sqlsrv_prepare` и `sqlsrv_execute` для выполнения параметризованного запроса.
   -   Явно задайте тип SQL.
  -   Для драйвера PDO_SQLSRV пользователь не имеет возможности явно задавать тип SQL параметра. Драйвер PDO_SQLSRV автоматически помогает пользователю определить тип SQL при привязке параметра.
 -   Чтобы драйверы определили тип SQL, применяются некоторые ограничения.
  -   Драйвер SQLSRV:
   -   Если пользователь хочет, чтобы драйвер определил типы SQL для зашифрованных столбцов, пользователь должен использовать `sqlsrv_prepare` и. `sqlsrv_execute`
   -   Если `sqlsrv_query` предпочтителен, пользователь несет ответственность за указание типов SQL для всех параметров. Указанный тип SQL должен включать длину строки для строковых типов, а также масштаб и точность для десятичных типов.
  -   Драйвер PDO_SQLSRV:
   -   Атрибут `PDO::SQLSRV_ATTR_DIRECT_QUERY` инструкции не поддерживается в параметризованном запросе.
   -   Атрибут `PDO::ATTR_EMULATE_PREPARES` инструкции не поддерживается в параметризованном запросе.
   
Драйвер SQLSRV и [sqlsrv_prepare](../../connect/php/sqlsrv-prepare.md):
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Abel;
$birthDate = "1996-10-19";
$params = array($ssn, $firstName, $lastName, $birthDate);
// during sqlsrv_prepare, the driver determines the SQL types for each parameter and pass them to SQL server
$stmt = sqlsrv_prepare($conn, $query, $params);
sqlsrv_execute($stmt);
```

Драйвер SQLSRV и [sqlsrv_query](../../connect/php/sqlsrv-query.md):
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Abel";
$birthDate = "1996-10-19";
// need to provide the SQL types for ALL parameters  
// note the SQL types (including the string length) have to be the same at the column definition
$params = array(array(&$ssn, null, null, SQLSRV_SQLTYPE_CHAR(11)),
                array(&$firstName, null, null, SQLSRV_SQLTYPE_NVARCHAR(50)),
                array(&$lastName, null, null, SQLSRV_SQLTYPE_NVARCHAR(50)),
                array(&$birthDate, null, null, SQLSRV_SQLTYPE_DATE));
sqlsrv_query($conn, $query, $params);
```

Драйвер PDO_SQLSRV и [PDO::p готовка](../../connect/php/pdo-prepare.md):
```
// insertion into encrypted columns must use a parameterized query
$query = "INSERT INTO [dbo].[Patients] ([SSN], [FirstName], [LastName], [BirthDate]) VALUES (?, ?, ?, ?)";
$ssn = "795-73-9838";
$firstName = "Catherine";
$lastName = "Able";
$birthDate = "1996-10-19";
// during PDO::prepare, the driver determines the SQL types for each parameter and pass them to SQL server
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $ssn);
$stmt->bindParam(2, $firstName);
$stmt->bindParam(3, $lastName);
$stmt->bindParam(4, $birthDate);
$stmt->execute();
```

### <a name="plaintext-data-retrieval-example"></a>Пример получения данных в виде открытого текста

В следующих примерах демонстрируется фильтрация данных на основе зашифрованных значений и получение данных в виде открытого текста из зашифрованных столбцов с помощью драйверов SQLSRV и PDO_SQLSRV. Обратите внимание на следующие моменты.
 -   Значение, используемое в предложении WHERE для фильтрации по столбцу SSN, необходимо передавать, используя параметр bind, чтобы перед отправкой на сервер драйвер мог его прозрачно зашифровать.
 -   При выполнении запроса с привязанными параметрами драйверы PHP автоматически определяют тип SQL для пользователя, если только пользователь явно не указывает тип SQL при использовании драйвера SQLSRV.
 -   Все значения, выводимые программой, будут представлены в открытом тексте, так как драйвер прозрачно расшифровывает данные, полученные из столбцов SSN и BirthDate.
 
Примечание. Запросы могут выполнять сравнение на равенство для зашифрованных столбцов, только если используется детерминированное шифрование. Дополнительные сведения см. в разделе [Выбор детерминированного или случайного шифрования](../../relational-databases/security/encryption/always-encrypted-database-engine.md#selecting--deterministic-or-randomized-encryption).

SQLSRV:
```
// since SSN is an encrypted column, need to pass the value in the WHERE clause through bind parameter
$query = "SELECT [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = ?";
$ssn = "795-73-9838";
$stmt = sqlsrv_prepare($conn, $query, array(&$ssn));
// during sqlsrv_execute, the driver encrypts the ssn value and passes it to the database
sqlsrv_execute($stmt);
// fetch like usual
$row = sqlsrv_fetch_array($stmt);
```

PDO_SQLSRV:
```
// since SSN is an encrypted column, need to pass the value in the WHERE clause through bind parameter
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [SSN] = ?";
$ssn = "795-73-9838";
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $ssn);
// during PDOStatement::execute, the driver encrypts the ssn value and passes it to the database
$stmt->execute();
// fetch like usual
$row = $stmt->fetch();
```

### <a name="ciphertext-data-retrieval-example"></a>Пример получения данных в виде зашифрованного текста

Если постоянное шифрование не включено, запрос может получать данные из зашифрованных столбцов, пока для него не будут указаны параметры, предназначенные для зашифрованных столбцов.

В следующих примерах показано извлечение двоичных зашифрованных данных из зашифрованных столбцов с помощью драйверов SQLSRV и PDO_SQLSRV. Обратите внимание на следующие моменты.
 -   Так как постоянное шифрование не включено в строке подключения, запрос возвращает зашифрованные значения SSN и BirthD в виде байтовых массивов (программа преобразует значения в строки).
 -   Запрос, получающий данные из зашифрованных столбцов с отключенным постоянным шифрованием, может иметь параметры при условии, что ни один из параметров не предназначен для зашифрованного столбца. Приведенный ниже запрос выполняет фильтрацию по столбцу LastName, который не зашифрован в базе данных. Запрос, отфильтрованный по SSN или BirthDate, завершится ошибкой.
 
SQLSRV:
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName] = ?";
$lastName = "Abel";
$stmt = sqlsrv_prepare($conn, $query, array(&$lastName));
sqlsrv_execute($stmt);
$row = sqlsrv_fetch_array($stmt);
```

PDO_SQLSRV:
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE [LastName] = ?";
$lastName = "Abel";
$stmt = $conn->prepare($query);
$stmt->bindParam(1, $lastName);
$stmt->execute();
$row = $stmt->fetch();
```

### <a name="avoiding-common-problems-when-querying-encrypted-columns"></a>Как избежать распространенных проблем при запросе зашифрованных столбцов

В этом разделе описываются общие категории ошибок, возникающих при выполнении запросов к зашифрованным столбцам из приложений PHP, и приводятся рекомендации о том, как их избежать.

#### <a name="unsupported-data-type-conversion-errors"></a>Ошибки преобразования неподдерживаемых типов данных

Постоянное шифрование поддерживает несколько преобразований для зашифрованных типов данных. Подробный список поддерживаемых преобразований типов см. в статье [Always Encrypted (ядро СУБД)](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Чтобы избежать ошибок при преобразовании типов данных, сделайте следующее:
 -   При использовании драйвера sqlsrv с `sqlsrv_prepare` `sqlsrv_execute` типом SQL вместе с размером столбца и числом десятичных разрядов параметра определяется автоматически.
 -   При использовании драйвера PDO_SQLSRV для выполнения запроса тип SQL с размером столбца и числом десятичных разрядов параметра также определяется автоматически
 -   При использовании драйвера sqlsrv с `sqlsrv_query` для выполнения запроса:
  -   Тип SQL для параметра всегда точно совпадает с типом целевого столбца или может быть преобразован в тип этого столбца.
  -   Точность и масштаб параметров, предназначенных для столбцов типов данных SQL Server `decimal` и `numeric`, соответствуют точности и масштабу, настроенным для конечного столбца.
  -   В запросах, которые изменяют конечный столбец, точность параметров, предназначенных для столбцов типов данных SQL Server `datetime2`, `datetimeoffset` или `time`, не превышает точность для конечного столбца.
 -   Не используйте атрибуты `PDO::SQLSRV_ATTR_DIRECT_QUERY` инструкции PDO_SQLSRV или `PDO::ATTR_EMULATE_PREPARES` в параметризованном запросе
 
#### <a name="errors-due-to-passing-plaintext-instead-of-encrypted-values"></a>Ошибки, возникающие из-за передачи значений в виде открытого текста, а не в зашифрованном виде

Любое значение, предназначенное для зашифрованного столбца, должно быть зашифровано до отправки на сервер. Попытка вставки, изменения или фильтрации по значению, в виде открытого текста в зашифрованном столбце приведет к возникновению ошибки. Чтобы избежать таких ошибок, убедитесь в том, что:
 -   Always encrypted включен (в строке подключения задайте `ColumnEncryption` для `Enabled`ключевого слова значение).
 -   для отправки данных, предназначенных для зашифрованных столбцов, используется параметр bind. В следующем примере показан запрос, который неправильно фильтрует по литералу или константе в зашифрованном столбце (SSN):
```
$query = "SELET [SSN], [FirstName], [LastName], [BirthDate] FROM [dbo].[Patients] WHERE SSN='795-73-9838'";
```

## <a name="controlling-performance-impact-of-always-encrypted"></a>Управление влиянием постоянного шифрования на производительность

Так как Always Encrypted является технологией шифрования на стороне клиента, значительное влияние на производительность происходит на стороне клиента, а не в базе данных. Помимо затрат на операции шифрования и расшифровки существуют и другие источники снижения производительности на стороне клиента:
 -   дополнительные круговые пути к базе данных для получения метаданных для параметров запроса;
 -   Вызовы хранилища главных ключей столбцов для доступа к главному ключу столбца.
 
### <a name="round-trips-to-retrieve-metadata-for-query-parameters"></a>круговые пути для получения метаданных для параметров запроса.

Если для соединения включена функция Always Encrypted, по умолчанию драйвер будет вызывать [sys.sp_describe_parameter_encryption](../../relational-databases/system-stored-procedures/sp-describe-parameter-encryption-transact-sql.md) для каждого параметризованного запроса, передавая инструкцию запроса (без значений параметров) в SQL Server. Эта хранимая процедура анализирует инструкцию запроса и выясняет, нужно ли шифровать параметры. Если да, то она возвращает по каждому параметру связанные с шифрованием сведения, позволяющие драйверу шифровать их.

Поскольку драйверы PHP позволяют пользователю привязать параметр в подготовленной инструкции без указания типа SQL, при привязке параметра в соединении с включенным Always Encrypted, драйверы PHP вызывают [SQLDescribeParam](../../odbc/reference/syntax/sqldescribeparam-function.md) для параметра, чтобы получить тип SQL. Размер столбца и десятичные цифры. Затем метаданные используются для вызова [SQLBindParameter]( ../../odbc/reference/syntax/sqlbindparameter-function.md). Эти дополнительные `SQLDescribeParam` вызовы не потребует дополнительных обращений к базе данных, так как драйвер ODBC уже сохранил информацию на стороне клиента при `sys.sp_describe_parameter_encryption` вызове.

Такое поведение гарантирует для клиентского приложения высокий уровень прозрачности. Так как значения, предназначенные для зашифрованных столбцов, передаются драйверу в параметрах, приложению (и разработчику приложения) не нужно знать, какие запросы обращаются к зашифрованным столбцам.

В отличие от драйвера ODBC для SQL Server включение Always Encrypted на уровне инструкции или запроса пока не поддерживается в драйверах PHP. 

### <a name="column-encryption-key-caching"></a>Кэширование ключа шифрования столбца

Чтобы уменьшить количество вызовов к хранилищу главных ключей столбцов для расшифровки ключей шифрования столбцов (CEK), драйвер кэширует CEK в памяти в формате открытого текста. Получив зашифрованные CEK (ECEK) из метаданных базы данных, драйвер ODBC сначала пытается найти в кэше CEK в соответствии с зашифрованным значением ключа. Драйвер обращается к хранилищу ключей, где хранится главный ключ столбца, только в том случае, если ему не удается найти в кэше значение CEK в открытом тексте.

Примечание. Драйвер ODBC Driver for SQL Server удаляет записи из этого кэша через два часа. Это означает, что для каждого ECEK драйвер обращается в хранилище ключей только один раз за цикл жизни приложения либо каждые два часа в зависимости от того, что меньше.

## <a name="working-with-column-master-key-stores"></a>Работа с хранилищами главных ключей столбцов

Для шифрования или расшифровки данных драйвер должен получить ключ CEK, который настроен для целевого столбца. Ключи CEK хранятся в зашифрованном виде (ECEK) в метаданных базы данных. Каждому ключу CEK соответствует определенный ключ CMK, который использовался для его шифрования. [Метаданные базы данных](../../t-sql/statements/create-column-master-key-transact-sql.md) не хранят значения CMK, а только имя хранилища ключей и информацию для поиска нужных CMK в этом хранилище ключей.

Чтобы получить значение ECEK в формате открытого текста, драйвер сначала получает метаданные о ключах CEK и CMK, а затем на основе этих сведений обращается в хранилище ключей с CMK и запрашивает расшифрованную версию ECEK. Драйвер взаимодействует с хранилищем ключей через поставщик хранилища ключей.

В Microsoft Driver 5.3.0 для PHP for SQL Server поддерживаются только поставщики хранилища сертификатов Windows и Azure Key Vault. Другой поставщик хранилища ключей, поддерживаемый драйвером ODBC (пользовательский поставщик хранилища ключей), пока не поддерживается.

### <a name="using-the-windows-certificate-store-provider"></a>Использование поставщика хранилища сертификатов Windows

Драйвер ODBC Driver for SQL Server в Windows содержит встроенный поставщик `MSSQL_CERTIFICATE_STORE` хранилища сертификатов Windows для хранения главного ключа. (Этот поставщик недоступен для macOS и Linux.) При использовании этого поставщика ключи CMK хранятся локально на клиентском компьютере, и для работы с ним не нужно вносить никаких дополнительных изменений в конфигурацию приложения. Но такому приложению необходимы права доступа к сертификату и закрытому ключу, размещенным в хранилище. Дополнительные сведения см. в разделе [Create and Store Column Master Keys (Always Encrypted)](../../relational-databases/security/encryption/create-and-store-column-master-keys-always-encrypted.md)(Создание и хранение главных ключей столбцов (постоянное шифрование)).

### <a name="using-azure-key-vault"></a>Использование Azure Key Vault

Azure Key Vault предлагает способ хранения ключей шифрования, паролей и других секретов с помощью Azure и может использоваться для хранения ключей для Always Encrypted. Драйвер ODBC для SQL Server (версии 17 и выше) включает встроенный поставщик хранилища главных ключей для Azure Key Vault. Следующие параметры соединения обработаны Azure Key Vault конфигурации `KeyStoreAuthentication`: `KeyStorePrincipalId`, и `KeyStoreSecret`. 
 -   `KeyStoreAuthentication`может принимать одно из двух возможных строковых значений: `KeyVaultPassword` и `KeyVaultClientSecret`. Эти значения определяют, какой тип учетных данных проверки подлинности используется с другими двумя ключевыми словами.
 -   `KeyStorePrincipalId`принимает строку, представляющую идентификатор учетной записи, которая ищет доступ к Azure Key Vault. 
     -   Если `KeyStoreAuthentication` параметр имеет `KeyVaultPassword`значение, то `KeyStorePrincipalId` это должно быть имя пользователя Azure ActiveDirectory.
     -   Если `KeyStoreAuthentication` параметр имеет `KeyVaultClientSecret`значение, то `KeyStorePrincipalId` должен быть идентификатором клиента приложения.
 -   `KeyStoreSecret`принимает строку, представляющую секрет учетных данных. 
     -   Если `KeyStoreAuthentication` параметр имеет `KeyVaultPassword`значение, то `KeyStoreSecret` он должен быть паролем пользователя. 
     -   Если `KeyStoreAuthentication` параметр имеет `KeyVaultClientSecret`значение, то `KeyStoreSecret` он должен быть секретом приложения, связанным с идентификатором клиента приложения.

В строке подключения для использования Azure Key Vault должны присутствовать все три параметра. Кроме того, `ColumnEncryption` необходимо `Enabled`задать значение. Если `ColumnEncryption` параметр имеет `Disabled` значение, но имеются параметры Azure Key Vault, скрипт продолжит работу без ошибок, но шифрование выполняться не будет.

В следующих примерах показано, как подключиться к SQL Server с помощью Azure Key Vault.

SQLSRV:

Использование учетной записи Azure Active Directory:
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled", "KeyStoreAuthentication"=>"KeyVaultPassword", "KeyStorePrincipalId"=>$AADUsername, "KeyStoreSecret"=>$AADPassword);
$conn = sqlsrv_connect($server, $connectionInfo);
```
С помощью идентификатора клиента приложения Azure и секрета:
```
$connectionInfo = array("Database"=>$databaseName, "UID"=>$uid, "PWD"=>$pwd, "ColumnEncryption"=>"Enabled", "KeyStoreAuthentication"=>"KeyVaultClientSecret", "KeyStorePrincipalId"=>$applicationClientID, "KeyStoreSecret"=>$applicationClientSecret);
$conn = sqlsrv_connect($server, $connectionInfo);
```

PDO_SQLSRV: использование учетной записи Azure Active Directory:
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled; KeyStoreAuthentication = KeyVaultPassword; KeyStorePrincipalId = $AADUsername; KeyStoreSecret = $AADPassword;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```
С помощью идентификатора клиента приложения Azure и секрета:
```
$connectionInfo = "Database = $databaseName; ColumnEncryption = Enabled; KeyStoreAuthentication = KeyVaultClientSecret; KeyStorePrincipalId = $applicationClientID; KeyStoreSecret = $applicationClientSecret;";
$conn = new PDO("sqlsrv:server = $server; $connectionInfo", $uid, $pwd);
```

## <a name="limitations-of-the-php-drivers-when-using-always-encrypted"></a>Ограничения драйвера PHP при использовании Always Encrypted

SQLSRV и PDO_SQLSRV:
 -   Linux и macOS не поддерживают поставщика хранилища сертификатов Windows
 -   Принудительное шифрование параметров
 -   Включение Always Encrypted на уровне инструкции 
 -   При использовании функции Always Encrypted и национальных настроек, отличных от UTF8, в Linux и macOS (например, "en_US. ISO-8859-1 "), вставка данных null или пустой строки в зашифрованный столбец типа char (n) может не работать, если в системе не установлена кодовая страница 1252
 
Только SQLSRV:
 -   Использование `sqlsrv_query` для параметра Binding без указания типа SQL
 -   Использование `sqlsrv_prepare` для привязки параметров в пакете инструкций SQL  
 
Только PDO_SQLSRV:
 -   `PDO::SQLSRV_ATTR_DIRECT_QUERY`атрибут инструкции, указанный в параметризованном запросе
 -   `PDO::ATTR_EMULATE_PREPARE`атрибут инструкции, указанный в параметризованном запросе
 -   Привязка параметров в пакете инструкций SQL
 
Драйверы PHP также наследуют ограничения, установленные драйвером ODBC для SQL Server и базы данных. См. раздел [ограничения драйвера ODBC при использовании Always encrypted](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md) и [Always encrypted подробных сведений о функциях](../../relational-databases/security/encryption/always-encrypted-database-engine.md#feature-details).  
  
## <a name="see-also"></a>См. также:  
[Руководство по программированию для драйвера SQL PHP](../../connect/php/programming-guide-for-php-sql-driver.md)
[Справочник по API для драйвера SQLSRV](../../connect/php/sqlsrv-driver-api-reference.md)  
[Справочник по API драйвера PDO_SQLSRV](../../connect/php/pdo-sqlsrv-driver-reference.md)  
  

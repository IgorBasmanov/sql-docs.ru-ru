---
title: Шаг 4. Устойчивое подключение к SQL с помощью PHP | Документация Майкрософт
ms.custom: ''
ms.date: 01/22/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 8013474f-48e9-43d5-ab89-7b0504044468
author: MightyPen
ms.author: genemi
ms.openlocfilehash: b32bbb0df1c5977e814eecca7a1e6d1ddee5eca4
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67992626"
---
# <a name="step-4-connect-resiliently-to-sql-with-php"></a>Шаг 4. Выполнение устойчивого подключения к SQL с помощью PHP
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

  
Демонстрационная программа разработана таким образом, что во время попытки подключения вызывается временная ошибка (то есть любой код ошибки с префиксом "08", как указано в этом [приложении](https://docs.microsoft.com/sql/odbc/reference/appendixes/appendix-a-odbc-error-codes)) при попытке подключиться к повторной попытке. Но временная ошибка во время выполнения команды запроса приводит к тому, что программа отклоняет соединение и создает новое соединение перед повторной попыткой выполнения команды запроса. Мы не рекомендуем и не рекомендую этот вариант разработки. Демонстрационная программа иллюстрирует некоторые из доступных возможностей по проектированию.  
  
Длина этого примера кода в основном обусловлена логикой перехвата исключений.   
  
Функцию [sqlsrv_query ()](../../connect/php/sqlsrv-query.md) можно использовать для получения результирующего набора из запроса к базе данных SQL. Эта функция фактически принимает любой запрос и объект соединения и возвращает результирующий набор, который можно пробрать с помощью функции [sqlsrv_fetch_array ()](../../connect/php/sqlsrv-fetch-array.md). 
  
```php

    <?php  
        // Variables to tune the retry logic.    
        $connectionTimeoutSeconds = 30;  // Default of 15 seconds is too short over the Internet, sometimes.  
        $maxCountTriesConnectAndQuery = 3;  // You can adjust the various retry count values.  
        $secondsBetweenRetries = 4;  // Simple retry strategy.  
        $errNo = 0;  
        $serverName = "tcp:yourdatabase.database.windows.net,1433";  
        $connectionOptions = array("Database"=>"AdventureWorks",  
           "Uid"=>"yourusername", "PWD"=>"yourpassword", "LoginTimeout" => $connectionTimeoutSeconds);  
        $conn = null;  
        $arrayOfTransientErrors = array('08001', '08002', '08003', '08004', '08007', '08S01'); 
        for ($cc = 1; $cc <= $maxCountTriesConnectAndQuery; $cc++) {  
            // [A.2] Connect, which proceeds to issue a query command.  
            $conn = sqlsrv_connect($serverName, $connectionOptions);    
            if ($conn === true) {  
                echo "Connection was established";  
                echo "<br>";  
  
                $tsql = "SELECT Name FROM Production.ProductCategory";  
                $stmt = sqlsrv_query($conn, $tsql);  
                if ($stmt === false) {
                    echo "Error in query execution";  
                    echo "<br>";  
                    die(print_r(sqlsrv_errors(), true));  
                }
                while($row = sqlsrv_fetch_array($stmt, SQLSRV_FETCH_ASSOC)) {     
                    echo $row['Name'] . "<br/>" ;
                }  
                sqlsrv_free_stmt($stmt);  
                sqlsrv_close( $conn); 
                break;  
            } else {    
                // [A.4] Check whether the error code is on the whitelist of transients.  
                $isTransientError = false;  
                $errorCode = '';
                if (($errors = sqlsrv_errors()) != null) {
                    foreach ($errors as $error) {  
                        $errorCode = $error['code'];
                        $isTransientError = in_array($errorCode, $arrayOfTransientErrors);
                        if ($isTransientError) {
                            break;
                        }
                    }
                }  
                if (!$isTransientError) { 
                    // it is a static persistent error...
                    echo("Persistent error suffered with error code = $errorCode. Program will terminate.");  
                    echo "<br>";  
                    // [A.5] Either the connection attempt or the query command attempt suffered a persistent error condition.  
                    // Break the loop, let the hopeless program end.  
                    exit(0);  
                }  
                // [A.6] It is a transient error from an attempt to issue a query command.  
                // So let this method reloop and try again. However, we recommend that the new query  
                // attempt should start at the beginning and establish a new connection.  
                if ($cc >= $maxCountTriesConnectAndQuery) {  
                    echo "Transient errors suffered in too many retries - $cc. Program will terminate.";  
                    echo "<br>";  
                    exit(0);  
                }  
                echo("Transient error encountered with error code = $errorCode. Program might retry by itself.");    
                echo "<br>";  
                echo "$cc attempts so far. Might retry.";  
                echo "<br>";  
                // A very simple retry strategy, a brief pause before looping.  
                sleep(1*$secondsBetweenRetries);  
            }  
            // [A.3] All has gone well, so let the program end.  
        }  
    ?>
```

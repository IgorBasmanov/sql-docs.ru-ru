---
title: Общие сведения о поддержке SSL | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 073f3b9e-8edd-4815-88ea-de0655d0325e
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 32820e38a8292068aa95c505a04292fbac2c69af
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67916613"
---
# <a name="understanding-ssl-support"></a>Основные сведения о поддержке SSL

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

При подключении к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], если приложение запрашивает шифрование и в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] настроена поддержка шифрования SSL, то [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] инициирует SSL-подтверждение. Подтверждение обеспечивает возможность согласования сервером и клиентом алгоритмов шифрования, используемых для защиты данных. SSL-подтверждение обеспечивает защищенную передачу данных между клиентом и сервером. В ходе SSL-подтверждения сервер отправляет сертификат открытого ключа клиенту. Поставщик сертификата открытого ключа называется центром сертификации (ЦС). Клиент несет ответственность за проверку надежности используемого центра сертификации.  
  
Если приложение не запрашивает шифрование, [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] не будет требовать поддержку шифрования в [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Если в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] не настроено принудительное SSL-шифрование, подключение устанавливается без шифрования. Если в экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] настроено принудительное SSL-шифрование, драйвер автоматически включит SSL-шифрование при запуске на корректно настроенной виртуальной машине Java (JVM). В противном случае соединение будет прервано, а драйвер выдаст ошибку.  
  
> [!NOTE]  
> Для успешного установления SSL-подключения передаваемое в **serverName** значение должно в точности совпадать с общим именем (CN) или DNS-именем в альтернативном имени субъекта (SAN) сертификата сервера.  
>
> Дополнительные сведения о настройке SSL для [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] см. в разделе по шифрованию подключений к [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] в электронной документации на [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="remarks"></a>Remarks

Чтобы разрешить приложениям использовать SSL-шифрование, в [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] начиная с версии 1.2 введены следующие свойства подключений: **encrypt**, **trustServerCertificate**, **trustStore**, **trustStorePassword** и **hostNameInCertificate**. Дополнительные сведения: [Задание свойств соединения](../../connect/jdbc/setting-the-connection-properties.md).  
  
 В следующей таблице приведены режимы работы версии [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] в различных сценариях SSL-подключений. Во всех вариантах используются различные наборы свойств SSL-соединений. В таблицу входят:  
  
- **blank**: "свойство не существует в строке подключения"  
  
- **value**: "свойство существует в строке подключения и имеет допустимое значение"  
  
- **any**: "существование свойства в строке подключения и наличие у него допустимого значения не играют никакой роли"  
  
> [!NOTE]  
> Эти режимы работы также относятся к проверке подлинности пользователей [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] и встроенной проверке подлинности Windows.  
  
| encrypt        | trustServerCertificate | hostNameInCertificate | trustStore | trustStorePassword | Поведение                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| -------------- | ---------------------- | --------------------- | ---------- | ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| false or blank | any                    | any                   | any        | any                | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] не будет требовать от [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] поддержку SSL-шифрования. Если на сервере имеется самозаверяющий сертификат, драйвер запускает обмен SSL-сертификатами. SSLсертификат не будет проверяться, а шифроваться будут только учетные данные (в пакете входа).<br /><br /> Если сервер требует от клиента поддержки SSL-шифрования, драйвер запускает обмен SSL-сертификатами. Проверка SSL-сертификатов не будет выполняться, а будет выполняться шифрование всего сеанса связи.                                                                                                                                                                                    |
| true           | true                   | any                   | any        | any                | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] запрашивает использование SSL-шифрования с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Если сервер требует от клиента поддерживать SSL-шифрование или сам поддерживает шифрование, драйвер запускает обмен SSL-сертификатами. Обратите внимание, что если свойство **trustServerCertificate** имеет значение "true" (истина), драйвер не будет проверять SSL-сертификат.<br /><br /> Если на сервере не настроена поддержка шифрования, то в работе драйвера будет вызвана ошибка и соединение будет разорвано.                                                                                                                                                                                          |
| true           | false or blank         | пустой                 | пустой      | пустой              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] запрашивает использование SSL-шифрования с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Если сервер требует от клиента поддерживать SSL-шифрование или сам поддерживает шифрование, драйвер запускает обмен SSL-сертификатами.<br /><br /> Драйвер будет проверять SSL-сертификат с помощью свойства **serverName**, указанного в URL-адресе подключения, а выбор используемого хранилища сертификатов будет зависеть от правил поиска в фабрике менеджера доверия.<br /><br /> Если на сервере не настроена поддержка шифрования, то в работе драйвера будет вызвана ошибка и соединение будет разорвано.                                                                             |
| true           | false or blank         | value                 | пустой      | пустой              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] запрашивает использование SSL-шифрования с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Если сервер требует от клиента поддерживать SSL-шифрование или сам поддерживает шифрование, драйвер запускает обмен SSL-сертификатами.<br /><br /> Драйвер проверяет значение субъекта SSL-сертификата с помощью значения, указанного в свойстве **hostNameInCertificate**.<br /><br /> Если на сервере не настроена поддержка шифрования, то в работе драйвера будет вызвана ошибка и соединение будет разорвано.                                                                                                                                                                 |
| true           | false or blank         | пустой                 | value      | value              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] запрашивает использование SSL-шифрования с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Если сервер требует от клиента поддерживать SSL-шифрование или сам поддерживает шифрование, драйвер запускает обмен SSL-сертификатами.<br /><br /> Драйвер использует значение свойства **trustStore**, чтобы найти файл trustStore сертификата, и значение свойства **trustStorePassword**, чтобы проверить целостность этого файла.<br /><br /> Если на сервере не настроена поддержка шифрования, то в работе драйвера будет вызвана ошибка и соединение будет разорвано.                                                                                                                |
| true           | false or blank         | пустой                 | пустой      | value              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] запрашивает использование SSL-шифрования с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Если сервер требует от клиента поддерживать SSL-шифрование или сам поддерживает шифрование, драйвер запускает обмен SSL-сертификатами.<br /><br /> Драйвер использует значение свойства **trustStorePassword** для проверки целостности файла trustStore, выбранного по умолчанию.<br /><br /> Если на сервере не настроена поддержка шифрования, то в работе драйвера будет вызвана ошибка и соединение будет разорвано.                                                                                                                                                                                  |
| true           | false or blank         | пустой                 | value      | пустой              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] запрашивает использование SSL-шифрования с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Если сервер требует от клиента поддерживать SSL-шифрование или сам поддерживает шифрование, драйвер запускает обмен SSL-сертификатами.<br /><br /> Драйвер использует значение свойства **trustStore** для поиска файла trustStore.<br /><br /> Если на сервере не настроена поддержка шифрования, то в работе драйвера будет вызвана ошибка и соединение будет разорвано.                                                                                                                                                                                                 |
| true           | false or blank         | value                 | пустой      | value              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] запрашивает использование SSL-шифрования с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Если сервер требует от клиента поддерживать SSL-шифрование или сам поддерживает шифрование, драйвер запускает обмен SSL-сертификатами.<br /><br /> Драйвер использует значение свойства **trustStorePassword** для проверки целостности файла trustStore, выбранного по умолчанию. Драйвер также использует значение свойства **hostNameInCertificate** для проверки SSL-сертификата.<br /><br /> Если на сервере не настроена поддержка шифрования, то в работе драйвера будет вызвана ошибка и соединение будет разорвано.                                                                   |
| true           | false or blank         | value                 | value      | пустой              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] запрашивает использование SSL-шифрования с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Если сервер требует от клиента поддерживать SSL-шифрование или сам поддерживает шифрование, драйвер запускает обмен SSL-сертификатами.<br /><br /> Драйвер использует значение свойства **trustStore** для поиска файла trustStore. Драйвер также использует значение свойства **hostNameInCertificate** для проверки SSL-сертификата.<br /><br /> Если на сервере не настроена поддержка шифрования, то в работе драйвера будет вызвана ошибка и соединение будет разорвано.                                                                                  |
| true           | false or blank         | value                 | value      | value              | [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] запрашивает использование SSL-шифрования с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> Если сервер требует от клиента поддерживать SSL-шифрование или сам поддерживает шифрование, драйвер запускает обмен SSL-сертификатами.<br /><br /> Драйвер использует значение свойства **trustStore**, чтобы найти файл trustStore сертификата, и значение свойства **trustStorePassword**, чтобы проверить целостность этого файла. Драйвер также использует значение свойства **hostNameInCertificate** для проверки SSL-сертификата.<br /><br /> Если на сервере не настроена поддержка шифрования, то в работе драйвера будет вызвана ошибка и соединение будет разорвано. |
  
Если для свойства "encrypt" задано значение **true**, то [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] использует настроенный по умолчанию поставщик безопасности JSSE в виртуальной машине Java, чтобы согласовать SSL-шифрование с [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Поставщик безопасности по умолчанию может не поддерживать все функции, необходимые для успешного согласования SSL-шифрования. Например, поставщик безопасности по умолчанию может не поддерживать размер открытого ключа RSA, используемого в SSL-сертификате [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. В этом случае поставщик безопасности по умолчанию может вызвать ошибку, в результате которой драйвер JDBC разорвет соединение. Чтобы устранить эту неполадку, выполните одно из следующих действий.  
  
- Настройте [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] с сертификатом сервера, имеющим открытый ключ RSA меньшего размера  
  
- Настройте в виртуальной машине Java другой поставщик безопасности JSSE в файле свойств безопасности "\<java-home>/lib/security/java.security"  
  
- Используйте другой JVM  
  
## <a name="validating-server-ssl-certificate"></a>Проверка SSL-сертификата сервера  

В ходе SSL-подтверждения сервер отправляет сертификат открытого ключа клиенту. Драйвер JDBC или клиент должны обеспечить проверку того, что сертификат сервера выдан центром сертификации, которому доверяет клиент. Драйвер требует соответствия сертификата сервера следующим требованиям:  
  
- Сертификат должен быть выдан доверенным центром сертификации.  
  
- Сертификат должен быть выдан для проверки подлинности серверов.  
  
- Сертификат является действительным на текущую дату.  
  
- Общее имя (CN) в субъекте или DNS-имя в альтернативном имени субъекта (SAN) сертификата точно соответствует значению **serverName**, указанному в строке подключения, или значению свойства **hostNameInCertificate**, если оно задано.  
  
- DNS-имя может содержать символы шаблонов. Однако [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] не поддерживает сопоставление с подстановочными знаками. Это значит, что abc.com не будет соответствовать \*.com, но \*.com будет соответствовать \*.com.  
  
## <a name="see-also"></a>См. также:

[Использование SSL-шифрования](../../connect/jdbc/using-ssl-encryption.md)

[Защита приложений драйвера JDBC](../../connect/jdbc/securing-jdbc-driver-applications.md)  

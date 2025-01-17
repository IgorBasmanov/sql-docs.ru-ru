---
title: Шифрование соединений с SQL Server на Linux
description: Эта статья описывает шифрование соединений с SQL Server на Linux.
ms.date: 01/30/2018
author: vin-yu
ms.author: vinsonyu
ms.reviewer: vanto
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
helpviewer_keywords:
- Linux, encrypted connections
ms.openlocfilehash: 3f658ba8723b142f37763ea8b4f0c8f7b0c5d0e1
ms.sourcegitcommit: db9bed6214f9dca82dccb4ccd4a2417c62e4f1bd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68077288"
---
# <a name="encrypting-connections-to-sql-server-on-linux"></a>Шифрование соединений с SQL Server на Linux

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-linuxonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-linuxonly.md)]

[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] на Linux может использовать протокол TLS для шифрования данных, передаваемых по сети между клиентским приложением и экземпляром [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] поддерживает одни и те же протоколы TLS в Windows и Linux: TLS 1.2, 1.1 и 1.0. Однако действия по настройке TLS зависят от конкретной операционной системы, где выполняется [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  

## <a name="requirements-for-certificates"></a>Требования к сертификатам 
Прежде чем приступить к работе, нужно убедиться, что сертификаты соответствуют следующим требованиям.
- Текущее системное время должно превышать значение свойства сертификата "Действителен с" и не превышать значение свойства "Действителен до".
- Сертификат должен быть предназначен для проверки подлинности сервера. Для этого требуется задать свойству сертификата "Улучшенный ключ" значение "Проверка подлинности сервера (1.3.6.1.5.5.7.3.1)".
- Сертификат должен быть создан с помощью параметра KeySpec свойства AT_KEYEXCHANGE. Обычно в свойстве применения ключа сертификата (KEY_USAGE) также предусмотрено шифрование ключа (CERT_KEY_ENCIPHERMENT_KEY_USAGE).
- Свойство "Субъект" сертификата должно указывать, что общее имя (CN) совпадает с именем узла или полным доменным именем (FQDN) сервера. Примечание. Поддерживаются групповые сертификаты.

## <a name="configuring-the-openssl-libraries-for-use-optional"></a>Настройка библиотек OpenSSL для использования (необязательно)
В `/opt/mssql/lib/` каталоге можно создать символьные ссылки, указывающие, какие библиотеки `libcrypto.so` и `libssl.so` следует использовать для шифрования. Это удобно, если нужно заставить SQL Server использовать определенную версию OpenSSL, отличную от предоставленной системой для использования по умолчанию. Если эти символьные ссылки отсутствуют, SQL Server загрузит настроенные в системе библиотеки OpenSSL по умолчанию.

Эти символьные ссылки должны называться `libcrypto.so` и `libssl.so` и находиться в каталоге `/opt/mssql/lib/`.

## <a name="overview"></a>Обзор
Протокол TLS используется для шифрования подключений от клиентского приложения к [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. При правильной настройке TLS обеспечивает как конфиденциальность, так и целостность данных при взаимодействии клиента и сервера.  Подключения TLS могут инициироваться клиентом или сервером. 

## <a name="client-initiated-encryption"></a>Шифрование, инициированное клиентом 
- **Создание сертификата** (значение /CN должно соответствовать полному доменному имени вашего узла SQL Server)

> [!NOTE]
> В этом примере используется самозаверяющий сертификат, который не следует использовать в рабочих сценариях. Нужно использовать сертификаты ЦС. 

        openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
        sudo chown mssql:mssql mssql.pem mssql.key 
        sudo chmod 600 mssql.pem mssql.key   
        sudo mv mssql.pem /etc/ssl/certs/ 
        sudo mv mssql.key /etc/ssl/private/ 

- **Настройка SQL Server**

        systemctl stop mssql-server 
        cat /var/opt/mssql/mssql.conf 
        sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssql.pem 
        sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssql.key 
        sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
        sudo /opt/mssql/bin/mssql-conf set network.forceencryption 0 

- **Регистрация сертификата на клиентском компьютере (Windows, Linux или macOS)**

    -   Если вы используете сертификат, подписанный центром сертификации, на клиентский компьютер нужно скопировать сертификат центра сертификации (ЦС), а не сертификат пользователя. 
    -   Если вы используете самозаверяющий сертификат, просто скопируйте PEM-файл в следующие папки, соответствующие используемому дистрибутиву, и выполните команды, чтобы включить их. 
        - **Ubuntu**: скопируйте сертификат в ```/usr/share/ca-certificates/```, переименуйте расширение на .crt и используйте dpkg-reconfigure ca-certificates, чтобы включить его в качестве системного сертификата ЦС. 
        - **RHEL**: скопируйте сертификат в ```/etc/pki/ca-trust/source/anchors/```, используйте ```update-ca-trust```, чтобы включить его в качестве системного сертификата ЦС.
        - **SUSE**: скопируйте сертификат в ```/usr/share/pki/trust/anchors/```, используйте ```update-ca-certificates```, чтобы включить его в качестве системного сертификата ЦС.
        - **Windows**:  импортируйте PEM-файл в качестве сертификата в раздел "Текущий пользователь" -> "Доверенные корневые центры сертификации" -> "Сертификаты".
        - **macOS**: 
           - скопируйте сертификат в ```/usr/local/etc/openssl/certs```.
           - Выполните следующую команду, чтобы получить хэш-значение: ```/usr/local/Cellar/openssql/1.0.2l/openssql x509 -hash -in mssql.pem -noout```.
           - Переименуйте сертификат в value. Например: ```mv mssql.pem dc2dd900.0```. Убедитесь, что dc2dd900.0 указано в ```/usr/local/etc/openssl/certs```.
    
-   **Примеры строк подключения** 

    - **[!INCLUDE[ssmanstudiofull-md](../includes/ssmanstudiofull-md.md)]**   
  ![Диалоговое окно подключения SSMS](media/sql-server-linux-encrypted-connections/ssms-encrypt-connection.png "Диалоговое окно подключения SSMS")  
  
    - **SQLCMD** 

            sqlcmd  -S <sqlhostname> -N -U sa -P '<YourPassword>' 
    - **ADO.NET** 

            "Encrypt=True; TrustServerCertificate=False;" 
    - **интерфейс ODBC** 

            "Encrypt=Yes; TrustServerCertificate=no;" 
    - **JDBC** 
    
            "encrypt=true; trustServerCertificate=false;" 

## <a name="server-initiated-encryption"></a>Шифрование, инициированное сервером 

- **Создание сертификата** (значение /CN должно соответствовать полному доменному имени вашего узла SQL Server)
        
        openssl req -x509 -nodes -newkey rsa:2048 -subj '/CN=mssql.contoso.com' -keyout mssql.key -out mssql.pem -days 365 
        sudo chown mssql:mssql mssql.pem mssql.key 
        sudo chmod 600 mssql.pem mssql.key   
        sudo mv mssql.pem /etc/ssl/certs/ 
        sudo mv mssql.key /etc/ssl/private/ 

- **Настройка SQL Server**

        systemctl stop mssql-server 
        cat /var/opt/mssql/mssql.conf 
        sudo /opt/mssql/bin/mssql-conf set network.tlscert /etc/ssl/certs/mssql.pem 
        sudo /opt/mssql/bin/mssql-conf set network.tlskey /etc/ssl/private/mssql.key 
        sudo /opt/mssql/bin/mssql-conf set network.tlsprotocols 1.2 
        sudo /opt/mssql/bin/mssql-conf set network.forceencryption 1 
        
-   **Примеры строк подключения** 

    - **SQLCMD**

            sqlcmd  -S <sqlhostname> -U sa -P '<YourPassword>' 
    - **ADO.NET** 

            "Encrypt=False; TrustServerCertificate=False;" 
    - **интерфейс ODBC** 

            "Encrypt=no; TrustServerCertificate=no;"  
    - **JDBC** 
    
            "encrypt=false; trustServerCertificate=false;" 
            
> [!NOTE]
> Задайте для **TrustServerCertificate** значение true, если клиенту не удается подключиться к ЦС для проверки подлинности сертификата.

## <a name="common-connection-errors"></a>Распространенные ошибки при подключении  

|Сообщение об ошибке |Fix |
|--- |--- |
|Цепочка сертификатов не была выпущена доверенным центром сертификации.  |Эта ошибка возникает, когда клиенты не могут проверить подпись сертификата, представленного SQL Server во время подтверждения TLS. Убедитесь, что клиент доверяет напрямую сертификату [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] или доверяет ЦС, подписавшему этот сертификат SQL Server. |
|Неправильное имя целевого субъекта.  |Убедитесь, что значение в поле общего имени в сертификате SQL Server соответствует имени сервера, указанному в строке подключения клиента. |  
|существующее соединение было принудительно завершено удаленным узлом. |Эта ошибка может возникать, если клиент не поддерживает версию протокола TLS, необходимую SQL Server. Например, если параметр [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] настроен для использования TLS 1.2, убедитесь, что клиенты также поддерживают протокол TLS 1.2. |
| | |   

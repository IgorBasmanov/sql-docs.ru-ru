---
title: Защита строк подключения | Документация Майкрософт
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 69ce8557-5260-4ea4-81b8-d0c5481f0868
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 05782c2691b3409339554e2154b385b93c17b4d9
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: MTE75
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67945788"
---
# <a name="securing-connection-strings"></a>Обеспечение безопасности строк подключений

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Защита доступа к источнику данных — одна из основных целей содействия защите приложения. Для ограничения доступа к источнику данных необходимо принять меры предосторожности, чтобы помочь защитить соединение, включая идентификатор, пароль и имя источника данных. Хранение пользовательского идентификатора и пароля в обычном тексте, таком как исходный код, создает серьезную проблему безопасности. Даже если поставляется скомпилированная версия текста, которая содержит идентификатор и пароль во внешнем источнике, скомпилированный код потенциально может быть дизассемблирован, а пользовательский идентификатор и пароль будут рассекречены. Поэтому важно, чтобы такая важная информация, как идентификатор и пароль пользователя, не присутствовали бы в коде.

Рекомендуем не хранить пароль вместе с URL-адресом подключения в исходном коде приложения. Вместо этого храните пароль в отдельном файле с ограниченным доступом. Доступ к тому файлу можно предоставить контексту, под которым работает приложение.

Другой способ — хранить в файле зашифрованный пароль. Удостоверьтесь, что используется шифрование API, не требующее, чтобы ключ был сохранен где-нибудь, и не полученное из пароля пользователя. Например, можно рассмотреть пары открытый/закрытый ключ на основе сертификатов, или использовать подход, где две стороны используют протокол соглашения о ключах (алгоритм Диффи-Хеллмана), чтобы сформировать идентичные секретные ключи для шифрования без необходимости передавать секретный ключ.

Если сведения о строках соединения берутся из внешнего источника, такого как пользователь, вводящий свой идентификатор и пароль, необходимо проверить любые введенные сведения из этого источника, чтобы гарантировать, что у них формат правильный и не содержит дополнительных параметров, действующих на соединение.

## <a name="see-also"></a>См. также:

[Защита приложений драйвера JDBC](../../connect/jdbc/securing-jdbc-driver-applications.md)

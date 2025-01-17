---
title: Загрузите и установите
titleSuffix: Azure Data Studio
description: Скачивание и установка Azure Data Studio для Windows, macOS или Linux
ms.prod: sql
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.custom: seodec18
ms.date: 07/11/2019
ms.reviewer: alayu; sstein
ms.openlocfilehash: ff212d44fc16ad4a8c6366eda88d92fa78f20d84
ms.sourcegitcommit: 495913aff230b504acd7477a1a07488338e779c6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2019
ms.locfileid: "68811220"
---
# <a name="download-and-install-azure-data-studio"></a>Скачивание и установка Azure Data Studio

[!INCLUDE[name-sos](../includes/name-sos.md)] выполняется в Windows, macOS и Linux.


Скачайте и установите последний, *июльский выпуск*:

> [!NOTE]
> При обновлении с SQL Operations Studio и необходимости сохранения параметров, сочетаний клавиш или фрагментов кода см. раздел [Перемещение параметров пользователя](#move-user-settings).

|Платформа|Загрузить|Дата выпуска| Версия |
|:---|:---|:---|:---|
|Windows|[Пользовательский установщик (рекомендуется)](https://go.microsoft.com/fwlink/?linkid=2098449)<br>[Системный установщик](https://go.microsoft.com/fwlink/?linkid=2098450)<br>[ZIP](https://go.microsoft.com/fwlink/?linkid=2098500)|11 июля 2019 г. |1.9.0|
|macOS|[ZIP](https://go.microsoft.com/fwlink/?linkid=2098501)|11 июля 2019 г. |1.9.0|
|Linux|[.deb](https://go.microsoft.com/fwlink/?linkid=2098279)<br>[.rpm](https://go.microsoft.com/fwlink/?linkid=2098280)<br>[.tar.gz](https://go.microsoft.com/fwlink/?linkid=2098197)|11 июля 2019 г. |1.9.0|

Подробнее см. в [заметках о выпуске](release-notes.md).

## <a name="get-azure-data-studio-for-windows"></a>Получение Azure Data Studio для Windows

Этот выпуск [!INCLUDE[name-sos](../includes/name-sos-short.md)] включает стандартные средства установщика Windows и ZIP-файл.

Рекомендуется использовать *пользовательский установщик*, так как он не требует прав администратора, что упрощает установку и обновление. Он не требует прав администратора, так как расположение находится в пользовательской локальной папке AppData (LOCALAPPDATA). Пользовательский установщик также обеспечивает более гладкую фоновую работу по обновлению. Дополнительные сведения см. в статье [Пользовательская настройка в Windows](https://code.visualstudio.com/updates/v1_26#_user-setup-for-windows).

**Пользовательский установщик** (рекомендуется)

1. Скачайте и запустите [*пользовательский* установщик [!INCLUDE[name-sos](../includes/name-sos-short.md)] для Windows](https://go.microsoft.com/fwlink/?linkid=2098449).
2. Запустите приложение [!INCLUDE[name-sos-short](../includes/name-sos-short.md)].

**Системный установщик**

1. Скачайте и запустите [*пользовательский* установщик [!INCLUDE[name-sos](../includes/name-sos-short.md)] для Windows](https://go.microsoft.com/fwlink/?linkid=2098450 ).
2. Запустите приложение [!INCLUDE[name-sos-short](../includes/name-sos-short.md)].

**ZIP-файл**

1. Скачайте [ZIP-файл [!INCLUDE[name-sos](../includes/name-sos-short.md)] для Windows](https://go.microsoft.com/fwlink/?linkid=2098500).
2. Перейдите к скачанному файлу и извлеките его содержимое.
3. Выполнить `\azuredatastudio-windows\azuredatastudio.exe`


## <a name="get-azure-data-studio-for-macos"></a>Получение Azure Data Studio для macOS

1. Скачайте [[!INCLUDE[name-sos](../includes/name-sos-short.md)] для macOS](https://go.microsoft.com/fwlink/?linkid=2098501).
2. Чтобы извлечь содержимое ZIP-файла, дважды щелкните его.
3. Чтобы сделать [!INCLUDE[name-sos](../includes/name-sos-short.md)] доступным на *панели запуска*, перетащите *Azure Data Studio.app* в папку *Приложения*.


## <a name="get-azure-data-studio-for-linux"></a>Получение Azure Data Studio для Linux

1. Скачайте [!INCLUDE[name-sos](../includes/name-sos-short.md)] для Linux с помощью одного из установщиков или архива tar.gz:
    - [.deb](https://go.microsoft.com/fwlink/?linkid=2098279)
    - [.rpm](https://go.microsoft.com/fwlink/?linkid=2098280)
    - [.tar.gz](https://go.microsoft.com/fwlink/?linkid=2098197)
1. Чтобы извлечь файл и запустить [!INCLUDE[name-sos](../includes/name-sos-short.md)], откройте новое окно терминала и введите следующие команды:

   **Установка в Debian:**
   ```bash
   cd ~
   sudo dpkg -i ./Downloads/azuredatastudio-linux-<version string>.deb

   azuredatastudio
   ```

   **Установка из RPM:**
   ```bash
   cd ~
   yum install ./Downloads/azuredatastudio-linux-<version string>.rpm

   azuredatastudio
   ```

   **Установка из tar.gz:**
   ```bash 
   cd ~ 
   cp ~/Downloads/azuredatastudio-linux-<version string>.tar.gz ~ 
   tar -xvf ~/azuredatastudio-linux-<version string>.tar.gz 
   echo 'export PATH="$PATH:~/azuredatastudio-linux-x64"' >> ~/.bashrc
   source ~/.bashrc 
   azuredatastudio 
   ``` 

   > [!NOTE]
   > В Debian, Redhat и Ubuntu, возможно, будут отсутствовать некоторые зависимости. Чтобы установить эти зависимости с учетом вашей версии Linux, используйте следующие команды:
   

   **Debian:** 
   ```bash
   sudo apt-get install libunwind8
   ```

   **Redhat:** 
   ```bash
   yum install libXScrnSaver
   ```

   **Ubuntu:** 
   ```bash
   sudo apt-get install libxss1

   sudo apt-get install libgconf-2-4

   sudo apt-get install libunwind8
   ```
## <a name="download-insiders-build-of-azure-data-studio"></a>Скачивание сборки Azure Data Studio для инсайдеров
Как правило, пользователям следует скачивать стабильный выпуск Azure Data Studio выше. Однако если вы хотите испытать наши бета-версии функций и поделиться своими отзывами, вы можете скачать [сборку для участников программы предварительной оценки Azure Data Studio](https://github.com/microsoft/azuredatastudio#try-out-the-latest-insiders-build-from-master).

## <a name="uninstall-azure-data-studio"></a>Удаление Azure Data Studio

Если вы установили [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] с помощью установщика Windows, удаление выполняется так же, как и для любого приложения Windows.

Если вы установили [!INCLUDE[name-sos-short](../includes/name-sos-short.md)] с помощью ZIP-файла или другого архива, просто удалите файлы.

## <a name="supported-operating-systems"></a>Поддерживаемые операционные системы

Программа [!INCLUDE[name-sos](../includes/name-sos-short.md)] работает в Windows, macOS и Linux, а также поддерживается на следующих платформах:

### <a name="windows"></a>Windows
- Windows 10 (64-разрядная)
- Windows 8.1 (64-разрядная)
- Windows 8 (64-разрядная)
- Windows 7 с пакетом обновления 1 (SP1) (64-разрядная) — требуется [KB2533623](https://www.microsoft.com/download/details.aspx?id=26767)
- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2 (64-разрядная версия)
- Windows Server 2012 (64-разрядная версия)
- Windows Server 2008 R2 (64-разрядная версия)

### <a name="macos"></a>macOS
- macOS 10.13 High Sierra
- macOS 10.12 Sierra

### <a name="linux"></a>Linux
- Red Hat Enterprise Linux 7.4
- Red Hat Enterprise Linux 7.3
- SUSE Linux Enterprise Server версии 12 с пакетом обновления 2 (SP2)
- Ubuntu 16.04

## <a name="recommended-system-requirements"></a>Рекомендованные требования к системе

|             | Ядра ЦП | Память (ОЗУ) |
|:-----------|:---------|:----------|
| Рекомендуемая |     4     |      8 ГБ    |
|   Минимальные   |     2     |      4 ГБ     |
|             |           |            |

## <a name="check-for-updates"></a>Проверка обновлений
Чтобы проверить наличие последних обновлений, щелкните значок шестеренки в левом нижнем углу окна и нажмите кнопку **Проверить наличие обновлений.**

## <a name="supported-sql-offerings"></a>Поддерживаемые предложения SQL

* Эта версия Azure Data Studio работает со всеми [поддерживаемыми версиями SQL Server 2014 — [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)]](https://support.microsoft.com/lifecycle?C2=1044) и обеспечивает поддержку новейших облачных функций базы данных SQL Azure и хранилища данных SQL Azure. Azure Data Studio также предоставляет предварительную поддержку для управляемых экземпляров SQL Azure.

## <a name="upgrade-from-sql-operations-studio"></a>Обновление с SQL Operations Studio

Если вы по-прежнему используете SQL Operations Studio, необходимо выполнить обновление до Azure Data Studio. SQL Operations Studio — это предварительное имя предварительной версии Azure Data Studio. В сентябре 2018 г. мы [изменили имя на Azure Data Studio](https://cloudblogs.microsoft.com/sqlserver/2018/09/25/azure-data-studio-for-sql-server/) и выпустили общедоступную версию. Поскольку SQL Operations Studio больше не обновляется и не поддерживается, мы просим всех пользователей SQL Operations Studio скачать последнюю версию Azure Data Studio, чтобы получить новейшие функции, обновления для системы безопасности и исправления.
 
При обновлении старой предварительной версии до последней Azure Data Studio будут потеряны текущие параметры и расширения. Чтобы переместить параметры, следуйте инструкциям в разделе *Перемещение параметров пользователя*.


## <a name="move-user-settings"></a>Перемещение параметров пользователя

Если вы хотите переместить пользовательские параметры, сочетания клавиш или фрагменты кода, выполните следующие действия. Это важно при обновлении с SQL Operations Studio до Azure Data Studio.

*Если у вас уже есть Azure Data Studio или вы никогда не устанавливали или не настраивали SQL Operations Studio, этот раздел можно пропустить*.


1. Откройте параметры, щелкнув шестеренку в левом нижнем углу и выбрав **Параметры**.

   ![open-settings](./media/download/open-settings.png)

2. Щелкните правой кнопкой мыши вкладку **Параметры пользователя** вверху и выберите пункт **Показать в проводнике**.

   ![reveal-in-explorer](./media/download/reveal-in-explorer.png)

3. Скопируйте все файлы в этой папке и сохраните их в удобном для поиска расположении на локальном диске, например в папке "Документы".

   ![copy-settings](./media/download/copy-settings.png)

4. В новой версии Azure Data Studio выполните шаги 1–2, а затем на шаге 3 вставьте сохраненное содержимое в папку. Вы также можете вручную скопировать параметры, сочетания клавиш или фрагменты в соответствующие места.

5. При замене существующей установки удалите старый каталог установки перед установкой, чтобы избежать ошибок при подключении к учетной записи Azure для обозревателя ресурсов.

## <a name="next-steps"></a>Next Steps

Чтобы приступить к работе, ознакомьтесь со следующими краткими руководствами.
- [Подключение и отправка запроса к SQL Server](quickstart-sql-server.md)
- [Подключение и отправка запроса к базе данных SQL Azure](quickstart-sql-database.md)
- [Подключение и отправка запроса к хранилищу данных Azure](quickstart-sql-dw.md)

[!INCLUDE[get-help-sql-tools](../includes/paragraph-content/get-help-sql-tools.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

[Заявление о конфиденциальности Майкрософт](https://go.microsoft.com/fwlink/?LinkId=521839) и [Сбор данных об использовании](usage-data-collection.md).

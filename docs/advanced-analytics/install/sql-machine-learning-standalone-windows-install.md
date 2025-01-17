---
title: Установка R Server или Machine Learning Server (изолированной) с помощью программы установки SQL Server
description: Настройте изолированный сервер машинного обучения, не поддерживающий экземпляры, для разработки R и Python с помощью RevoScaleR, revoscalepy, MicrosoftML и других пакетов.
ms.prod: sql
ms.technology: machine-learning
ms.date: 08/28/2018
ms.topic: conceptual
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 94ca7b3646b9005e11b3ee4968cbfaaa65d42264
ms.sourcegitcommit: 321497065ecd7ecde9bff378464db8da426e9e14
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68715840"
---
# <a name="install-machine-learning-server-standalone-or-r-server-standalone-using-sql-server-setup"></a>Установка Machine Learning Server (изолированной) или сервера R Server (автономная версия) с помощью программы установки SQL Server
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

::: moniker range=">=sql-server-2017||=sqlallproducts-allversions"
SQL Server программа установки включает параметр **общего компонента** для установки автономного сервера машинного обучения, не поддерживающего экземпляры, который работает вне SQL Server. Он называется **Machine Learning Server (автономный)** и включает R и Python. 
::: moniker-end
::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
SQL Server программа установки включает параметр **общего компонента** для установки автономного сервера машинного обучения, не поддерживающего экземпляры, который работает вне SQL Server. В SQL Server 2016 эта функция называется **R Server (изолированная)** .  
::: moniker-end

Изолированный сервер, установленный SQL Server установки, функционально эквивалентен версиям [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server), не относящимся к SQL, с поддержкой тех же сценариев использования и скриптов, в том числе:

+ Удаленное выполнение, переключение между локальными и удаленными сеансами в одной консоли
+ Эксплуатация с помощью веб-узлов и вычислений на узлах
+ Развертывание веб-службы: возможность упаковать скрипты R и Python в веб-службы
+ Полный набор библиотек функций R и Python

В качестве независимого сервера, отделенного от SQL Server, среда R и Python настраивается, защищена и доступна с помощью базовой операционной системы и средств, предоставляемых на отдельном сервере, но не SQL Server.

Как дополнения SQL Server, автономный сервер полезен, если необходимо разработать высокопроизводительные решения машинного обучения, которые могут использовать удаленные контексты вычислений в полном диапазоне поддерживаемых платформ данных. Вы можете переместить выполнение с локального сервера на удаленный Machine Learning Server в кластере Spark или на другом экземпляре SQL Server.

<a name="bkmk_prereqs"> </a>

## <a name="pre-install-checklist"></a>Контрольный список перед установкой

Если вы установили предыдущую версию, например SQL Server 2016 R Server (изолированная версия) или Microsoft R Server, удалите существующую установку, прежде чем продолжить.

В качестве общего правила рекомендуется рассматривать изолированные установки сервера и ядра СУБД как взаимоисключающие, чтобы избежать конфликтов ресурсов, но если у вас достаточно ресурсов, нет запрета на установку этих экземпляров в тот же физический компьютер.

На компьютере может быть только один изолированный сервер: либо SQL Server Machine Learning Server (автономный), либо SQL Server R Server (изолированный). Перед добавлением новой версии обязательно удалите одну из них.

::: moniker range="=sql-server-2016"
<a name="bkmk_ga_instalpatch"></a> 

 ###  <a name="install-patch-requirement"></a>Требование установки исправления 

Только для SQL Server 2016: Корпорация Майкрософт выявила проблему с определенной версией двоичных файлов среды выполнения Microsoft VC++ 2013, которые SQL Server устанавливает в качестве необходимого компонента. Если это обновление двоичных файлов среды выполнения VC не установлено, в SQL Server могут возникать проблемы с надежностью в определенных сценариях. Перед установкой SQL Server выполните инструкции, приведенные в [заметках о выпуске SQL Server](../../sql-server/sql-server-2016-release-notes.md#bkmk_ga_instalpatch), чтобы узнать, требуется ли на вашем компьютере исправление для двоичных файлов среды выполнения VC.  
::: moniker-end

## <a name="get-the-installation-media"></a>Получение установочного носителя

[!INCLUDE[GetInstallationMedia](../../includes/getssmedia.md)]

::: moniker range=">=sql-server-2017||=sqlallproducts-allversions"
## <a name="run-setup"></a>Запуск программы установки

Для локальных установок необходимо запускать программу установки с правами администратора. При установке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из удаленной общей папки необходимо использовать учетную запись домена с разрешениями на чтение и выполнение для удаленной общей папки.

1. Запустите мастер установки.

2. Перейдите на вкладку **Установка** и выберите пункт **Новая Machine Learning Server (изолированная) Установка**.
    
     ![Установка автономного Machine Learning Server](media/2017setup-installation-page-mlsvr.png "Запуск установки автономного Machine Learning Server")

3. После завершения проверки правил примите условия лицензионного соглашения SQL Server и выберите новую установку.

4. На странице **Выбор компонентов** уже должны быть выбраны следующие параметры.

    - Microsoft Machine Learning Server (изолированный)

    - R и Python выбираются по умолчанию. Можно снять выделение с любого языка, но рекомендуется установить хотя бы один из поддерживаемых языков.

     ![Выбор компонентов R или Python](media/2017setup-features-page-mlsvr-rpy.png "Запуск установки автономного Machine Learning Server")
    
    Все прочие параметры нужно игнорировать. 
    
    > [!NOTE]
    > Не устанавливайте **Общие компоненты** , если на компьютере уже установлен Службы машинного обучения для SQL Server аналитики в базе данных. При этом создаются дубликаты библиотек.
    > 
    > Кроме того, в то время как скрипты R или Python, выполняемые в SQL Server, управляются SQL Server поэтому не конфликтуют с памятью, используемой другими службами ядра СУБД, изолированный сервер машинного обучения не имеет таких ограничений и может конфликтовать с другими операциями базы данных. . Наконец, удаленный доступ через сеанс RDP, который часто используется для операционной работы, обычно блокируется администраторами базы данных.
    > 
    > По этим причинам рекомендуется устанавливать Machine Learning Server (автономный) на отдельном компьютере с SQL Server Службы машинного обучения.

5.  Примите условия лицензионного соглашения для загрузки и установки базовых языковых распределений. Когда кнопка **Принять** станет недоступна, можно нажать кнопку **Далее**. 

     ![Лицензионное соглашение Python](media/2017setup-python-license.png "Лицензионное соглашение Python")

6.  На странице **Все готово для установки** проверьте выбранные параметры и нажмите кнопку **Установить**.

После завершения установки ознакомьтесь со статьей [пользовательские отчеты для SQL Server R Services](../r/monitor-r-services-using-custom-reports-in-management-studio.md) , чтобы получить справку по ошибкам и предупреждениям, см. [вопросы и ответы по обновлению и установке — службы машинного обучения](../r/upgrade-and-installation-faq-sql-server-r-services.md).
::: moniker-end

::: moniker range="=sql-server-2016"
## <a name="run-setup"></a>Запуск программы установки

Для локальных установок необходимо запускать программу установки с правами администратора. При установке [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] из удаленной общей папки необходимо использовать учетную запись домена с разрешениями на чтение и выполнение для удаленной общей папки.

1. Запустите мастер установки.

2. На вкладке **Установка** щелкните **Новая установка R Server (изолированная)** .
    
     ![Запустить автономную установку R Server](media/2016-setup-installation-rsvr.png "Запустить автономную установку R Server")

3. После завершения проверки правил примите условия лицензионного соглашения SQL Server и выберите новую установку.

4.  На странице **Выбор компонентов** уже должен быть выбран следующий параметр:
    
    **R Server (изолированный)**  
    
    ![Варианты выбора компонентов для сервера R Server в автономном] режиме (media/2016setup-rserver-features.png "Варианты выбора компонентов для сервера R Server в автономном") режиме
    
    Все прочие параметры можно игнорировать. 
    
    > [!NOTE]
    > Избегайте установки **общих компонентов** при запуске программы установки на компьютере, где уже установлены службы R для SQL Server аналитики в базе данных. При этом создаются дубликаты библиотек.
    > 
    > В то время как скрипты R, выполняемые в SQL Server, управляются SQL Server поэтому не конфликтуют с памятью, используемой другими службами ядра СУБД, изолированный сервер R не имеет таких ограничений и может помешать работе других операций с базой данных.
    > 
    > Обычно рекомендуется устанавливать сервер R Server (изолированный) на отдельном компьютере из SQL Server R Services (в базе данных).

5.  Примите условия лицензионного соглашения для загрузки и установки базовых языковых распределений. Когда кнопка **Принять** станет недоступна, можно нажать кнопку **Далее**. 

6.  На странице **Все готово для установки** проверьте выбранные параметры и нажмите кнопку **Установить**.

После завершения установки ознакомьтесь со статьей [пользовательские отчеты для SQL Server R Services](../r/monitor-r-services-using-custom-reports-in-management-studio.md) , чтобы получить справку по ошибкам и предупреждениям, см. [вопросы и ответы по обновлению и установке — службы машинного обучения](../r/upgrade-and-installation-faq-sql-server-r-services.md).
::: moniker-end

## <a name="set-environment-variables"></a>Задание переменных среды

Для интеграции функций R необходимо задать переменную среды **MKL_CBWR** , чтобы [обеспечить последовательный вывод данных](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr) из вычислений Intel Math Kernel Library (MKL).

1. На панели управления щелкните **система и система безопасности** >  > **Дополнительные** > **переменные среды**системы.

2. Создайте новую пользовательскую или системную переменную. 

  + Задайте для переменной имя`MKL_CBWR`
  + Присвойте переменной значение`AUTO`

3. Перезапустите сервер.

<a name="install-path"></a>

### <a name="default-installation-folders"></a>Папки установки по умолчанию

Для разработки на языке R и Python часто приходится иметь несколько версий на одном компьютере. В соответствии с установленной программой установки SQL Server базовое распределение устанавливается в папку, связанную с версией SQL Server, которая использовалась для установки.

В следующей таблице перечислены пути для дистрибутивов R и Python, созданных установщиками Майкрософт. Для полноты таблица содержит пути, созданные программой установки SQL Server, а также автономный установщик для Microsoft Machine Learning Server.

|Version| Метод установки | Папка по умолчанию|
|----|----|----|
|Machine Learning Server SQL Server 2017 (автономная) |  Мастер установки SQL Server 2017 |`C:\Program Files\Microsoft SQL Server\140\R_SERVER` <br/>`C:\Program Files\Microsoft SQL Server\140\PYTHON_SERVER`|
|Microsoft Machine Learning Server (изолированный) |  Автономный установщик Windows |`C:\Program Files\Microsoft\ML Server\R_SERVER`<br/>`C:\Program Files\Microsoft\ML Server\PYTHON_SERVER`|
|Службы машинного обучения SQL Server (в базе данных) |Мастер установки SQL Server 2017 с параметром языка R|`C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\R_SERVICES`  <br/>`C:\Program Files\Microsoft SQL Server\MSSQL14.<instance_name>\PYTHON_SERVICES` |
|SQL Server 2016 R Server (изолированный) |  Мастер установки SQL Server 2016 |`C:\Program Files\Microsoft SQL Server\130\R_SERVER`|
|Службы R SQL Server 2016 (в базе данных) |Мастер установки SQL Server 2016|`C:\Program Files\Microsoft SQL Server\MSSQL13.<instance_name>\R_SERVICES`|

<a name="apply-cu"></a>

## <a name="apply-updates"></a>Применить обновления

Мы рекомендуем применить Последнее накопительное обновление к компонентам ядра СУБД и машинного обучения. Накопительные обновления устанавливаются с помощью программы установки. 

На устройствах, подключенных к Интернету, можно скачать самораспаковывающийся исполняемый файл. При применении обновления для ядра СУБД автоматически запрашиваются накопительные обновления для существующих функций R и Python. 

На отключенных серверах требуются дополнительные действия. Необходимо получить накопительный пакет обновления для ядра СУБД, а также CAB-файлы для функций машинного обучения. Все файлы должны быть переданы на изолированный сервер и применены вручную.

1. Начните с базового экземпляра. Накопительные обновления можно применять только к существующим установкам.

  + Machine Learning Server (автономный) из начального выпуска SQL Server 2017
  + Сервер R Server (изолированный) от начального выпуска SQL Server 2016, SQL Server 2016 1 или SQL Server 2016 SP 2

2. Закройте все открытые сеансы R или Python и завершите все процессы, запущенные в системе.

3. Если вы включили возможность запуска в качестве веб-узлов и вычислений для развертывания веб-служб, создайте резервную копию файла **appSettings. JSON** в качестве меры предосторожности. Применение SQL Server 2017 CU13 или более поздней версии изменяет этот файл, поэтому для сохранения исходной версии может потребоваться резервная копия.

4. На подключенном к Интернету устройстве щелкните ссылку накопительное обновление для вашей версии SQL Server.

  + [Обновления SQL Server 2017](https://sqlserverupdates.com/sql-server-2017-updates/)
  + [Обновления SQL Server 2016](https://sqlserverupdates.com/sql-server-2016-updates/)

5. Скачайте Последнее накопительное обновление. Это исполняемый файл.

6. На устройстве, подключенном к Интернету, дважды щелкните EXE-файл, чтобы запустить программу установки и пошаговое выполнение мастера, чтобы принять условия лицензионного соглашения, просмотреть затронутые функции и отслеживать ход выполнения до завершения.

7. На сервере без подключения к Интернету:

   + Получите соответствующие CAB-файлы для R и Python. Ссылки для загрузки см. в разделе [скачивания CAB-файлов для накопительных обновлений SQL Server экземплярах аналитики в базе данных](sql-ml-cab-downloads.md).

   + Перенос всех файлов, основных исполняемых и CAB-файлов в папку на автономном компьютере.

   + Дважды щелкните EXE-файл, чтобы запустить программу установки. При установке обновления суммировать на сервере без подключения к Интернету вам будет предложено выбрать расположение CAB-файлов для R и Python.

8. После установки на сервере, на котором вы включили поддержку веб-узлов и вычислений, измените **appSettings. JSON**, добавив запись "ммлресаурцепас" непосредственно в разделе "ммлнативепас":

    ```json
    "ScorerParameters": {
        "MMLNativePath": "C:\Program Files\Microsoft SQL Server\140\R_SERVER\library\MicrosoftML\mxLibs\x64\",
        "MMLResourcePath": "C:\Program Files\Microsoft SQL Server\140\R_SERVER\library\MicrosoftML\mxLibs\x64\"
    }
    ```

9. [Запустите служебную программу CLI администратора](https://docs.microsoft.com/machine-learning-server/operationalize/configure-admin-cli-launch) , чтобы перезапустить веб-узел и ресурсы вычислений. Инструкции и синтаксис см. в разделе [мониторинг, запуск и завершение веб-узлов и вычислений](https://docs.microsoft.com/machine-learning-server/operationalize/configure-admin-cli-stop-start).

## <a name="development-tools"></a>Средства разработки

Интегрированная среда разработки не устанавливается в процессе установки. Дополнительные сведения о настройке среды разработки см. в статьях [Настройка средств R](../r/set-up-a-data-science-client.md) и [Настройка средств Python](../python/setup-python-client-tools-sql.md).

## <a name="next-steps"></a>Следующие шаги

Разработчики r могут приступить к работе с некоторыми простыми примерами и ознакомиться с основами работы R с SQL Server. Следующий шаг см. по следующим ссылкам:

+ [Учебник. Запуск R в T-SQL](../tutorials/rtsql-using-r-code-in-transact-sql-quickstart.md)
+ [Учебник. Аналитика в базе данных для разработчиков R](../tutorials/sqldev-in-database-r-for-sql-developers.md)

::: moniker range=">=sql-server-2017||=sqlallproducts-allversions"
Разработчики Python могут узнать, как использовать Python с SQL Server, следуя этим учебникам:

+ [Учебник. Запуск Python в T-SQL](../tutorials/run-python-using-t-sql.md)
+ [Учебник. Аналитика в базе данных для разработчиков Python](../tutorials/sqldev-in-database-python-for-sql-developers.md)
::: moniker-end

Примеры машинного обучения, основанные на реальных сценариях, см. в разделе [учебники](../tutorials/machine-learning-services-tutorials.md)по машинному обучению.

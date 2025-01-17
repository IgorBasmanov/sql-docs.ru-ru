---
title: 'Контрольный список развертывания: Многосерверная установка PowerPivot для SharePoint 2010 | Документация Майкрософт'
ms.custom: ''
ms.date: 03/08/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: database-engine
ms.topic: conceptual
ms.assetid: 4380040a-1368-4a47-8930-47c65a192e59
author: markingmyname
ms.author: maghan
manager: craigg
ms.openlocfilehash: 33bbaf46bb4dee5e0e7b58f6dc179ae5647ee3e7
ms.sourcegitcommit: a1adc6906ccc0a57d187e1ce35ab7a7a951ebff8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68890737"
---
# <a name="deployment-checklist-multi-server-installation-of-powerpivot-for-sharepoint-2010"></a>Контрольный список развертывания: установка нескольких серверов PowerPivot для SharePoint 2010
  Этот контрольный список поможет вам выполнить действия по [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] добавлению для SharePoint в трехуровневой ферму SharePoint 2010, которая строится с нуля. Трехуровневая ферма содержит три уровня — базы данных, приложений и веб-уровень. Для [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] добавления в эту топологию необходимо запустить программу установки SQL Server для [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] установки на уровне приложения. Программные файлы PowerPivot добавляются на веб-уровень, но только в качестве задачи, выполняемой после установки, при развертывании решения веб-приложения. Хотя развертывание состоит из определенных шагов, оно не предполагает отдельных шагов для установки на веб-уровень или на уровень данных. Единственный шаг установки, который необходимо выполнить, — установка [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] на серверах приложений.  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]**  SharePoint 2010|  
  
  
  
## <a name="prerequisites"></a>предварительные требования  
 Для установки SQL Server и SharePoint 2010 необходимо быть локальным администратором.  
  
 Устанавливающий SharePoint пользователь должен также настроить ферму. Для настройки фермы необходимо имя входа SQL Server для сервера баз данных. Имя входа должно быть назначено следующим ролям: `dbcreator` `securityadmin` и `public`.  
  
 Должен быть установлен выпуск SharePoint Server 2010 Enterprise или Enterprise Evaluation.  
  
 Компьютер должен быть присоединен к домену.  
  
 Необходимо знать, какие учетные записи будут использоваться для запуска компонента Database Engine, служб в ферме и служб Analysis Services в режиме интеграции с SharePoint. Хотя эти учетные записи можно изменить позже, они указываются в первый раз во время установки.  
  
 Учетные записи служб, указанные во время установки, должны быть учетными записями пользователей домена.  
  
 Перед началом установки проверьте настройки браузера, чтобы убедиться в наличии соединения с Интернетом. Установщик предварительных условий использует соединение с Интернетом для загрузки необходимых компонентов. Необходимо выполнить следующие изменения, чтобы убедиться, что все необходимое программное обеспечение будет получено:  
  
-   В диспетчере сервера временно отключите улучшенную конфигурацию безопасности Internet Explorer, чтобы разрешить загрузки на сервер. Для загрузки необходимого программного обеспечения можно отключить расширенную конфигурацию IE ESC только для администраторов.  
  
-   В Internet Explorer также может потребоваться настройка настроить браузер таким образом, чтобы он не обращался к прокси-серверу и разрешал локальному компьютеру доступ к URL-адресам Интернета.  
  
    1.  В Internet Explorer в меню «Сервис» выберите пункт «Свойства обозревателя».  
  
    2.  Перейдите на вкладку «Подключения», в области настроек локальной сети нажмите кнопку «Настройки локальной сети».  
  
    3.  В области автоматической настройки снимите флажок «Автоматическое определение параметров».  
  
    4.  В области прокси-сервера установите флажок «Использовать прокси-сервер для подключений LAN».  
  
    5.  Введите адрес прокси-сервера в поле «Адрес».  
  
    6.  Введите номер порта прокси-сервера в поле «Порт».  
  
    7.  Установите флажок «Не использовать прокси-сервер для локальных адресов».  
  
    8.  Нажмите кнопку ОК, чтобы закрыть диалоговое окно «Настройка локальной сети».  
  
    9. Нажмите кнопку ОК, чтобы закрыть диалоговое окно «Свойства обозревателя».  
  
##  <a name="installdb"></a>Установка сервера базы данных  
 В этом разделе предполагается, что топология фермы основана на описанной в статье [несколько серверов для трехуровневой фермы](https://go.microsoft.com/fwlink/?LinkId=182771). Если у вас уже есть ферма, которая находится в рабочем состоянии, переходите к [установке PowerPivot для SharePoint](#installppapp).  
  
 Если создание топологии только начинается, начните с установки компонента SQL Server Database Engine. В результате выполнения этих инструкций будет создан сервер баз данных, доступный от серверов SharePoint в ферме.  
  
1.  На компьютере, используемом для сервера базы данных, запустите программу установки SQL Server для установки SQL Server ядро СУБД (см. [install SQL Server 2014 с помощью мастера &#40;&#41;установки](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)).  
  
     Во время выбора устанавливаемых элементов выберите следующие компоненты:  
  
    -   Службы ядра СУБД  
  
    -   Средства связи клиентских средств  
  
    -   Средства управления — полностью (основные включаются автоматически)  
  
2.  После установки компонента Database Engine, включите TCP/IP для удаленных соединений и перезапустите службу:  
  
    1.  Запустите диспетчер конфигурации SQL Server.  
  
    2.  Откройте раздел Сетевая конфигурация SQL Server.  
  
    3.  Выберите **Протоколы для MSSQLSERVER**.  
  
    4.  Щелкните правой кнопкой мыши **TCP/IP** и выберите **включить**.  
  
    5.  Щелкните **SQL Server службы**.  
  
    6.  Щелкните правой кнопкой мыши **SQL Server (MSSQLSERVER)** и выберитепункт Перезапустить.  
  
3.  Включите входной доступ к серверу баз данных через брандмауэр Windows. Это позволит серверам SharePoint в ферме устанавливать соединения с базами данных SharePoint. Дополнительные сведения см. в статье [Настройка брандмауэра Windows для разрешения доступа к SQL Server](../../../2014/sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
    1.  В панели управления Windows в меню Администрирование выберите пункт **Брандмауэр Windows в области повышенная безопасность**.  
  
    2.  Щелкните **правила для входящих подключений**.  
  
    3.  Нажмите кнопку **создать правило**.  
  
    4.  В списке Тип правила щелкните **Настраиваемый**.  
  
    5.  Нажмите кнопку **Далее**.  
  
    6.  В окне программы в разделе службы щелкните **настроить**.  
  
    7.  Щелкните **Применить к этой службе**.  
  
    8.  Выберите **SQL Server (MSSQLSERVER)** , если SQL Server установлен в качестве экземпляра по умолчанию, и нажмите кнопку **ОК**.  
  
    9. Нажмите кнопку **Далее**.  
  
    10. В поле Протокол и порты примите параметры по умолчанию и нажмите кнопку **Далее**.  
  
    11. В поле область примите параметры по умолчанию и нажмите кнопку **Далее**.  
  
    12. В поле действие примите параметры по умолчанию и нажмите кнопку **Далее**.  
  
    13. В поле Профиль снимите флажки для **частных** и общедоступных и нажмите кнопку **Далее**.  
  
    14. В поле Имя введите описательное имя правила для входящего трафика (например, **SQL Server**).  
  
    15. Нажмите кнопку **Готово**.  
  
##  <a name="installsp"></a>Установка и настройка фермы SharePoint 2010 с тремя уровнями  
 Выполните на каждом из компьютеров, используемых в качестве серверов SharePoint, программу установщика предварительных условий, необходимых для SharePoint, а затем программу установки сервера SharePoint.  
  
 Для установки и настройки фермы SharePoint 2010, которая содержит два веб-сервера и сервер приложений, см. следующие инструкции в документации по SharePoint 2010:  
  
 [Несколько серверов для трехуровневой фермы (SharePoint Server 2010)](https://go.microsoft.com/fwlink/?LinkId=182771)  
  
 При запросе на указание сервера базы данных укажите ранее установленный сервер баз данных.  
  
 В следующих процедурах предполагается, что ферма настроена согласно инструкциям по установке трехуровневой фермы. Ферма должна включать следующие службы и функции:  
  
-   службу Excel, службу поиска, службу Secure Store  
  
-   веб-приложение и семейство веб-сайтов;  
  
-   сбор данных об использовании и исправности;  
  
-   журнал диагностики.  
  
##  <a name="installppapp"></a>Установка PowerPivot для SharePoint на сервере приложений  
 Чтобы добавить PowerPivot для SharePoint в ферму SharePoint, запустите программу установки SQL Server. Если ферма состоит из нескольких серверов SharePoint, программу установки SQL Server необходимо запустить на сервере приложений, который уже добавлен в ферму.  
  
 Всегда устанавливайте PowerPivot для SharePoint на сервере приложений. Несмотря на то что на северах клиентских веб-интерфейсов также будут выполняться серверные компоненты PowerPivot для SharePoint, компоненты, выполняющиеся на клиентском веб-интерфейсе, устанавливаются на этапе настройки PowerPivot для SharePoint при развертывании решений в ферме. Дополнительные сведения об установке см. в разделе [Install PowerPivot для SharePoint 2010](../../../2014/sql-server/install/install-powerpivot-for-sharepoint-2010.md).  
  
 Если топология развертывания требует наличия двух экземпляров PowerPivot для SharePoint, запустите программу установки SQL Server на обоих серверах приложений. На отдельном компьютере может быть установлен только один экземпляр PowerPivot для SharePoint. Если требуется несколько экземпляров, необходимо использовать дополнительные серверы. Дополнительные сведения о добавлении нескольких PowerPivot для SharePoint серверов в одну ферму см. в [разделе Контрольный список развертывания. Горизонтальное масштабирование путем добавления серверов PowerPivot в ферму](../../../2014/sql-server/install/deployment-checklist-scale-out-adding-powerpivot-servers-sharepoint-2010-farm.md)SharePoint 2010.  
  
##  <a name="installclientlib"></a>Установите Analysis Services клиентские библиотеки на серверах приложений SharePoint без установки PowerPivot для SharePoint  
 Топология фермы, включающая сервер клиентского веб-интерфейса или сервер приложений, при отсутствии PowerPivot для SharePoint на том же компьютере, требует дополнительного программного обеспечения для поддержания доступа к данным PowerPivot и функциям:  
  
-   Службы Excel Services или PerformancePoint Services  
  
-   Центр администрирования, работающий на собственном сервере в качестве изолированного приложения  
  
 Для доступа к данным PowerPivot служб Excel Services и PerformancePoint Services требуется новая версия поставщика OLE DB служб Analysis Services. Для запуска любого приложения на компьютере, на котором не установлена последняя версия поставщика OLE DB, необходимо установить поставщик вручную. Дополнительные сведения см. [в статье установка поставщик Analysis Services OLE DB на серверах SharePoint](../../../2014/sql-server/install/install-the-analysis-services-ole-db-provider-on-sharepoint-servers.md) .  
  
 Аналогично, если на компьютере имеется только центр администрирования и отсутствует PowerPivot для SharePoint, потребуется клиентская библиотека ADOMD.NET. Эта библиотека используется панелью мониторинга PowerPivot для доступа к внутренним данным, которые она использует для заполнения. Дополнительные сведения можно найти в статье [Установка ADOMD.NET на веб-серверах, обслуживающих клиентские запросы, под управлением центра администрирования](../../../2014/sql-server/install/install-adomd-net-on-web-front-end-servers-running-central-administration.md).  
  
##  <a name="configsrvr"></a>Настройка сервера  
 Для настройки PowerPivot для SharePoint используйте средство настройки PowerPivot. Средство проверит существующую конфигурацию фермы и предложит варианты установки или активации компонентов SharePoint, необходимых для PowerPivot для SharePoint. На этом этапе будет запущена служба Claims to Windows Token Service. Кроме того, если еще не включены другие необходимые функции SharePoint, средство настройки добавит их в список и запланирует действия по включению этих функций.  
  
 Дополнительные сведения см. в статье [Configure and Repair PowerPivot для SharePoint &#40;2010 PowerPivot Configuration&#41;Tool](../../../2014/analysis-services/configure-repair-powerpivot-sharepoint-2010.md).  
  
##  <a name="AAM"></a>Настройка альтернативного сопоставления доступа для серверов клиентского веб-интерфейса  
 Чтобы обеспечить обработку запросов на доступ к данным PowerPivot или на обновление данных каждым сервером веб-интерфейса, необходимо сопоставить различные URL-адреса каждого из серверов одному и тому же веб-приложению.  
  
1.  В центре администрирования в окне "Управление приложениями" щелкните **Настройка сопоставления альтернативного доступа**.  
  
2.  В коллекции альтернативного сопоставления доступа выберите веб-приложение.  
  
3.  Щелкните **Добавить внутренний URL-адрес**.  
  
4.  Укажите URL-адрес первого сервера веб-интерфейса.  
  
5.  Повторите предыдущие шаги, чтобы добавить URL-адрес второго сервера клиентского веб-интерфейса.  
  
##  <a name="activatePP"></a>Активация интеграции функций PowerPivot для семейств веб-сайтов  
 Включение функций на уровне семейств веб-сайтов делает страницы и шаблоны приложения доступными для веб-сайтов, в том числе и страницы конфигурации для планового обновления данных, страницы приложений для библиотеки PowerPivot Gallery и библиотеки каналов данных.  
  
 Средство настройки PowerPivot включает интеграцию функций для указанных семейств веб-сайтов. Средство можно запускать повторно, чтобы выбрать другие семейства веб-сайтов. При необходимости администраторы сайтов могут включить функции и из SharePoint. Дополнительные сведения см. [в разделе Активация интеграции функций PowerPivot для семейств веб-сайтов в центре администрирования](https://docs.microsoft.com/analysis-services/power-pivot-sharepoint/activate-power-pivot-integration-for-site-collections-in-ca).  
  
##  <a name="verify"></a>Проверка доступности сервера и интеграции  
 Запросы PowerPivot обрабатываются в ферме при открытии пользователем или приложением книги Excel, содержащей данные PowerPivot. Как минимум можно проверить страницы на веб-сайтах SharePoint, чтобы убедиться в доступности функций PowerPivot. Однако, чтобы полностью проверить установку, необходимо иметь книгу PowerPivot, которую можно опубликовать в SharePoint и открывать из библиотеки. В целях тестирования можно опубликовать образец книги, в котором уже есть данные PowerPivot, и с его помощью удостовериться в правильности настройки интеграции с SharePoint.  
  
 Чтобы проверить интеграцию PowerPivot с сайтом SharePoint, выполните следующие действия.  
  
1.  Откройте созданное веб-приложение в браузере. Если используются значения по умолчанию, можно указать http://\<имя компьютера > в URL-адресе.  
  
2.  Удостоверьтесь в наличии в приложении доступа к данным PowerPivot и функциям обработки. Определить это можно по наличию предоставляемых PowerPivot шаблонов библиотек.  
  
    1.  В меню Действия сайта щелкните **Дополнительные параметры..** .  
  
    2.  В разделе библиотеки должны отобразиться **Библиотека веб-каналов данных** и **Галерея PowerPivot**. Эти шаблоны библиотек предоставляются компонентом PowerPivot и отображаются в списке библиотек только при правильной интеграции этого компонента.  
  
 Чтобы выполнить проверку доступа к данным PowerPivot на сервере, выполните следующие действия.  
  
1.  [Загрузите](https://go.microsoft.com/fwlink/?LinkID=219108) образец данных Picnic, поставляемый вместе с учебником по службам Reporting Services. Книгу в этом образце можно использовать для проверки доступа к данным PowerPivot. Извлеките файлы.  
  
2.  Передайте книгу PowerPivot в галерею PowerPivot или любую библиотеку SharePoint.  
  
3.  Щелкните документ, чтобы открыть его из библиотеки.  
  
4.  Щелкните срез или отфильтруйте данные, чтобы запустить запрос PowerPivot. Сервер загрузит данные PowerPivot в фоновом режиме и вернет результаты. Далее нужно будет подключиться к серверу, чтобы удостовериться, что данные загружены и кэшированы.  
  
5.  Из группы программ Microsoft SQL Server 2008 R2 в меню «Пуск» запустите среду SQL Server Management Studio. Если это средство не установлено на сервере, можно пропустить этот шаг и перейти к последнему действию по подтверждению наличия кэшированных файлов.  
  
6.  На странице «Тип сервера» выберите **Analysis Services**.  
  
7.  В поле имя сервера введите  **\<имя сервера > \PowerPivot**, где  **\<Server-name >** — это имя компьютера, на котором установлена PowerPivot для SharePointная установка.  
  
8.  Нажмите кнопку **Соединить**.  
  
9. В обозревателе объектов щелкните **базы данных** , чтобы просмотреть список загруженных файлов данных PowerPivot.  
  
10. В файловой системе компьютера откройте следующую папку, чтобы определить, были ли файлы кэшированы на диск. Наличие кэшированных файлов служит еще одним подтверждением работоспособности развертывания. Чтобы открыть кэш файлов, перейдите в папку \Program Files\Microsoft SQL Server\MSAS10_50.POWERPIVOT\OLAP\Backup.  
  
##  <a name="nextsteps"></a>Действия после установки  
 Проверив установку, завершите настройку службы созданием галереи PowerPivot или настройкой отдельных параметров конфигурации. Чтобы полностью использовать компоненты только что установленного сервера, можно загрузить [!INCLUDE[ssGeminiClient](../../includes/ssgeminiclient-md.md)], чтобы создать, а затем опубликовать первую книгу PowerPivot.  
  
####  <a name="bkmk_disk"></a>Установка верхних пределов использования дискового пространства  
 Пользователь может задавать максимальный предел пространства на диске, используемого для кэшируемых на диске файлов данных PowerPivot. По умолчанию разрешено использовать все доступное место на диске. Инструкции по ограничению использования дискового пространства см. в разделе [Настройка использования &#40;места на&#41;диске PowerPivot для SharePoint](https://docs.microsoft.com/analysis-services/power-pivot-sharepoint/configure-disk-space-usage-power-pivot-for-sharepoint).  
  
####  <a name="Upload"></a>Увеличить максимальный размер отправляемых файлов для веб-приложений SharePoint  
 Поскольку книги PowerPivot могут быть большими, может потребоваться увеличить максимальный размер передаваемых файлов. Существует два параметра размера файла для настройки: Максимальный размер передачи для веб-приложения и максимальный размер книги в службах Excel. Для максимального размера файла должно быть установлено одинаковое значение в обоих приложениях. Инструкции см. в разделе [Настройка максимального размера &#40;отправляемого&#41;файла PowerPivot для SharePoint](https://docs.microsoft.com/analysis-services/power-pivot-sharepoint/configure-maximum-file-upload-size-power-pivot-for-sharepoint).  
  
#### <a name="grant-sharepoint-permissions-to-workbook-users"></a>Предоставление разрешений SharePoint пользователям книги  
 Для публикации и просмотра книг пользователям требуются разрешения SharePoint. Не забудьте предоставить разрешения на **Просмотр** пользователям, которым необходимо просматривать опубликованные книги, и разрешения на **участие** для пользователей, которые публикуют книги и управляют ими. Для предоставления разрешений необходимо быть администратором семейства веб-сайтов.  
  
1.  На сайте щелкните **действия сайта**.  
  
2.  Щелкните **разрешения сайта**.  
  
3.  Установите флажок для группы **члены** семейства веб-сайтов.  
  
4.  На ленте щелкните **предоставить разрешения**.  
  
5.  Введите учетные записи пользователей или групп домена Windows, которые будут иметь разрешения на добавление или удаление документов.  
  
6.  Нажмите кнопку **ОК**.  
  
7.  Установите флажок для группы **посетителей** семейства веб-сайтов.  
  
8.  На ленте щелкните **предоставить разрешения**.  
  
9. Введите учетные записи пользователей или групп домена Windows, которые будут иметь разрешение на просмотр документов. Как и ранее, адреса электронной почты или группы распределения следует использовать только в том случае, если приложение настроено для классической проверки подлинности.  
  
10. Нажмите кнопку **ОК**.  
  
#### <a name="install-adonet-data-services-35-sp1"></a>Установка службы ADO.NET Data Services 3.5 с пакетом обновления 1 (SP1)  
 Для обеспечения возможности экспорта списков SharePoint в виде веб-каналов данных требуется служба ADO.NET Data Services. Этот компонент не входит в программу SharePoint 2010 PrerequisiteInstaller, поэтому его необходимо устанавливать вручную. Дополнительные сведения об установке служб ADO.NET Data Services см. в статье [Установка служб данных ADO.NET для поддержки экспорта веб-каналов данных в списках SharePoint](../../../2014/sql-server/install/install-ado-net-data-services-to-support-data-feed-exports-of-sharepoint-lists.md).  
  
#### <a name="install-data-providers-used-in-data-refresh-and-check-user-permissions"></a>Установка поставщиков данных, используемых для обновления данных, и проверка разрешений пользователя  
 Обновление данных на сервере позволяет пользователям импортировать обновленные данные в свои книги в автоматическом режиме. Для успешного обновления данных на сервере должен иметься тот же поставщик данных, который первоначально использовался для импорта данных. Кроме того, учетная запись пользователя, под которой выполняется обновление данных, часто должна иметь разрешение на чтение из внешних источников данных. Чтобы гарантировать успешное завершение операции, необходимо проверить требования для включения и настройки обновления данных. Дополнительные сведения см. [в разделе PowerPivot Data Refresh with SharePoint 2010](../../../2014/analysis-services/powerpivot-data-refresh-with-sharepoint-2010.md).  
  
#### <a name="create-a-powerpivot-gallery"></a>Создать галерею PowerPivot  
 Галерея PowerPivot — это библиотека, включающая в себя возможность предварительного просмотра и представления для просмотра книг PowerPivot на сайте SharePoint. Галерею PowerPivot рекомендуется использовать для публикации и просмотра книг PowerPivot из-за ее возможностей предварительного просмотра. Кроме того, если на том же сервере SharePoint также развернуть службы Reporting Services, то галерея PowerPivot позволит упростить создание отчетов. Построитель отчетов можно запускать из галереи PowerPivot, чтобы использовать в качестве основы для нового отчета опубликованную книгу PowerPivot. Дополнительные сведения о создании и использовании библиотеки см. в разделе [Создание и настройка галереи PowerPivot](https://docs.microsoft.com/analysis-services/power-pivot-sharepoint/create-and-customize-power-pivot-gallery) и [использование галереи PowerPivot](https://docs.microsoft.com/analysis-services/power-pivot-sharepoint/use-power-pivot-gallery).  
  
#### <a name="tune-configuration-settings"></a>Настройка параметров конфигурации  
 Приложение службы PowerPivot создается с использованием свойств и значений по умолчанию. Параметры конфигурации отдельных приложений службы можно менять для изменения методологии распределения запросов, задания времени ожидания сервера, изменения пороговых значений для событий отчета об ответе на запрос или указания срока хранения данных об использовании. Дополнительные сведения о настройке в центре администрирования или об использовании функций PowerPivot в веб-приложениях SharePoint см. [в разделе Администрирование и настройка PowerPivot в центре администрирования](https://docs.microsoft.com/analysis-services/power-pivot-sharepoint/power-pivot-server-administration-and-configuration-in-central-administration).  
  
## <a name="see-also"></a>См. также  
 [Функции, поддерживаемые различными выпусками SQL Server 2012](https://go.microsoft.com/fwlink/?linkid=232473)   
 [Установка PowerPivot для SharePoint 2010](../../../2014/sql-server/install/install-powerpivot-for-sharepoint-2010.md)   
 [Контрольный список развертывания: Горизонтальное масштабирование путем добавления серверов PowerPivot в ферму SharePoint 2010](../../../2014/sql-server/install/deployment-checklist-scale-out-adding-powerpivot-servers-sharepoint-2010-farm.md)  
  
  

---
title: Устранение неполадок при установке Reporting Services | Документация Майкрософт
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: database-engine
ms.topic: conceptual
ms.assetid: e2536f7f-d90c-4571-9ffd-6bbfe69018d6
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 122bbd15f7b3332e917561f1ff9abe0119fafb2b
ms.sourcegitcommit: a1adc6906ccc0a57d187e1ce35ab7a7a951ebff8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68891496"
---
# <a name="troubleshoot-a-reporting-services-installation"></a>Устранение неполадок при установке служб Reporting Services
  Если службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] не удалось установить из-за ошибок, возникших в процессе установки, воспользуйтесь инструкциями, приведенными в этом разделе, чтобы выяснить причины, которые, скорее всего, привели к их возникновению.  
  
 Последние сведения о проблемах с [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]см. в разделе [Советы и устранение неполадок служб SQL Server 2012 Reporting Services](https://go.microsoft.com/fwlink/?LinkId=221297).  
  
 Сведения о других ошибках и проблемах, связанных со службами [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , см. в разделе [Устранение неполадок и ошибок служб SSRS.](https://social.technet.microsoft.com/wiki/contents/articles/ssrs-troubleshooting-issues-and-errors.aspx)  
  
 В случае обнаружения проблемы, описанной в заметках о выпуске, см. статью [Заметки о выпуске в Интернете](https://go.microsoft.com/fwlink/?linkid=236893) .  
  
 В этом разделе содержатся следующие сведения:  
  
-   [Проверка журналов установки](#bkmk_setuplogs)  
  
-   [Проверка предварительных требований](#bkmk_prereq)  
  
-   [Устранение неполадок при установке в режиме интеграции с SharePoint](#bkmk_tshoot_sharepoint)  
  
-   [Устранение неполадок при установке в собственном режиме](#bkmk_tshoot_native)  
  
-   [Дополнительные ресурсы](#bkmk_additional)  
  
##  <a name="bkmk_setuplogs"></a> Проверка журналов установки  
 Ошибки установки регистрируются в файлах журнала, расположенных в папке **Program Files\Microsoft SQL Server\110\Setup Bootstrap\Log** . При каждом запуске программы установки там создается новая вложенная папка. Эта вложенная папка имеет имя, включающее время и дату запуска программы установки. Дополнительные сведения о просмотре файлов журналов установки см. в статье [Просмотр и чтение файлов журналов программы установки SQL Server](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md).  
  
-   Журналы содержат набор файлов.  
  
-   Сведения о продукте, компонентах и экземпляре можно просмотреть в файлах с именем «*_summary.txt».  
  
-   Файлы «*_errorlog.txt» содержат сведения об ошибках, сформированных в процессе установки.  
  
-   Откройте файл *_RS\_\*_ComponentUpdateSetup.log, чтобы просмотреть сведения об установке служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
##  <a name="bkmk_prereq"></a> Проверка требований, необходимых для установки  
 Программа установки автоматически проверяет требования, необходимые для установки. Однако при устранении неполадок, возникших в процессе установки, бывает полезно знать, на соответствие каким именно требованиям производится проверка.  
  
-   Требования к учетной записи для запуска программы установки включают членство в локальной группе «Администраторы». Программа установки должна иметь разрешения на добавление файлов, параметров реестра, создание локальных групп безопасности и предоставление разрешений. При установке конфигурации по умолчанию программа установки должна иметь разрешения на создание базы данных сервера отчетов на экземпляре [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , на котором выполняется установка.  
  
-   Операционная система должна поддерживать службу HTTP.SYS 1.1.  
  
-   Служба HTTP должна быть включена и запущена.  
  
-   Кроме того, если устанавливается служба агента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , должен быть запущен координатор распределенных транзакций (DTC).  
  
-   В папке System32 должна присутствовать библиотека Authz.dll.  
  
 Программа установки больше не проверяет наличие служб IIS или [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] требуются компоненты MDAC 2.0 и платформа [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] версии 2.0. Если эти компоненты не установлены, то будет произведена их установка.  
  
##  <a name="bkmk_tshoot_sharepoint"></a> Устранение неполадок установки в режиме интеграции с SharePoint  
  
-   [Диспетчер конфигурации служб Reporting Services не запускается](#bkmk_configmanager_notstart)  
  
-   [После установки SQL Server 2012 SSRS в режиме интеграции с SharePoint служба SQL Server Reporting Services не отображается в центре администрирования SharePoint](#bkmk_no_ssrs_service)  
  
-   [Командлеты PowerShell для служб Reporting Services недоступны, и команды не распознаются.](#bkmk_cmdlets_not_recognized)  
  
-   [Будет выдано сообщение об ошибке, указывающее на то, что не настроен URL-адрес](#bkmk_URL_not_configured)  
  
-   [Программа установки завершает работу с ошибками на компьютере с установленным, но не настроенным компонентом SharePoint](#bkmk_sharepoint_not_confiugred)  
  
-   [Страница центра администрирования SharePoint пуста.](#bkmk_central_admin_blank)  
  
-   [При попытке создать отчет построителя отчетов отображается сообщение об ошибке](#bkmk_reportbuilder_newreport_error)  
  
-   [Отображается сообщение об ошибке: RS_SHP не поддерживается для действия PREPAREIMAGE](#bkmk_RS_SHP_notsupported)  
  
###  <a name="bkmk_configmanager_notstart"></a> Диспетчер конфигурации служб Reporting Services не запускается  
 **Nописание** Эта проблема связана с проектированием [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]в. Теперь службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] рассчитаны на архитектуру служб SharePoint. Диспетчер конфигурации больше не нужен для настройки и администрирования служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в режиме совместимости с SharePoint.  
  
 **Решение:** Для настройки сервера отчетов в режиме Sharepoint используйте центр администрирования SharePoint. Дополнительные сведения см. в статье [Управление служебным приложением SharePoint службы Reporting Services](../../../2014/reporting-services/manage-a-reporting-services-sharepoint-service-application.md).  
  
###  <a name="bkmk_no_ssrs_service"></a>После установки SQL Server 2012 SSRS в режиме интеграции с SharePoint служба SQL Server Reporting Services не отображается в центре администрирования SharePoint  
 **Nописание** Если после успешной установки [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] врежимеинтеграциисSharePointинадстройкидляSharePoint2010выневидите"SQLServerReportingServices[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] " в следующих двух меню, то служба не зарегистрировано:  
  
-   Центр администрирования SharePoint 2010 — > "Управление приложениями"-> странице "Управление службами на сервере"  
  
-   Центр администрирования SharePoint 2010 — > "Управление приложениями"-> "Администрирование приложений службы" — > меню "создать"  
  
 **Решение:** Чтобы зарегистрировать и запустить [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] службы SharePoint, выполните следующие действия.  
  
1.  На компьютере, где запущен центр администрирования SharePoint 2010  
  
    1.  Откройте консоль управления SharePoint 2010 с разрешениями администратора. Щелкните значок правой кнопкой мыши и выберите «Запуск от имени администратора». Вызовите на выполнение из командной оболочки следующие три командлета:  
  
    2.  ```  
        Install-SPRSService  
        ```  
  
    3.  ```  
        Install-SPRSServiceProxy  
        ```  
  
    4.  ```  
        Get-SPServiceInstance -all |where {$_.TypeName -like "SQL Server Reporting*"} | Start-SPServiceInstance  
        ```  
  
2.  Убедитесь, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] что в службе отображается состояние"запущено" на странице: Центр администрирования SharePoint 2010 — > "**Управление приложениями**" — > "**Управление службами на сервере**"  
  
###  <a name="bkmk_cmdlets_not_recognized"></a> Командлеты PowerShell для служб Reporting Services недоступны, и команды не распознаются.  
 **Nописание** При попытке запустить [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] командлет PowerShell отображается сообщение об ошибке следующего вида:  
  
-   Термин "Install-SPRSServiceInstall-SPRSService" **не распознан** как имя командлета, функции, файла скрипта или действующей программы. Проверьте правильность написания имени или, если путь включен, проверьте правильность пути и повторите попытку. Строка: 1 знак: 39 + Install-SPRSServiceInstall-SPRSService < < < < + CategoryInfo: ObjectNotFound (Install-SPRSServiceInstall-SPRSService: строка) [], CommandNotFoundExcep  
  
 **Решение:** Выполните одно из следующих действий.  
  
-   Запустите надстройку служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] для продуктов SharePoint. **rssharepoint.msi**.  
  
-   Установите службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в режиме интеграции с SharePoint с установочного носителя SQL Server.  
  
 **Примечание.** Если **Командная консоль SharePoint 2013** открыта по завершении одного из этих решений, закройте и снова откройте командную консоль.  
  
 Дополнительные сведения см. в следующих разделах:  
  
-   [Где найти надстройку службы Reporting Services для продуктов SharePoint](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md)  
  
-   [Установка служб Reporting Services в режиме SharePoint для SharePoint 2010](../../../2014/sql-server/install/install-reporting-services-sharepoint-mode-for-sharepoint-2010.md)  
  
-   [Установка Reporting Services режима SharePoint для SharePoint 2013](../../../2014/sql-server/install/install-reporting-services-sharepoint-mode-for-sharepoint-2013.md)  
  
###  <a name="bkmk_URL_not_configured"></a> Будет выдано сообщение об ошибке, указывающее на то, что не настроен URL-адрес  
 **Nописание** Появится сообщение об ошибке следующего вида:  
  
 Функциональность служб SQL Server Reporting Services (SSRS) не поддерживается. С помощью центра администрирования проверьте и исправьте одну из следующих проблем:•Не настроен URL-адрес сервера отчетов. Его можно задать на странице интеграции со службами SSRS.•Не настроен прокси-сервер службы SSRS. Ее можно задать на страницах приложения службы SSRS.•Приложение службы SSRS не сопоставлено с этим веб-приложением. На страницах приложения службы SSRS можно связать прокси-сервер приложения службы SSRS с группой прокси-серверов приложения для данного веб-приложения.  
  
 **Решение:** Сообщение об ошибке содержит три предлагаемых действия по устранению этой проблемы. Первое предложение в сообщении "URL-адрес сервера отчетов не настроен". относится к случаю интеграции с версией сервера отчетов до [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. Конфигурация SharePoint для предыдущих версий сервера отчетов выполнялась на странице **Общие параметры приложения** в службах **SQL Server Reporting Services (2008 и 2008 R2)** .  
  
 **Дополнительные сведения:** Это сообщение об ошибке отображается при попытке использовать любую из [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] функций, требующих подключения [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] к службе. В том числе:  
  
-   Открытие построителя отчетов SQL Server из библиотеки документов SharePoint.  
  
-   Управление подписками.  
  
-   Управление приложением службы.  
  
###  <a name="bkmk_sharepoint_not_confiugred"></a> Программа установки завершает работу с ошибками на компьютере с установленным, но не настроенным компонентом SharePoint  
 **Nописание** Если выбрать установку Reporting Services режиме SharePoint на компьютере, где установлен SharePoint, но не настроена SharePoint, появится сообщение, похожее на следующее, и программа установки будет остановлена:  
  
 Программа установки SQL Server завершила работу  
  
 **Решение:** Настройте SharePoint, а затем запустите SQL Server установку.  
  
 **Дополнительные сведения:** При установке [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] в и существующей установке SharePoint программа установки пытается установить и [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] запустить службу SharePoint. Если SharePoint не настроен, установка службы завершится сбоем, в результате чего программа установки также завершится сбоем.  
  
###  <a name="bkmk_central_admin_blank"></a> Страница центра администрирования SharePoint пуста.  
 **Nописание** Вы смогли успешно установить SharePoint 2010, без ошибок установки. Однако при просмотре центра администрирования отображается только пустая страница.  
  
 **Решение:** Эта проблема не относится к [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , но связана с конфигурацией разрешений в общей установке SharePoint. Ниже приведен список предлагаемых действий.  
  
-   Просмотр раздела справки SharePoint по средам разработки. [Настройка среды разработки для SharePoint 2010 в Windows Vista, Windows 7 и Windows Server 2008](https://msdn.microsoft.com/library/ee554869\(office.14\).aspx)  
  
-   Ознакомьтесь с сообщением на форуме: [Центр администрирования возвращает пустую страницу после установки в Windows 7](https://social.technet.microsoft.com/Forums/en/sharepoint2010setup/thread/a422a3c8-39f6-4b9e-988a-4c4d1e745694)  
  
-   Учетная запись службы, используемая для служб SharePoint, например службы центра администрирования SharePoint 2010, должна обладать правами администратора в локальной операционной системе.  
  
###  <a name="bkmk_reportbuilder_newreport_error"></a> При попытке создать отчет построителя отчетов отображается сообщение об ошибке  
 **Nописание** При попытке создать построитель отчетов отчет в библиотеке документов отображается сообщение об ошибке следующего вида:  
  
 Эта функция не поддерживается, поскольку приложения служб SQL Server Reporting Services не существует либо в центре администрирования не настроен URL-адрес сервера отчетов.  
  
 **Решение:** Убедитесь, что у [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] вас есть приложение службы, и оно настроено правильно. Дополнительные сведения см. в разделе "Создание приложения Reporting Services службы" раздела [Install Reporting Services SharePoint для sharepoint 2010](../../../2014/sql-server/install/install-reporting-services-sharepoint-mode-for-sharepoint-2010.md) .  
  
###  <a name="bkmk_RS_SHP_notsupported"></a> Отображается сообщение об ошибке: RS_SHP не поддерживается для действия PREPAREIMAGE  
 **Nописание** При попытке запустить PREPAREIMAGE для [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] отображается сообщение об ошибке следующего вида:  
  
 "Указанный компонент RS_SHP не поддерживается при запуске действия PREPAREIMAGE, поскольку он не поддерживает SysPrep. Удалите компоненты, несовместимые с SysPrep, и запустите программу установки еще раз".  
  
 **Решение:** Не существует решения. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] не поддерживают SYSPREP (PREPAREIMAGE). [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] поддерживает SYSPREP.  
  
##  <a name="bkmk_tshoot_native"></a> Устранение неполадок установки в собственном режиме  
  
###  <a name="PerfCounters"></a> Счетчики производительности невидимы после обновления до Windows Vista или Windows Server 2008  
 Если выполнено обновление операционной системы с переходом к версии [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)] или [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] на компьютере, где работают службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], то после обновления счетчики производительности служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] не будут установлены.  
  
#### <a name="to-reinstate-reporting-services-performance-counters"></a>Восстановление счетчиков производительности служб Reporting Services  
  
1.  Удалите следующие разделы реестра:  
  
    -   **Веб-служба HKLM\SYSTEM\CurrentControlSet\Services\MSRS 2011**  
  
    -   **Служба Windows HKLM\SYSTEM\CurrentControlSet\Services\MSRS 2011**  
  
2.  Откройте окно командной строки и введите следующую команду:  
  
    -   **запустите\<** *каталог .NET 2,0 Framework* **> \инсталлутил.ЕКСЕ \<**  *сервер отчетов \репортингсервицеслибрари.длл каталог bin* **>**  
  
        > [!NOTE]  
        >  Замените \< *каталог .NET 2,0 Framework*> физическим путем .NET Framework 2,0 файлов и замените \< *каталог bin сервера отчетов*> физическим путем к файлам bin сервера отчетов.  
  
3.  Перезапустите службу [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Чтобы убедиться, что данные шаги были выполнены успешно, откройте веб-браузер и перейдите по URL-адресу диспетчера отчетов или сервера отчетов. После этого откройте системный монитор, чтобы проверить, работают ли счетчики.  
  
#### <a name="to-re-add-the-performance-registry-keys-by-using-registry-editor"></a>Повторное добавление разделов реестра Performance при помощи редактора реестра  
  
1.  Откройте редактор реестра следующим образом.  
  
    1.  Нажмите кнопку **Пуск**и выберите пункт **Выполнить**.  
  
    2.  В диалоговом окне **выполнить** в поле **Открыть** введите `regedit`.  
  
2.  В редакторе реестра выберите следующий раздел реестра: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2011 Web Service\Performance`  
  
3.  Щелкните правой кнопкой мыши узел **Performance** , укажите пункт **Создать**, а затем щелкните **Мультистроковый параметр**.  
  
4.  Введите `Counter Names` и нажмите клавишу ВВОД.  
  
5.  Повторите эти шаги для добавления раздела реестра `Counter Types` в этом узле.  
  
6.  Перейдите к следующему разделу реестра: `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2011 Web Service\Performance`  
  
7.  Щелкните правой кнопкой мыши узел **Performance** , укажите пункт **Создать**, а затем щелкните **Мультистроковый параметр**.  
  
8.  Введите `Counter Names` и нажмите клавишу ВВОД.  
  
9. Повторите эти шаги для добавления раздела реестра `Counter Types` в этом узле.  
  
 После исправления 64-разрядного экземпляра или повторного добавления разделов реестра вручную можно использовать системный монитор для настройки объектов производительности служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , которые необходимо мониторить.  
  
###  <a name="ConfigPropsMissing"></a> Свойства настройки ReportServerExternalURL и PassThroughCookies не настраиваются после обновления с переходом от версии SQL Server 2005  
 При обновлении с [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] на [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] свойства конфигурации `ReportServerExternalURL` и `PassThroughCookies` не настраиваются. `ReportServerExternalURL`является необязательным свойством, и его следует задавать только в том случае, если вы используете SharePoint 2,0 веб-части и хотите, чтобы пользователи могли получить отчет и открыть его в новом окне браузера. Дополнительные сведения о `ReportServerExternalURL`см. [в разделе URL-адреса &#40;в файлах&#41;конфигурации SSRS Configuration Manager](../../reporting-services/install-windows/urls-in-configuration-files-ssrs-configuration-manager.md). `PassThroughCookies`требуется только при использовании настраиваемого метода проверки подлинности. Дополнительные сведения о `PassThroughCookies`см. [в разделе Настройка диспетчер отчетов для передачи файлов cookie пользовательской проверки](../security/configure-the-web-portal-to-pass-custom-authentication-cookies.md)подлинности.  
  
> [!NOTE]  
>  При использовании нестандартной проверки подлинности рекомендуется произвести миграцию установки, а не выполнять обновление. Дополнительные сведения о переносе [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] см. в статье [Перенос установки служб Reporting Services (собственный режим)](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md).  
  
 По умолчанию эти свойства в конфигурации служб [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] не существуют. Если эти свойства настроены в [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] и остается необходимость в предоставляемых ими функциях, необходимо вручную добавить их в файл **RSReportServer.config** после завершения процесса обновления. Дополнительные сведения см. в статье [Изменение файла конфигурации служб Reporting Services (RSreportserver.config)](../report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  
  
###  <a name="Default2005InstallBreaks2008"></a>Сбой установки для экземпляра по умолчанию SQL Server 2005 Reporting Services на компьютере, на котором запущены службы SQL Server 2012Reporting Services  
 При попытке установки экземпляра [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] по умолчанию на компьютере, где уже запущен экземпляр служб [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)], установка экземпляра служб [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] завершится следующей ошибкой:  
  
 «Экземпляр с тем же именем уже установлен на этом компьютере. Чтобы продолжить программу установки SQL Server, предоставьте уникальное имя экземпляра».  
  
 Эта проблема возникает независимо от того, является ли экземпляр служб [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] экземпляром по умолчанию или именованным экземпляром, а также независимо от того, существует ли уже экземпляр служб [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] с этим именем на компьютере.  
  
 Чтобы обойти эту проблему, можно воспользоваться одним из следующих вариантов.  
  
-   Если необходимо, чтобы службы [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] работали как экземпляр по умолчанию на этом компьютере, то необходимо установить экземпляр служб [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] перед установкой экземпляра [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] .  
  
-   Если экземпляр служб [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] не должен быть экземпляром по умолчанию, то можно установить экземпляр служб [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] как именованный экземпляр после установки экземпляра служб [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] .  
  
###  <a name="WindowsAuthBreaksAfterUpgrade"></a>401-несанкционированная ошибка при использовании проверки подлинности Windows после обновления с SQL Server 2005 до SQL Server 2012  
 Если выполнено обновление с переходом от служб [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] к службам [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)], а после обновления используется проверка подлинности NTLM при помощи встроенной учетной записи для учетной записи службы сервера отчетов, то во время доступа к серверу отчетов или диспетчеру отчетов может возникнуть ошибка "401 — нет доступа".  
  
 Это происходит вследствие изменения в настройке по умолчанию служб [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] для проверки подлинности Windows. Настроено «Negotiate», если учетной записью службы сервера отчетов является Network Service или Local System. Настроена NTLM, если учетная запись службы сервера отчетов не входит в число этих встроенных учетных записей. Для устранения этой проблемы после обновления можно изменить файл RSReportServer.config и выполнить настройку, чтобы параметр `AuthenticationType` имел значение `RSWindowsNTLM`. Дополнительные сведения см. в статье [Configure Windows Authentication on the Report Server](../security/configure-windows-authentication-on-the-report-server.md).  
  
###  <a name="Uninstall32BitBreaks64Bit"></a>Удаление 32-разрядного экземпляра SQL Server 2012 Reporting Services в параллельном развертывании с экземпляром 64-bit, разбивается на экземпляр 64-bit  
 Параллельная установка на компьютер 32-разрядного и 64-разрядного экземпляров служб [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] и последующее удаление 32-разрядного экземпляра приводит к удалению четырех разделов реестра для служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . В результате — нарушение работы 64-разрядного экземпляра служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. При удалении 32-разрядного экземпляра удаляются следующие разделы реестра для служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2011 Web Service\Performance:Counter Names` `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2011 Windows Service\Performance:Counter Names` `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2011 Web Service\Performance:Counter Types` `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2011 Windows Service\Performance:Counter Types`  
  
 Для устранения этой проблемы можно внести исправления в 64-разрядный экземпляр. Безусловно, рекомендуется использовать процесс исправления, но разделы реестра можно вновь добавить вручную при помощи редактора реестра.  
  
> [!CAUTION]  
>  Неправильное изменение реестра может вызвать серьезные проблемы. Перед внесением изменений в реестр рекомендуется создать резервную копию всех важных данных.  
  
##  <a name="bkmk_additional"></a> Дополнительные ресурсы  
 Ниже приведены дополнительные ресурсы, которые могут быть полезны при устранение проблем:  
  
-   Вики-сайт TechNet: Проблемы, связанные [с устранением неполадок SQL Server Reporting Services (SSRS) в режиме интеграции с SharePoint](https://social.technet.microsoft.com/wiki/contents/articles/troubleshoot-sql-server-reporting-services-ssrs-in-sharepoint-integrated-mode.aspx)  
  
-   [Дискуссион SQL Server Reporting Services](http://social.msdn.microsoft.com/Forums/sqlreportingservices/threads)  
  
 ![Параметры SharePoint](https://docs.microsoft.com/analysis-services/analysis-services/media/as-sharepoint2013-settings-gear.gif "Параметры SharePoint") [Отправка отзыва и контактной информации через Microsoft SQL Server Connect](https://connect.microsoft.com/SQLServer/Feedback) (https://connect.microsoft.com/SQLServer/Feedback).  
  
  

---
title: Установка построителя отчетов | Документы Майкрософт
ms.date: 09/22/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.assetid: 6b2291bb-1d20-4d08-81cb-a16dd8e01faf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d0d991e088a9bd06c322c3d0c8839b0c9c29fc52
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MTE75
ms.contentlocale: ru-RU
ms.lasthandoff: 06/15/2019
ms.locfileid: "65502948"
---
# <a name="install-report-builder"></a>Install Report Builder
  [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] является автономным приложением и устанавливается на компьютере пользователем или администратором. Вы можете установить приложение из центра загрузки Майкрософт, с сервера отчетов [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] или с сайта SharePoint, интегрированного с [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  

> [!NOTE]
> Интеграция служб Reporting Services с SharePoint больше не доступна после выхода SQL Server 2016.
  
 Обычно администратор устанавливает и настраивает службы [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], предоставляет разрешение на скачивание [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] с веб-портала и управляет папками и разрешениями для отчетов, элементов отчетов и общих наборов данных, сохраняемых на сервере отчетов. Дополнительные сведения об администрировании [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] см. в разделе [Сервер отчетов служб Reporting Services (собственный режим)](../../reporting-services/report-server/reporting-services-report-server-native-mode.md).  
  
## <a name="install-includessrbnoversionincludesssrbnoversionmd-from--a--web-portal-or-sharepoint-library"></a>Установка [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] из веб-портала или библиотеки SharePoint 
  
 Вы можете запустить [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] на веб-портале [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] или сайте SharePoint, интегрированном с [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Сведения см. в разделе [Запуск построителя отчетов](../../reporting-services/report-builder/start-report-builder.md).  

::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
  
### <a name="sharepoint-site-integrated-with-includessrsnoversionincludesssrsnoversion-mdmd"></a>Сайт SharePoint, интегрированный с [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]
  
 На сайте SharePoint, интегрированном с [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], если меню **Создать документ** не содержит пункты **Отчет построителя отчетов**, **Модель построителя отчетов**и **Источник данных отчета**, необходимо добавить типы их содержимого в библиотеку SharePoint. Дополнительные сведения см. в разделе [Добавление типов содержимого служб Reporting Services в библиотеку SharePoint](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md).  

::: moniker-end
 
## <a name="install-includessrbnoversionincludesssrbnoversionmd-with-system-center-configuration-manager"></a>Установка [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] с System Center Configuration Manager 
  
 Администратор также может принудительно отправить программу на компьютер пользователя с помощью таких программ, как System Center Configuration Manager. Сведения по использованию определенного программного обеспечения для установки [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]можно получить в документации по этому программному обеспечению. Дополнительные сведения см. в разделе [Сайт System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).  
  
> [!IMPORTANT]  
>  Для запуска операций из командной строки в Windows Vista и Windows 7 требуются повышенные разрешения безопасности; эти ОС будут запрашивать разрешения на выполнение командной строки. Эта установка выполняется не автоматически. Для проведения автоматической установки необходимо запустить командную строку от имени администратора.  
  
## <a name="system-requirements"></a>Требования к системе
  
 См. раздел **системные требования** на [странице загрузки построителя отчетов](https://go.microsoft.com/fwlink/?LinkID=734968) в центре загрузки Майкрософт.
  
##  <a name="download"></a> Установка [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] с сайта загрузки  
  
1.  На [странице построителя отчетов в центре загрузки Майкрософт](https://go.microsoft.com/fwlink/?LinkID=734968) нажмите **Скачать**.  
  
2.  После завершения загрузки [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] нажмите кнопку  **Запустить**.  
  
     Будет запущен мастер SQL Server [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] .  
  
3.  Примите лицензионное соглашение и нажмите кнопку **Далее**.  
  
4.  На странице **Целевой сервер по умолчанию** можно по желанию привести URL-адрес целевого сервера отчетов, если он отличается от адреса по умолчанию. Нажмите кнопку **Далее**.  
  
    > [!NOTE]  
    >  Если планируется работать со средством [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] , когда оно подключено к серверу отчетов, на данном этапе будет удобнее указать URL-адрес сервера. Вы также можете сделать это в диалоговом окне **Параметры** в [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)].  
  
5.  Нажмите кнопку **Установить**, чтобы завершить установку [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)].  
  
## <a name="to-install-includessrbnoversionincludesssrbnoversionmd-from-a-share"></a>Установка [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] из общей папки  
  
1.  Свяжитесь с администратором, чтобы узнать расположение файла ReportBuilder3.msi, который нужно запустить для установки [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] на локальном компьютере.  
  
2.  Перейдите к файлу ReportBuilder3.msi, который представляет собой пакет установщика Windows (MSI) для [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)], и щелкните его.  
  
     Будет запущен мастер SQL Server [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] .  
  
3.  Выполните оставшиеся действия в [Установка построителя отчетов с сайта загрузки](#download).  
  
## <a name="to-install-includessrbnoversionincludesssrbnoversionmd-from-the-command-line"></a>Установка [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] из командной строки 

 Кроме того, можно установить [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] из командной строки, задав аргументы для настройки установки. Кроме стандартных внутренних параметров MSI, вы можете использовать пользовательские параметры, предоставляемые [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]: RBINSTALLDIR и REPORTSERVERURL. RBINSTALLDIR указывает корневой каталог установки для [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]. REPORTSERVERURL указывает сервер отчетов по умолчанию, используемый [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] для сохранения отчетов на сервере.  
  
 Если необходимо произвести установку в полностью автоматическом режиме, без какого-либо взаимодействия с пользовательским интерфейсом, укажите параметр **/quiet** . Флаг параметра подавляет сообщения об ошибках установки. В связи с этим при использовании автоматической установки рекомендуется включить параметр **/l** , указывающий необходимость ведения журнала.   
  
1.  На [странице построителя отчетов в центре загрузки Майкрософт](https://go.microsoft.com/fwlink/?LinkID=734968)нажмите **Скачать**.  
  
2.  После завершения скачивания [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] нажмите кнопку  **Сохранить**.  
  
3.  В меню **Пуск** выберите команду **Выполнить**.  
  
4.  В окне **Открыть** введите **cmd**.  
  
5.  В окне командной строки перейдите к папке, в которой был сохранен файл ReportBuilder3.msi.  
  
6.  Введите команду в следующем формате:  
  
     `msiexec/i ReportBuilder3.msi /option [value] [/option [value]]`  
  
     Для установки [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] нужны два параметра: RBINSTALLDIR и REPORTSERVERURL. Вам не обязательно включать эти аргументы в командную строку. Ниже приведена базовая команда:  
  
     `msiexec /i ReportBuilder3_x86.msi /quiet`  
  
7.  Нажмите клавишу ВВОД, чтобы выполнить команду.  
  
## <a name="set-includessrbnoversionincludesssrbnoversionmd-defaults"></a>Установка значений по умолчанию для [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]  
  
-   После установки [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]вы можете задать некоторые параметры по умолчанию. Щелкните **Файл** > **Параметры**.  
  
     Рекомендуем настроить стандартный веб-портал [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] или сайт SharePoint. Дополнительные сведения см. в статье [Set default options for Report Builder](../../reporting-services/report-builder/set-default-options-for-report-builder.md).  
  
-   Щелкните **Построитель отчетов** .  
  
     Если сервер отчетов отсутствует в списке существующих серверов, закройте диалоговое окно **Открытие отчета**, а затем в нижней части [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] нажмите кнопку **Подключиться**, чтобы подключиться к серверу.  
  
## <a name="see-also"></a>См. также:  
 [Запуск построителя отчетов](../../reporting-services/report-builder/start-report-builder.md)   
 [Удаление построителя отчетов](../../reporting-services/install-windows/uninstall-report-builder.md)  
  
  

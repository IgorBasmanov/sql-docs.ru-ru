---
title: Установка или удаление надстройки PowerPivot для SharePoint (SharePoint 2013) | Документация Майкрософт
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: database-engine
ms.topic: conceptual
ms.assetid: fe13ce8b-9369-4126-928a-9426f9119424
author: minewiskan
ms.author: owend
manager: craigg
ms.openlocfilehash: b4cf0d930d6e32c28c9b89a8a430b06368ed63a1
ms.sourcegitcommit: a1adc6906ccc0a57d187e1ce35ab7a7a951ebff8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68888633"
---
# <a name="install-or-uninstall-the-powerpivot-for-sharepoint-add-in-sharepoint-2013"></a>Установка или удаление надстройки PowerPivot для SharePoint (SharePoint 2013)
  [!INCLUDE[ssGeminiShortvnext](../../../includes/ssgeminishortvnext-md.md)] представляет собой набор компонентов сервера приложений и служб, которые обеспечивают доступ к данным [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] в ферме [!INCLUDE[SPS2013](../../../includes/sps2013-md.md)] . Надстройка [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] для SharePoint (**spPowerpivot.msi**) — это пакет установщика, используемый для установки компонентов сервера приложений.  
  
-   Надстройка не требуется для развертываний SharePoint 2010.  
  
-   Надстройка не требуется при развертывании в конфигурации с одиночным сервером, на котором установлено программное обеспечение SharePoint 2013 и [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] в режиме интеграции с SharePoint. Компоненты, которые устанавливаются этой надстройкой, включаются при установке сервера [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] в режиме интеграции с SharePoint. Схемы примеров развертывания с надстройкой см. в разделе [топологии развертывания для SQL Server функций бизнес-аналитики в SharePoint](../../../sql-server/install/deployment-topologies-for-sql-server-bi-features-in-sharepoint.md).  
  
 **Примечание.** В [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] этом разделе описывается установка файлов решения и [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] средства настройки SharePoint 2013. После установки ознакомьтесь со следующим разделом, чтобы получить сведения о средстве настройки и дополнительных компонентах, [настройке PowerPivot &#40;и развертывании решений SharePoint 2013&#41;](https://docs.microsoft.com/analysis-services/instances/install-windows/configure-power-pivot-and-deploy-solutions-sharepoint-2013).  
  
 Дополнительные сведения о том, как загрузить **spPowerPivot.msi**, см. в документации по [Microsoft® SQL Server® 2014 PowerPivot® для Microsoft SharePoint®](https://go.microsoft.com/fwlink/?LinkID=324854).  
  
 **В этом разделе:**  
  
-   [Историческая справка](#bkmk_background)  
  
-   [Куда устанавливать spPowerPivot.msi?](#bkmk_where_to_install)  
  
-   [Требования и необходимые условия](#bkmk_prereq)  
  
-   [Установка PowerPivot для SharePoint](#bkmk_install)  
  
-   [Развертывание файлов решения SharePoint с помощью средства настройки PowerPivot для SharePoint 2013](#bkmk_deploy_solution)  
  
-   [Удаление или восстановление надстройки](#bkmk_remove_addin)  
  
##  <a name="bkmk_background"></a> Историческая справка  
  
-   **Сервер приложений:** [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] в SharePoint 2013 позволяют применять рабочие книги в качестве источников данных, планово обновлять данные, а также использовать панель управления [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] .  
  
     [!INCLUDE[ssGeminiShortvnext](../../../includes/ssgeminishortvnext-md.md)] — это пакет установщика [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows (**spPowerpivot.msi**), который выполняет развертывание клиентских библиотек служб Analysis Services и копирует файлы установки [!INCLUDE[ssGeminiShortvnext](../../../includes/ssgeminishortvnext-md.md)] на компьютер. Установщик не разворачивает и не настраивает функции [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] в SharePoint. По умолчанию устанавливаются следующие компоненты:  
  
    -   [!INCLUDE[ssGeminiShort](../../../includes/ssgeminishort-md.md)] 2013. Этот компонент содержит скрипты PowerShell (PS1-файлы), пакеты решения SharePoint (WSP-файлы) и средство настройки [!INCLUDE[ssGeminiShort](../../../includes/ssgeminishort-md.md)] 2013 для развертывания [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] в ферме SharePoint 2013.  
  
    -   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Поставщик OLE DB для служб Analysis Services (MSOLAP).  
  
    -   Поставщик данных ADOMD.NET.  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Объекты управления аналитикой (AMO-объекты).  
  
-   **Серверные службы:** Если для создания [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] книг, содержащих аналитические данные, используется для Excel, для доступа к этим данным в серверной среде необходимо, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] чтобы службы Excel были настроены с сервером бизнес-аналитики, работающим в режиме интеграции с SharePoint. Вы можете запустить программу установки SQL Server на компьютере, где установлен SharePoint Server 2013 или на другом компьютере, где нет ПО SharePoint. Службы Analysis services нисколько не зависят от SharePoint.  
  
     Дополнительные сведения об установке, отмене установки и настройке серверных служб см. в следующем документе:  
  
    -   [Установка PowerPivot для SharePoint 2013](https://docs.microsoft.com/analysis-services/instances/install-windows/install-analysis-services-in-power-pivot-mode)  
  
    -   [Удаление PowerPivot для SharePoint](../../../sql-server/install/uninstall-power-pivot-for-sharepoint.md)  
  
##  <a name="bkmk_where_to_install"></a> Куда устанавливать spPowerPivot.msi?  
 Рекомендуется устанавливать **spPowerPivot.msi** на всех серверах фермы SharePoint, чтобы обеспечить согласованность конфигурации, в том числе на серверы приложений и серверы веб-интерфейса. В пакет установщика входят поставщики данных служб Analysis Services, а также средство настройки [!INCLUDE[ssGeminiShortvnext](../../../includes/ssgeminishortvnext-md.md)] . При установке **spPowerPivot.msi** вы можете исключить определенные компоненты.  
  
 **Поставщики данных:** Некоторые технологии SharePoint и SQL Server используют поставщики данных Analysis Services, включая службы Excel Services, PerformancePoint Services и Power View. Установка **spPowerPivot.msi** на всех серверах SharePoint обеспечивает полный набор поставщиков данных служб Analysis Services, а также имеется постоянная возможность подключения к PowerPivot по всей ферме.  
  
> [!NOTE]  
>  Необходимо установить поставщики данных служб Analysis services на сервере SharePoint 2013 с помощью **spPowerPivot.msi**. Другие пакеты установщика, доступные в составе пакета дополнительных компонентов [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] , не поддерживаются, поскольку в такие пакеты не входят файлы поддержки SharePoint 2013, которые требуются поставщикам данных для этой среды.  
  
 **Средство настройки:** Средство настройки PowerPivot для SharePoint 2013 требуется только на одном из серверов SharePoint. Однако в многосерверных фермах рекомендуется устанавливать средство настройки по крайней мере на 2 сервера, чтобы обеспечить доступ к средству настройки, даже если один из двух серверов находится вне сети.  
  
##  <a name="bkmk_prereq"></a> Требования и необходимые условия  
  
-   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SharePoint Server 2013.  
  
-   **spPowerPivot.msi** подходит только для 64-разрядной версии в соответствии с требованиями к продуктам и технологиям SharePoint.  
  
-   Сервер [!INCLUDE[ssASCurrent](../../../includes/ssascurrent-md.md)] в режиме PowerPivot. Службы Excel будут использовать экземпляр служб SQL Server Analysis Services в качестве сервера PowerPivot. Службы Analysis services могут работать на локальном или удаленном компьютере.  
  
-   **Разрешения**: Для установки [!INCLUDE[ssGeminiShortvnext](../../../includes/ssgeminishortvnext-md.md)]текущий пользователь должен быть администратором компьютера и группой администраторов фермы SharePoint.  
  
-   Дополнительные сведения [!INCLUDE[ssGeminiShort](../../../includes/ssgeminishort-md.md)] о требованиях и необходимых компонентах см. в требованиях к [оборудованию и программному обеспечению для &#40;Analysis Services Server&#41;в режиме интеграции с SharePoint SQL Server 2014](../../../sql-server/install/hardware-software-requirements-analysis-services-server-sharepoint-mode.md).  
  
##  <a name="bkmk_install"></a>Установка PowerPivot для SharePoint  
 Пакет установщика **spPowerpivot.msi** поддерживает графический пользовательский интерфейс и режим командной строки. При использовании этих методов установки необходимо запустить MSI-файл с правами администратора. После установки ознакомьтесь со следующим разделом, чтобы получить сведения о средстве настройки и дополнительных компонентах, [настройке PowerPivot &#40;и развертывании решений SharePoint 2013&#41;](https://docs.microsoft.com/analysis-services/instances/install-windows/configure-power-pivot-and-deploy-solutions-sharepoint-2013).  
  
### <a name="user-interface-installation"></a>Установка с помощью пользовательского интерфейса  
 Чтобы установить [!INCLUDE[ssGeminiShortvnext](../../../includes/ssgeminishortvnext-md.md)] с помощью графического пользовательского интерфейса, выполните следующие шаги.  
  
1.  Запустите **SpPowerPivot.msi**.  
  
2.  На странице приветствия нажмите кнопку **Далее** .  
  
3.  Прочитайте и примите лицензионное соглашение, затем нажмите кнопку **Далее**.  
  
4.  На странице **Выбор компонентов** все компоненты выбраны по умолчанию.  
  
5.  Нажмите кнопку **Далее**.  
  
6.  Нажмите **Установить** , чтобы завершить установку.  
  
### <a name="command-line-installation"></a>Установка из командной строки  
 Для установки из командной строки откройте командную строку с правами администратора, а затем запустите **spPowerPivot.msi**. Пример:  
  
 `Msiexec.exe /i SpPowerPivot.msi`.  
  
 Чтобы создать журнал установки, можно использовать стандартные переключатели ведения журнала MsiExec. В следующем примере создается файл журнала "Install_Log. txt" с помощью параметра подробного ведения журнала "v".  
  
```  
Msiexec.exe /i SpPowerPivot.msi /L v c:\test\Install_Log.txt  
```  
  
### <a name="quiet-command-line-installation-for-scripting"></a>Автоматическая установка из командной строки для создания скрипта  
 Вы можете использовать параметры **/q** или **/quiet** для автоматической установки, в ходе которой не будут выводиться никакие диалоговые окна или сообщения. Автоматическая установка удобна, если установку надстройки нужно выполнить из скрипта.  
  
> [!IMPORTANT]  
>  При использовании параметра **/q** для автоматической установки из командной строки условия лицензионного соглашения не отображаются. Независимо от метода установки, использование программного обеспечения регулируется лицензионным соглашением и пользователь несет ответственность за его соблюдение.  
  
 **Выполнение автоматической установки**  
  
1.  Откройте командную строку **с разрешениями администратора**.  
  
2.  Выполните следующую команду:  
  
    ```  
    Msiexec.exe /i spPowerPivot.msi /q  
    ```  
  
### <a name="command-line-installation-to-include-specific-components"></a>Установка из командной строки для включения определенных компонентов  
 Нет необходимости в средстве настройки [!INCLUDE[ssGeminiShortvnext](../../../includes/ssgeminishortvnext-md.md)] на каждом сервере SharePoint, однако рекомендуется установить этот компонент как минимум на двух серверах, чтобы он был доступен в нужный момент.  
  
 При установке spPowerPivot.msi вы можете использовать командную строку для установки конкретных элементов, например поставщиков данных, и не устанавливать средство настройки [!INCLUDE[ssGeminiShortvnext](../../../includes/ssgeminishortvnext-md.md)] . Следующая командная строка представляет собой пример установки всех компонентов, кроме средства настройки.  
  
```  
Msiexec /i spPowerPivot.msi AGREETOLICENSE="yes" ADDLOCAL=" SQL_OLAPDM,SQL_ADOMD,SQL_AMO,SQLAS_SP_Common"  
```  
  
|Параметр|Описание|  
|------------|-----------------|  
|Analysis_Server_SP_addin|Настройка PowerPivot|  
|SQL_OLAPDM|MSOLAP|  
|SQL_ADOMD|Поставщик ADOMD.net|  
|SQL_AMO|Поставщик AMO|  
|SQLAS_SP_Common|Общие компоненты служб Analysis Services для SharePoint 2013|  
  
##  <a name="bkmk_deploy_solution"></a>Развертывание файлов решения SharePoint с помощью средства настройки PowerPivot для SharePoint 2013  
 Три из скопированных на жесткий диск файлов программой spPowerPivot.msi являются файлами решения SharePoint. Область одного файла решения — уровень фермы, а область другого файла — уровень веб-приложения. Это относится к следующим файлам:  
  
-   `PowerPivotFarmSolution.wsp`  
  
-   `PowerPivotFarm14Solution.wsp`  
  
-   `PowerPivotWebApplicationSolution.wsp`  
  
 Файлы решений копируются в следующую папку:  
  
 `C:\Program Files\Microsoft SQL Server\120\Tools\PowerPivotTools\SPAddinConfiguration\Resources`  
  
 После установки MSI-файла запустите средство настройки [!INCLUDE[ssGeminiShortvnext](../../../includes/ssgeminishortvnext-md.md)] для настройки и развертывания решений в ферме SharePoint.  
  
 **Для запуска средства настройки выполните следующие действия.**  
  
 На начальном экране Windows введите "Power" и в результатах поиска приложений щелкните **PowerPivot для SharePoint конфигурации 2013**. Обратите внимание, что результаты поиска могут включать две ссылки, поскольку программа установки [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] устанавливает отдельное средство настройки служб [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] для SharePoint 2010 и SharePoint 2013. Убедитесь, что запущен [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] для средства настройки SharePoint 2013.  
  
 ![два средства настройки PowerPivot](https://docs.microsoft.com/analysis-services/analysis-services/media/as-powerpivot-configtools-bothicons.gif "два средства настройки PowerPivot")  
  
 **Or**  
  
1.  Откройте меню **Пуск**, **Все программы**.  
  
2.  Выберите **Microsoft SQL Server 2014**.  
  
3.  Щелкните **Средства настройки**.  
  
4.  Нажмите **Настройка PowerPivot для SharePoint 2013**.  
  
 Дополнительные сведения о средстве настройки см. в разделе [PowerPivot Configuration Tools](../../power-pivot-sharepoint/power-pivot-configuration-tools.md).  
  
##  <a name="bkmk_remove_addin"></a> Удаление или восстановление надстройки  
  
> [!CAUTION]  
>  При удалении **spPowerPivot.msi** удаляются поставщики данных и средства настройки. Удаление поставщиков данных лишает сервер возможности подключения к PowerPivot.  
  
 Удалить или восстановить [!INCLUDE[ssGeminiShortvnext](../../../includes/ssgeminishortvnext-md.md)] вы можете одним из следующих способов.  
  
1.  **Панель управления Windows:** Выберите **Microsoft SQL Server 2012 PowerPivot для SharePoint 2013**. Нажмите **Удалить** или **Восстановить**.  
  
2.  Запустите spPowerPivot.msi и выберите параметр **Удалить** или **Восстановить** .  
  
 **Командная строка:** Чтобы восстановить или удалить PowerPivot для SharePoint 2013 с помощью командной строки, откройте командную строку **с разрешениями администратора** и выполните одну из следующих команд:  
  
-   Для восстановления выполните следующую команду:  
  
    ```  
    msiexec.exe /f spPowerPivot.msi  
    ```  
  
 OR  
  
-   Для удаления выполните следующую команду:  
  
    ```  
    msiexec.exe /uninstall spPowerPivot.msi  
    ```  
  
  

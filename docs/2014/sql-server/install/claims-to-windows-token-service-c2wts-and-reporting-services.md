---
title: Служба claims to Windows Token Service (C2WTS) и службы Reporting Services | Документация Майкрософт
ms.custom: ''
ms.date: 03/25/2016
ms.prod: sql-server-2014
ms.reviewer: ''
ms.technology: database-engine
ms.topic: conceptual
helpviewer_keywords:
- c2wts.exe.config
- SharePoint mode
- C2WTS
- WSS_WPG
ms.assetid: 4d380509-deed-4b4b-a9c1-a9134cc40641
author: markingmyname
ms.author: maghan
manager: craigg
ms.openlocfilehash: e08be032df493913cc6cebf5ae29d583f26c86ba
ms.sourcegitcommit: 3026c22b7fba19059a769ea5f367c4f51efaf286
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/15/2019
ms.locfileid: "66096551"
---
# <a name="claims-to-windows-token-service-c2wts-and-reporting-services"></a>Служба Claims to Windows Token Service (C2WTS) и службы Reporting Services
  SharePoint Claims to Windows Token Service (c2WTS) является обязательным при использовании [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] режима SharePoint, если вы хотите использовать проверку подлинности windows для источников данных, которые находятся за пределами фермы SharePoint. Это верно, даже если пользователь получает доступ к источникам данных с использованием проверки подлинности Windows, поскольку для связи между клиентским веб-интерфейсом и общей службой [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] всегда используется проверка подлинности Claims.  
  
 Служба c2WTS необходима, даже если источники данных находятся на одном компьютере с общей службой. Однако в этом случае ограниченное делегирование не требуется.  
  
 Токены, созданные службой c2WTS, работают только при наличии ограниченного делегирования (ограничение набором определенных служб) и заданном параметре "Использовать любой протокол проверки подлинности". Как уже отмечалось, если источники данных находятся на одном компьютере с общей службой, ограниченное делегирование не требуется.  
  
 Если в среде используется делегирование, ограниченное Kerberos, то внешние источники данных и служба SharePoint Server должны находиться в одном домене Windows. Любая служба, использующая службу токенов Claims to Windows Service (c2WTS), должна применять делегирование, **ограниченное** Kerberos, чтобы разрешить c2WTS использовать передачу протокола Kerberos для перевода утверждений в учетные данные Windows. Эти требования являются достоверными для всех общих служб SharePoint. Дополнительные сведения см. в разделе [обзор проверки подлинности Kerberos для продуктов Microsoft SharePoint 2010 (https://technet.microsoft.com/library/gg502594.aspx)](https://technet.microsoft.com/library/gg502594.aspx).  
  
 Эта процедура описана в данном разделе.  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]** SharePoint 2013 #124; SharePoint 2010|  
  
## <a name="prerequisites"></a>предварительные требования  
  
> [!NOTE]  
>  Примечание. Некоторые действия по настройке может измениться, или могут не работать в определенных топологиях фермы. Например, установка одиночного сервера не поддерживает службы c2WTS Windows Identity Foundation, поэтому в этой конфигурации фермы требования сценариев делегирования токенов Windows невозможны.  
  
### <a name="basic-steps-needed-to-configure-c2wts"></a>Основные шаги для настройки службы c2WTS  
  
1.  Настройка учетной записи службы c2WTS. Добавьте учетную запись службы к группе локальных администраторов на каждом сервере приложений с c2WTS. Кроме того, убедитесь, что учетная запись имеет следующие права локальной политики безопасности:  
  
    -   работа в качества части операционной системы;  
  
    -   олицетворение клиента после проверки подлинности;  
  
    -   Вход в систему в качестве службы.  
  
     Учетной записи, используемой для службы c2WTS, также должна быть настроена для ограниченного делегирования с переводом протоколов и требуются разрешения на делегирование службам, он необходим для взаимодействия с (т. е. ядро SQL Server, SQL Server Analysis Services). Для настройки делегирования можно использовать оснастку «Active Directory-пользователи и компьютеры».  
  
    1.  Щелкните правой кнопкой мыши каждую учетную запись службы и откройте диалоговое окно свойств. В диалоговом окне перейдите на вкладку **Делегирование** .  
  
        > [!NOTE]  
        >  Примечание. Вкладка делегирования отображается, только если объекту назначено имя участника-службы. Службе c2WTS не требуется имя участника-службы для учетной записи службы c2WTS, однако без имени участника службы вкладка **Делегирование** отображаться не будет. Другой способ настройки ограниченного делегирования заключается в использовании специальной программы, например **ADSIEdit**.  
  
    2.  Основными параметрами, приведенными на вкладке делегирования, являются следующие:  
  
        -   Выберите «Доверять этому пользователю делегирование указанных служб»  
  
        -   Выберите «Использовать любой протокол проверки подлинности»  
  
         Дополнительные сведения см. в разделе «Настройка ограниченного делегирования Kerberos для компьютеров и учетных записей служб» из технического документа, [Настройка проверки подлинности Kerberos для продуктов SharePoint 2010 и SQL Server 2008 R2](http://blogs.technet.com/b/tothesharepoint/archive/2010/07/22/whitepaper-configuring-kerberos-authentication-for-sharepoint-2010-and-sql-server-2008-r2-products.aspx)  
  
2.  Настройка службы c2WTS «AllowedCallers»  
  
     службы c2WTS требуется удостоверения «вызывающим объектам», явно перечисленные в файле конфигурации **c2wtshost.exe.config**. c2WTS принимает запросы от всех пользователей, прошедших проверку подлинности в системе только если он настроен для этого. В этом случае вызывающим является группа Windows WSS_WPG. Файл c2wtshost.exe.confi хранится в следующем расположении:  
  
     **Foundation\v3.5\c2wtshost.exe.config \Program Files\Windows удостоверений**  
  
     В следующем примере показан файл конфигурации:  
  
    ```  
    <configuration>  
      <windowsTokenService>  
        <!--  
            By default no callers are allowed to use the Windows Identity Foundation Claims To NT Token Service.  
            Add the identities you wish to allow below.  
          -->  
        <allowedCallers>  
          <clear/>  
          <add value="WSS_WPG" />  
        </allowedCallers>  
      </windowsTokenService>  
    </configuration>  
    ```  
  
3.  Запустите службу c2WTS операционной системы:  
  
    1.  Задайте использование службой учетной записи, настроенной в предыдущем шаге.  
  
    2.  Измените тип запуска "**автоматического**" и запустите службу.  
  
4.  Запуск SharePoint «Claims to Windows Token Service»: Запустите службу Claims to Windows Token из центра администрирования SharePoint на странице **Управление службами на сервере**. Служба должна быть запущена на сервере, который будет выполнять действие. Например, при наличии сервера, который является интерфейсным веб-сервером, и другого сервера, который служит сервером приложений, где работает общая служба служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , службу c2WTS необходимо запустить только на сервере приложений. Служба c2WTS не требуется на интерфейсном веб-сервере.  
  
## <a name="see-also"></a>См. также  
 [Утверждения to Windows Token Service (c2WTS) (Обзор https://msdn.microsoft.com/library/ee517278.aspx)](https://msdn.microsoft.com/library/ee517278.aspx)   
 [Обзор проверки подлинности Kerberos для продуктов Microsoft SharePoint 2010)https://technet.microsoft.com/library/gg502594.aspx)](https://technet.microsoft.com/library/gg502594.aspx)  
  
  

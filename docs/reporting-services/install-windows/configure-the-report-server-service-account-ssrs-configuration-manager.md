---
title: Настройка учетной записи службы сервера отчетов (диспетчер конфигураций служб SSRS) | Документы Майкрософт
author: maggiesMSFT
ms.author: maggies
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.custom: seodec18
ms.date: 12/10/2018
ms.openlocfilehash: de0ea61c93de1464ebde068ef47d85e89b8a1587
ms.sourcegitcommit: e7d921828e9eeac78e7ab96eb90996990c2405e9
ms.translationtype: MTE75
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68261606"
---
# <a name="configure-the-report-server-service-account-ssrs-configuration-manager"></a>Настройка учетной записи службы сервера отчетов (диспетчер конфигурации служб SSRS)

  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] реализован в виде единой службы, состоящей из веб-службы сервера отчетов, [!INCLUDE[ssRSWebPortal-Non-Markdown](../../includes/ssrswebportal-non-markdown-md.md)]и приложения фоновой обработки, которое предназначено для запланированной обработки отчетов и доставки подписки. В этом разделе описан процесс начальной настройки учетной записи службы, а также изменения учетной записи или пароля при помощи программы настройки служб Reporting Services.  
  
## <a name="initial-configuration"></a>Первоначальная конфигурация

 Задание учетной записи службы сервера отчетов производится во время установки. Служба может быть запущена как от учетной записи пользователя домена, так и от встроенной учетной записи вроде **учетной записи виртуальной службы**. Учетной записи по умолчанию не существует. Начальной учетной записью сервера отчетов будет та, которая была указана на странице **Учетные записи службы** мастера установки.  
  
> [!IMPORTANT]  
> Хотя веб-служба сервера отчетов и [!INCLUDE[ssRSWebPortal-Non-Markdown](../../includes/ssrswebportal-non-markdown-md.md)] являются отдельными приложениями [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] , они выполняются в единой архитектуре служб с одним и тем же удостоверением процесса сервера отчетов.

> [!NOTE]  
> Встроенные учетные записи служб Windows (Local Service или Network Service) не поддерживаются в качестве учетных записей служб для сервера отчетов на компьютере, который является контроллером домена.
  
## <a name="changing-the-service-account"></a>Смена учетной записи службы

 Для просмотра и изменения реквизитов учетной записи службы всегда используйте программу настройки диспетчера конфигураций [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Сведения об удостоверении службы внутренне сохраняется в нескольких разных местах. Использование этого средства гарантирует, что при смене учетной записи или пароля все необходимые ссылки будут соответствующим образом обновлены. Для обеспечения доступности сервера отчетов диспетчер конфигураций [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] выполняет следующие дополнительные шаги:  
  
- Автоматически добавляет новую учетную запись к группе сервера отчетов, созданной на локальном компьютере. Эта группа указывается в списках управления доступом (ACL), которые обеспечивают защиту файлов служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
- Автоматически обновляет разрешения на вход в экземпляр компонента [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)] , на котором размещена база данных сервера отчетов. Новая учетная запись будет добавлена к роли **RSExecRole**.  
  
     Старое имя входа в базе данных автоматически не удаляется. Не забывайте удалять учетные записи, которые больше не используются. Дополнительные сведения см. в статье [Администрирование базы данных сервера отчетов (службы Reporting Services в собственном режиме)](../../reporting-services/report-server/administer-a-report-server-database-ssrs-native-mode.md) в электронной документации по SQL Server.  
  
     Предоставление новой учетной записи службы разрешений производится только в момент первой настройки подключения к базе данных сервера отчетов. Если подключение к базе данных сервера отчетов настроено на использование учетной записи пользователя домена или учетной записи базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , то на эти настройки обновление учетной записи службы не влияет.  
  
- Автоматически обновляет ключ шифрования, добавляя в него сведения о профиле новой учетной записи.  
  
    > [!NOTE]  
    > Если сервер отчетов является частью масштабного развертывания, то изменяется только обновляемый сервер отчетов. При изменении учетной записи сервера ключи шифрования для других серверов отчетов в развертывании не изменяются.  

## <a name="to-configure-the-report-server-service-account"></a>Настройка конфигурации учетной записи службы сервера отчетов  
  
1. Запустите диспетчер конфигурации служб [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] и подключитесь к серверу отчетов.  
  
2. На странице «Учетная запись службы» выберите параметр, указывающий тип используемой учетной записи.  
  
3. Если выбрана учетная запись пользователя Windows, укажите новую учетную запись и пароль. Имя учетной записи не может превышать в длину 20 символов.  
  
     Если сервер отчетов развернут в сети, где поддерживается проверка подлинности по протоколу Kerberos, для указанной учетной записи пользователя домена необходимо зарегистрировать имя участника-службы (SPN) сервера отчетов. Дополнительные сведения см. в статье [Регистрация имени субъекта-службы (SPN) для сервера отчетов](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md).  
  
4. Нажмите кнопку **Применить**.  
  
5. При получении запроса на создание резервной копии симметричного ключа укажите местоположение и имя файла резервной копии симметричного ключа, введите пароль для его блокировки, а затем нажмите кнопку **ОК**.  
  
6. Если сервер отчетов пользуется для подключения к базе данных сервера отчетов учетной записью службы, то ее имя и пароль будут обновлены в сведениях о соединении. Для этого потребуется соединение с базой данных. Если откроется диалоговое окно [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Database Connection** dialog box appears, enter credentials that have permission to connect to the database, and then click **OK**.  
  
7. Получив запрос на восстановление симметричного ключа, введите пароль, указанный на шаге 5, а затем нажмите кнопку **ОК**.  
  
8. Ознакомьтесь с сообщениями о состоянии на панели «Результаты», чтобы убедиться в успешном выполнении всех задач.  
  
## <a name="choosing-an-account"></a>Выбор учетной записи

 Чтобы получить наилучший результат, укажите учетную запись, обладающую разрешениями сетевого подключения и имеющую доступ к контроллерам домена, шлюзам и SMTP-серверам предприятия. В следующей сводной таблице перечислены учетные записи и рекомендации по их использованию.  
  
|Учетная запись|Объяснение|  
|-------------|-----------------|  
|Учетная запись пользователя домена|При наличии учетной записи пользователя домена Windows, обладающей минимумом разрешений, необходимым для работы сервера отчетов, лучше пользоваться ей.<br /><br /> Рекомендуется пользоваться учетной записью пользователя домена, поскольку она изолирует службу сервера отчетов от других приложений. Запуск нескольких приложений от имени общей учетной записи (например, «Сетевая служба») увеличивает риск получения возможности управления над сервером отчетов, так как в случае нарушения защиты в одном из приложений злоумышленнику станут доступны все приложения, запускаемые от имени той же учетной записи.<br /><br /> При использовании учетной записи пользователя домена придется периодически менять пароль, если в организации действует политика истечения срока действия паролей. Может также потребоваться регистрация службы под учетной записью пользователя. Дополнительные сведения см. в статье [Регистрация имени субъекта-службы (SPN) для сервера отчетов](../../reporting-services/report-server/register-a-service-principal-name-spn-for-a-report-server.md).<br /><br /> Избегайте применения учетных записей локальных пользователей Windows. Как правило, они не предоставляют разрешений, необходимых для доступа к ресурсам на других компьютерах. Дополнительные сведения о том, каким образом применение локальных учетных записей ограничивает функции сервера отчетов, см. в подразделе [Вопросы применения учетных записей локальных пользователей](#localaccounts) далее в этом разделе.|  
|**учетной записи виртуальной службы**|**Учетная запись виртуальной службы** представляет службу Windows. Это встроенная учетная запись, обладающая наименьшими правами доступа, которая имеет разрешение на вход в сеть. Использование этой учетной записи рекомендуется в случае отсутствия учетной записи пользователя домена, если желательно избежать перерывов в обслуживании, вызванных применением политики истечения срока действия паролей.|  
|**Сетевая служба.**|**Сетевая служба** — это встроенная учетная запись, обладающая наименьшими правами доступа, которая имеет разрешение на вход в сеть. <br /><br /> Выбрав учетную запись **Сетевая служба**, постарайтесь уменьшить число других запускаемых от нее служб. Нарушение защиты одного из приложений приведет к компрометации всех остальных приложений, запускаемых от имени той же учетной записи.|  
|**Локальная служба.**|**Локальная служба** — это встроенная учетная запись, похожая на учетную запись локального пользователя Windows, прошедшего проверку подлинности. Службы, запущенные с учетной записью **Локальная служба** , получают доступ к сетевым ресурсам в форме неопределенного сеанса без учетных данных. Эта учетная запись не подходит для развертывания в интрасети, когда сервер отчетов перед открытием отчета и обработкой подписки производит соединение со своей базой данных и контроллером домена, чтобы выполнить проверку подлинности пользователя.|  
|**Локальная система**|**Локальная система** — это учетная запись с широким набором прав доступа, которые не нужны для запуска сервера отчетов. Избегайте использования данной учетной записи при установках сервера отчетов. Лучше вместо нее использовать учетную запись **Сетевая служба** .|  
  
## <a name="localaccounts"></a> Вопросы применения учетных записей локальных пользователей

 Использование локальных учетных записей главным образом обосновано в тех случаях, когда серверу отчетов требуется доступ к удаленным серверам баз данных, почтовым серверам и контроллеру домена. Если сервер отчетов настроен для запуска от имени учетной записи локального пользователя Windows, «Локальная служба» или «Локальная система», следует исходить в первую очередь из установки других параметров настройки, а также методов создания и доставки подписки.  
  
- Запуск службы от имени локальной учетной записи может привести к ограничению параметров настройки позже, во время установления соединения с удаленной базой данных сервера отчетов. В частности, если используется удаленная база данных сервера отчетов, для настройки подключения будет необходимо использовать учетную запись пользователя домена или пользователя базы данных [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], которая обладает разрешением на подключение к удаленному экземпляру [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
- Работа службы под локальной учетной записью накладывает новые требования на создание подписок. Сервер отчетов хранит сведения о пользователе, который создал подписку. Если пользователь создает подписку во время того, как он подключен под учетной записью домена, то служба сервера отчетов будет пытаться подключиться к контроллеру домена для проверки подлинности пользователя при обработке подписки. Если служба запущена от имени локальной учетной записи, то запрос проверки подлинности окажется неуспешным при попытке сервера отчета отправить запрос на удаленный контроллер домена. Чтобы обойти это ограничение, можно использовать модуль проверки подлинности на основе пользовательских форм или подключить всех пользователей к серверу отчета под локальной учетной записью пользователя.  
  
- Работа службы под локальной учетной записью накладывает новые требования на доставку подписок. Некоторые модули доставки имеют сведения об учетных записях пользователя в определении подписок. Если служба сервера отчетов запущена от имени локальной учетной записи, при отправке отчета по адресам электронной почты, основанным на учетных записях пользователей домена, служба сервера отчетов не может получить доступ к удаленному контроллеру домена и найти целевую учетную запись электронной почты.  
  
- Встроенные учетные записи служб Windows (Local Service или Network Service) не поддерживаются в качестве учетных записей служб для сервера отчетов на компьютере, который является контроллером домена.  
  
Следующие правила и ссылки в этом разделе помогут выбрать наилучший подход при развертывании.  
  
- [Настройка учетных записей службы Windows и разрешений](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).  
  
- [Руководство по планированию безопасности учетных записей и служб](http://usergroup.doubletake.com/file_cabinet/download/0x000021733).  
  
## <a name="updating-an-expired-password"></a>Обновление пароля с истекшим сроком действия

 Если служба сервера отчетов запущена от имени учетной записи домена и срок действия пароля истек до его обновления в диспетчере конфигураций Reporting Services, служба не будет запускаться до тех пор, пока не будет задан новый пароль.  
  
 Если срок действия пароля учетной записи службы для компонента [!INCLUDE[ssDE](../../includes/ssde-md.md)] истек, то при попытке подключиться к серверу отчетов возникает ошибка **rsReportServerDatabaseUnavailable** . Эта ошибка устраняется с помощью сброса пароля.  
  
## <a name="troubleshooting-service-identity-update-errors"></a>Устранение неполадок, возникающих при обновлении учетных данных службы

 Внесение изменений в удостоверение службы приводит к возникновению ряда событий, которые включают перезапуск службы, обновление защищенного паролем ключа шифрования, резервирования URL-адресов и сведений о соединении, если учетная запись службы предназначена для подключения к базе данных сервера отчетов. Состояние этих событий отражается в виде уведомлений на панели «Результаты» в нижней части страницы. Если в процессе работы возникли ошибки, то можно попробовать устранить их следующими методами.  
  
- Если не удалось восстановить симметричный ключ, можно предпринять попытку восстановить его вручную при помощи кнопки **Восстановить** на странице "Ключи шифрования". Если сделать это не удается, рассмотрите возможность удаления зашифрованного содержимого. При этом придется повторно создать подписки и сведения о соединении с источником данных, но остальная часть содержимого останется доступной. Дополнительные сведения см. в разделе [Резервное копирование и восстановление ключей шифрования служб Reporting Services](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md).  
  
- Если служба не запустилась, запустите ее вручную при помощи оснастки "Службы" в разделе "Администрирование".  
  
- При обновлении учетной записи службы могут возникнуть ошибки резервирования URL-адресов. Каждое резервирование URL-адресов предусматривает создание дескриптора безопасности, который включает список разграничительного управления доступа DACL, который предоставляет учетной записи службы разрешение на прием запросов по данному URL-адресу. После обновления учетной записи URL-адрес должен быть создан повторно, чтобы обновить список DACL на основе новых сведений об учетной записи. Если резервирование URL-адресов не может быть выполнено повторно, но известно, что учетная запись является действительной, то можно попробовать перезапустить компьютер. Если после этого ошибка не исчезла, попробуйте воспользоваться другой учетной записью.  
  
## <a name="next-steps"></a>Next Steps

 [Настройка URL-адресов сервера отчетов (диспетчер конфигурации служб SSRS)](../../reporting-services/install-windows/configure-report-server-urls-ssrs-configuration-manager.md)   
 [Использование диспетчера конфигурации служб Reporting Services (собственный режим)](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)

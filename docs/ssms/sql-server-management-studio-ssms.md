---
title: SQL Server Management Studio (SSMS)
description: Описание назначения и новых возможностей SQL Server Management Studio (SSMS).
ms.prod: sql
ms.technology: ssms
ms.topic: overview
author: markingmyname
ms.author: maghan
ms.reviewer: ''
f1_keywords:
- sql13.ssms.viewhelp.f1
helpviewer_keywords:
- SQL Server Management Studio
- SQL Server Management Studio for Integration Services
- SQL Server Management Studio for Reporting Services
- SQL Server Management Studio for Analysis Services
ms.custom: ''
ms.date: 05/29/2019
ms.openlocfilehash: 8632bd86a19bdce57114e36247f0b805da6804e7
ms.sourcegitcommit: a1adc6906ccc0a57d187e1ce35ab7a7a951ebff8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68893261"
---
# <a name="what-is-sql-server-management-studio-ssms"></a>Что такое SQL Server Management Studio (SSMS)? 

[!INCLUDE[appliesto-ss-asdb-asdw-xxx-md.md](../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] (SSMS) — это интегрированная среда для управления любой инфраструктурой SQL. Используйте SSMS для доступа, настройки, администрирования и разработки всех компонентов [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], базы данных SQL Azure и хранилища данных SQL, а также управления ими. Среда SSMS предоставляет единую полнофункциональную служебную программу, которая сочетает в себе обширную группу графических инструментов с рядом отличных редакторов сценариев для доступа к службе [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] для разработчиков и администраторов баз данных всех профессиональных уровней.

- [**Скачивание SQL Server Management Studio (SSMS)** ](download-sql-server-management-studio-ssms.md)
- [**Скачивание SQL Server Developer**](https://my.visualstudio.com/Downloads?q=SQL%20Server%20Developer)
- [**Скачивание Visual Studio**](https://www.visualstudio.com/downloads/)

## <a name="sql-server-management-studio-components"></a>Компоненты среды SQL Server Management Studio  
  
|Описание|Компонент|  
|---------------|---------|  
|**Обозреватель объектов** используется для просмотра всех объектов и управления ими в одном экземпляре [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)](или более).|[Обозреватель объектов](../ssms/object/object-explorer.md)|  
|**Обозреватель шаблонов** используется для создания файлов со стандартным текстом, которые можно использовать для ускорения разработки запросов и скриптов, и управления ими.|[Обозреватель шаблонов](../ssms/template/template-explorer.md)|  
|Устаревший **обозреватель решений** используется для создания проектов, применяемых для управления такими элементами администрирования, как скрипты и запросы.|[Обозреватель решений](../ssms/solution/solution-explorer.md)|  
|Использование инструментов для визуального проектирования, включенных в [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)].|[Visual Database Tools](../ssms/visual-db-tools/visual-database-tools.md)|  
|Использование редакторов языков [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] для интерактивного написания и отладки запросов и скриптов.|[Редакторы запросов и текста (среда SQL Server Management Studio)](../relational-databases/scripting/query-and-text-editors-sql-server-management-studio.md)|

## <a name="sql-server-management-studio-for-business-intelligence"></a>SQL Server Management Studio для решения задач бизнес-аналитики

Среда [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] предназначена для доступа к службам [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] и [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], а также для их настройки, администрирования и управления ими. Хотя все три технологии бизнес-аналитики полагаются на среду [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], административные задачи, связанные с каждой из этих технологий, несколько отличаются.
  
> [!NOTE]
> Чтобы создать и изменить решения [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)], [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]и [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] , воспользуйтесь [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull_md.md)], а не [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]. [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull_md.md)] представляет собой среду разработки, основанную на [!INCLUDE[msCoName](../includes/msconame_md.md)][!INCLUDE[vsprvs](../includes/vsprvs-md.md)].
  
### <a name="managing-analysis-services-solutions-using-sql-server-management-studio"></a>Управление решениями Analysis Services в среде SQL Server Management Studio

[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] позволяет управлять объектами служб [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] , например выполнять их резервное копирование и обработку.
  
[!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] позволяет создавать проекты скриптов служб [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] , в которых выполняются разработка и сохранение скриптов с использованием многомерных выражений (MDX), расширений интеллектуального анализа данных (DMX) и XML для аналитики (XMLA). Проекты скриптов служб [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] используются для выполнения задач управления или повторного создания баз данных, кубов и других объектов в экземплярах служб [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] . Например, можно разработать скрипт XMLA в проекте скрипта служб [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] , который создает объекты непосредственно в существующем экземпляре служб [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] . Проекты скриптов служб [!INCLUDE[ssASnoversion](../includes/ssasnoversion_md.md)] могут быть сохранены в составе решения и интегрироваться с контролем исходного кода.
  
Дополнительные сведения об использовании среды [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]см. в разделе [Разработка и реализация с помощью среды SQL Server Management Studio](https://docs.microsoft.com/analysis-services/instances/analysis-services-scripts-project-in-sql-server-management-studio).
  
### <a name="managing-integration-services-solutions-using-sql-server-management-studio"></a>Управление решениями Integration Services в среде SQL Server Management Studio

[!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] позволяет использовать службу [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] для управления пакетами и наблюдения за выполняющимися пакетами. В среде [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)] можно организовать пакеты в папки, выполнять, импортировать и экспортировать пакеты, переносить пакеты служб DTS и обновлять пакеты служб [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] .

### <a name="managing-reporting-services-projects-using-sql-server-management-studio"></a>Управление проектами служб Reporting Service в среде SQL Server Management Studio

Среда SQL Server Management Studio позволяет включать компоненты служб Reporting Services, администрировать серверы и базы данных, управлять ролями и заданиями.

Она реализует функции управления общими расписаниями (в папке «Общие расписания») и базами данных сервера отчетов (ReportServer, ReportServerTempdb). Можно также создать роль RSExecRole в системной базе данных Master, когда производится перемещение базы данных сервера отчетов на новое или другое ядро СУБД (компонент[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../includes/ssde_md.md)]). Дополнительные сведения об этих задачах см. в следующих разделах:  

- [Разделы руководства по использованию среды Management Studio](https://msdn.microsoft.com/60685458-9108-47bf-820a-5e7db454d408)
- [Администрирование баз данных сервера отчетов](../reporting-services/report-server/administer-a-report-server-database-ssrs-native-mode.md)
- [Руководство. Создание роли RSExecRole](../reporting-services/security/create-the-rsexecrole.md)
  
Позволяет включать и настраивать различные функции, задавать для сервера значения по умолчанию, управлять ролями и заданиями. Дополнительные сведения об этих задачах см. в следующих разделах:

- [Руководство. Установка свойств сервера отчетов (среда Management Studio)](https://msdn.microsoft.com/1ed0f84b-b12a-4e49-b65c-a11a99f9093f)
- [Руководство. Создание, удаление и изменение ролей (среда Management Studio)](https://msdn.microsoft.com/3d1d56e6-a283-44a7-8417-36cb4d2c74d1)
- [Разрешение и запрет печати на стороне клиента для служб Reporting Services](https://msdn.microsoft.com/0e709c96-7517-4547-8ef6-5632f8118524)

## <a name="non-english-language-versions-of-sql-server-management-studio-ssms"></a>Локализованные версии SQL Server Management Studio (SSMS)

Блокировка разных языков установки снята. Например, немецкий SSMS можно установить на французскую версию Windows. Если язык операционной системы не соответствует языку SSMS, пользователю следует изменить язык в разделе "Сервис" — "Параметры" — "Международные настройки". В противном случае SSMS будет отображать интерфейс пользователя на английском языке.

Дополнительные сведения о различных настройках языка и региональных параметров с предыдущими версиями см. в разделе [Установка локализованных версий SSMS](install-other-languages.md).

## <a name="support-policy-for-ssms"></a>Политика поддержки для SSMS

- Начиная с SSMS 17.0, команда разработчиков средств SQL внедрила [политику современного жизненного цикла Майкрософт](https://support.microsoft.com/help/30881/modern-lifecycle-policy).
- Ознакомьтесь с исходным [объявлением о выходе политики современного жизненного цикла](https://support.microsoft.com/help/447912/announcing-microsoft-modern-lifecycle-policy).
- Дополнительные сведения см. в статье [Политика современного жизненного цикла: вопросы и ответы](https://support.microsoft.com/help/30882/modern-lifecycle-policy-faq).

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

## <a name="next-steps"></a>Следующие шаги

- [Установка локализованных версий SQL Server Management Studio (SSMS)](install-other-languages.md)
- [Учебник. SQL Server Management Studio](tutorials/tutorial-sql-server-management-studio.md)
- [Учебник. Составление инструкций Transact-SQL](https://msdn.microsoft.com/2addc9be-67d0-423d-a457-192fe9d7d058)
- [Разработка и реализация с помощью SQL Server Data Tools](https://docs.microsoft.com/analysis-services/multidimensional-models/creating-multidimensional-models-using-sql-server-data-tools-ssdt)
- [Службы Reporting Services в SQL Server Data Tools](../reporting-services/tools/reporting-services-in-sql-server-data-tools-ssdt.md)
- [Учебник. SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/tutorials/tutorial-sql-server-management-studio)

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
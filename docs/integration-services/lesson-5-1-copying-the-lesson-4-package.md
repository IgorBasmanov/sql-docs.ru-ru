---
title: Шаг 1. Копирование пакета, созданного на занятии 4 | Документация Майкрософт
ms.custom: ''
ms.date: 01/08/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 8aa7d690-4649-4c0a-ac6f-9504637ee426
author: janinezhang
ms.author: janinez
ms.openlocfilehash: f4145d46016175ce16755d238fd805a0d3613249
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68055750"
---
# <a name="lesson-5-1-copy-the-lesson-4-package"></a>Занятие 5-1. Копирование пакета занятия 4

[!INCLUDE[ssis-appliesto](../includes/ssis-appliesto-ssvrpluslinux-asdb-asdw-xxx.md)]



В этой задаче будет создана копия пакета **Lesson 4.dtsx**, созданного на занятии 4. Если вы не прошли занятие 4, можно добавить готовый пакет занятия 4, прилагаемый к учебнику по проекту, скопировать его и работать с копией. Полученная копия будет использоваться на протяжении всего занятия 5.  
  
## <a name="create-the-lesson-5-package"></a>Создание пакета занятия 5  
  
Если вы копируете готовый пакет занятия 4, используйте описанную ниже процедуру.  Чтобы скопировать образец занятия 4, см. следующий раздел.

1.  Если средства [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Data Tools еще не открыты, нажмите кнопку **Пуск** > **Все программы** > **Microsoft SQL Server 2017** и выберите пункт **SQL Server Data Tools**.

2.  В меню **Файл** выберите команду **Открыть** > **Проект или решение**, выберите папку **Учебник по службам SSIS** и нажмите **Открыть**.  Затем дважды щелкните файл **SSIS Tutorial.sln**.

3.  В **обозревателе решений** правой кнопкой мыши щелкните файл **Lesson 4.dtsx** и выберите команду **Копировать**.

4.  В **обозревателе решений** правой кнопкой мыши щелкните узел **Пакеты служб SSIS** и выберите команду **Вставить**.

    По умолчанию копируемому пакету присваивается имя **Lesson 4.dtsx**.

5.  Чтобы открыть пакет, в **обозревателе решений** дважды щелкните файл **Lesson 4.dtsx**.

6.  Щелкните правой кнопкой мыши в области конструктора **Поток управления** и выберите пункт **Свойства**.

7.  В окне **Свойства** измените свойство **Имя** на **Занятие 5**.

8.  Установите флажок для свойства **ИД**, щелкните стрелку раскрывающегося списка и выберите пункт **\<Сформировать новый идентификатор>** .

## <a name="add-the-completed-lesson-4-package"></a>Добавление готового пакета занятия 4

1.  Откройте [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Data Tools, затем откройте проект «Учебник по службам SSIS».

2.  В **обозревателе решений** правой кнопкой мыши щелкните значок **Пакеты служб SSIS** и выберите команду **Добавить существующий пакет**.

3.  В диалоговом окне **Добавление копии существующего пакета** в разделе **Расположение пакета**выберите пункт **Файловая система**.

4.  Нажмите кнопку обзора **(…)** , перейдите в папку с пакетом **Lesson 4.dtsx** на компьютере и нажмите кнопку **Открыть**.

5.  Скопируйте и вставьте пакет занятия 4, как описано в шагах 3–8 в предыдущем разделе.
  
## <a name="go-to-next-task"></a>Переход к следующей задаче  
[Шаг 2. Активация и настройка конфигураций пакетов](../integration-services/lesson-5-2-enabling-and-configuring-package-configurations.md)  
  

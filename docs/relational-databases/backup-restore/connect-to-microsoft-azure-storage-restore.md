---
title: Подключение к службе хранилища Microsoft Azure (восстановление)| Документация Майкрософт
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
f1_keywords:
- sql13.swb.restore.connectstorage.f1
ms.assetid: c0b7d7c8-b878-4b7f-8120-d0c6917b583f
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 84dfb4e9f5027650ae3a35146b13f70fd0f84a2e
ms.sourcegitcommit: b2464064c0566590e486a3aafae6d67ce2645cef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68076163"
---
# <a name="connect-to-microsoft-azure-storage-restore"></a>Подключение к службе хранилища Microsoft Azure (восстановление)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Это диалоговое окно позволяет указать сведения для подключения к учетной записи хранения Windows Azure для получения файлов в учетной записи хранения. После ввода необходимой информации нажмите кнопку **Подключить** , чтобы установить соединение с хранилищем Windows Azure.  
  
## <a name="windows-azure-storage-account"></a>Учетная запись хранения Windows Azure  
 **Учетная запись хранения**  
 Выберите, введите или вставьте имя учетной записи хранения Windows Azure. В раскрывающемся списке указаны ранее используемые учетные записи.  
  
 **Ключ учетной записи**  
 Укажите ключ доступа к учетной записи хранения Windows Azure.  
  
 Флажок**Использовать защищенные конечные точки (HTTPS)**  
 Установите этот флажок, чтобы установить безопасное соединение с хранилищем Windows Azure — рекомендуется.  
  
 Флажок**Сохранить мой ключ учетной записи**  
 Установите этот флажок, чтобы SQL Server запоминал ключ доступа для этой учетной записи хранения.  
  
### <a name="sql-credential"></a>Учетные данные SQL  
 **Выбор существующих учетных данных**  
 Выберите существующие учетные данные SQL, соответствующие учетной записи хранения и ключу доступа.  
  
 **Создание новых учетных данных**  
 Выберите этот параметр, чтобы создать новые учетные данные, используя учетную запись хранения и ключ доступа. Введите имя учетных данных в поле **Имя учетных данных** .  
  
  

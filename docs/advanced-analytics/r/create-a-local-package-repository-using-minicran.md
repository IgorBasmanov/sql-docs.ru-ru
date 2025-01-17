---
title: Создание локального репозитория пакетов R с помощью miniCRAN
description: Используйте miniCran для обнаружения, сборки и установки зависимостей пакета R в единый консолидированный пакет.
ms.prod: sql
ms.technology: machine-learning
ms.date: 06/13/2019
ms.topic: conceptual
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: a2324ad662cad2c91bc6e002fd652fed73d8ab3d
ms.sourcegitcommit: 321497065ecd7ecde9bff378464db8da426e9e14
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2019
ms.locfileid: "68715765"
---
# <a name="create-a-local-r-package-repository-using-minicran"></a>Создание локального репозитория пакетов R с помощью miniCRAN
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

пакет [miniCRAN](https://cran.r-project.org/web/packages/miniCRAN/index.html) , созданный [Андре de Врие](https://blog.revolutionanalytics.com/2016/05/minicran-sql-server.html), определяет и скачивает пакеты и зависимости в одну папку, которую можно скопировать на другие компьютеры для автономной установки пакета R.

В качестве входных данных укажите один или несколько пакетов. **miniCRAN** рекурсивно считывает дерево зависимостей для этих пакетов и скачивает только перечисленные пакеты и их зависимости от Cran или аналогичных репозиториев.

В качестве выходных данных **miniCRAN** создает внутренний согласованный репозиторий, состоящий из выбранных пакетов и всех необходимых зависимостей. Затем можно переместить этот локальный репозиторий на сервер и перейти к установке пакетов без подключения к Интернету.

> [!NOTE]
> Опытные пользователи R часто ищут список зависимых пакетов в файле описания для скачанного пакета. Однако у пакетов, перечисленных в Imports, могут быть зависимости второго уровня. По этой причине мы рекомендуем **miniCRAN** для сборки полного набора необходимых пакетов.

## <a name="why-create-a-local-repository"></a>Зачем создавать локальный репозиторий

Целью создания локального репозитория пакетов является предоставление единого расположения, которое администратор сервера или другие пользователи организации могут использовать для установки новых пакетов R на сервере, особенно для тех, у которых нет доступа к Интернету. После создания репозитория его можно изменить, добавив новые пакеты или обновив версию существующих пакетов.

Репозитории пакетов полезны в следующих сценариях:

- **Безопасность**: Многие пользователи R привыкли скачивать и устанавливать новые пакеты R, начиная с CRAN или одного из его зеркальных узлов. Однако в целях безопасности рабочие серверы, работающие [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] обычно, не имеют подключения к Интернету.

- **Упрощенная автономная установка**: Чтобы установить пакет на автономный сервер, необходимо также скачать все зависимости пакетов, используя miniCRAN, что позволит получить все зависимости в правильном формате.

    С помощью miniCRAN можно избежать ошибок зависимостей пакетов при подготовке пакетов к установке с помощью инструкции [CREATE External Library](https://docs.microsoft.com/sql/t-sql/statements/create-external-library-transact-sql) .

- **Улучшенное управление версиями**: В многопользовательской среде существуют веские причины, позволяющие избежать неограниченной установки нескольких версий пакетов на сервере. Используйте локальный репозиторий для обеспечения согласованного набора пакетов, используемых аналитиками. 

> [!TIP]
> MiniCRAN можно также использовать для подготовки пакетов для использования в Машинное обучение Azure. Дополнительные сведения см. в этом блоге: [Использование miniCRAN в МАШИНном обучении Azure с помощью Мишель Усуелли](https://www.r-bloggers.com/using-minicran-in-azure-ml/) 

## <a name="install-minicran"></a>Установка miniCRAN

Сам пакет **miniCRAN** зависит от 18 других пакетов Cran, между которыми является пакет **ркурл** , который имеет зависимость системы от **пакета.** Аналогичным образом пакет **XML** имеет зависимость от **libxml2-которые**. Для разрешения зависимостей рекомендуется сначала создать локальный репозиторий на компьютере с полным доступом к Интернету. 

Выполните следующие команды на компьютере с базовыми инструментами R, R и подключением к Интернету. Предполагается, что это *не* SQL Server компьютере. Следующие команды устанавливают пакет **miniCRAN** и обязательный пакет **ИГРАФ** . В этом примере проверяется, установлен ли пакет, но можно обойти инструкции If и установить пакеты напрямую.

```R
if(!require("miniCRAN")) install.packages("miniCRAN") 
if(!require("igraph")) install.packages("igraph") 
library("miniCRAN")
```

## <a name="set-the-cran-mirror-and-mran-snapshot"></a>Настройка зеркального отображения CRAN и моментального снимка MRAN:

Укажите зеркальный сайт, который будет использоваться при получении пакетов. Например, можно использовать сайт MRAN: или любой другой сайт в вашем регионе, который содержит необходимые пакеты. Если загрузка завершается неудачно, попробуйте другой зеркальный сайт.

```R
CRAN_mirror <- c(CRAN = "https://mran.microsoft.com")
CRAN_mirror <- c(CRAN = "https://cran.cnr.berkeley.edu")
```

## <a name="create-a-local-folder"></a>Создание локальной папки

Создайте локальную папку, например `C:\mylocalrepo` для хранения собранных пакетов. Если вы Повторяйте это часто, вероятно, вам нужно использовать более описательное имя, например "Миникранзупаккажес" или "miniCRANMyRPackagev2".

Укажите папку в качестве локального репозитория. Синтаксис R использует прямую косую черту для имен путей, которые противоположны соглашениям Windows.

```R
local_repo <- "C:/mylocalrepo"
```

## <a name="add-packages-to-the-local-repo"></a>Добавление пакетов в локальный репозиторий

После установки и загрузки **miniCRAN** создайте список, указывающий дополнительные пакеты, которые требуется скачать.

**Не** добавляйте зависимости в этот начальный список. Пакет **ИГРАФ** , используемый **miniCRAN** , создает список зависимостей. Дополнительные сведения об использовании созданного графа зависимостей см. в разделе [Использование miniCRAN для указания зависимостей пакетов](https://cran.r-project.org/web/packages/miniCRAN/vignettes/miniCRAN-dependency-graph.html).

1. Добавьте целевые пакеты, "Zoo" и "прогноз" в переменную.

    ```R
    pkgs_needed <- c("zoo", "forecast")
    ```

2. При необходимости можно построить график зависимостей, но это может быть информативным.
    
    ```R
    plot(makeDepGraph(pkgs_needed))
    ```

3. Создайте локальный репозиторий. Не забудьте изменить версию R, если это необходимо для версии, установленной на экземпляре SQL Server. Версия 3.2.2 SQL Server 2016, версия 3,3 — SQL Server 2017. Если вы выполнили обновление компонента, ваша версия может быть более новой. Дополнительные сведения см. в разделе [Получение сведений о пакете R и Python](../package-management/installed-package-information.md).

    ```R
    pkgs_expanded <- pkgDep(pkgs_needed, repos = CRAN_mirror);
    makeRepo(pkgs_expanded, path = local_repo, repos = CRAN_mirror, type = "win.binary", Rversion = "3.3");
    ```

   На основе этих сведений пакет miniCRAN создает структуру папок, в которой необходимо скопировать пакеты [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] в дальнейшем.

На этом этапе у вас должна быть папка, содержащая необходимые пакеты, а также все необходимые дополнительные пакеты. Путь должен быть похож на следующий пример: C:\mylocalrepo\bin\windows\contrib\3.3 и он должен содержать коллекцию zip-пакетов. Не распакуйте пакеты или переименуйте файлы.

При необходимости выполните следующий код, чтобы вывести список пакетов, содержащихся в локальном репозитории miniCRAN.

```R
pdb <- as.data.frame(pkgAvail(local_repo, type = "win.binary", Rversion = "3.3"), stringsAsFactors = FALSE);
head(pdb);
pdb$Package;
pdb[, c("Package", "Version", "License")]
```

## <a name="add-packages-to-the-instance-library"></a>Добавление пакетов в библиотеку экземпляров

После создания локального репозитория с нужными пакетами переместите репозиторий пакетов на компьютер SQL Server. В следующей процедуре описывается установка пакетов с помощью средств R.

1. Скопируйте папку, содержащую репозиторий miniCRAN, целиком, на сервер, на котором планируется установить пакеты. Эта папка обычно имеет следующую структуру: miniCRAN root > bin > Windows > от участников сообщества > версии > всех пакетов. В следующих примерах предполагается, что папка находится за пределами корневого диска: 

2. Откройте средство R, связанное с экземпляром (например, можно использовать RGUI. exe). Щелкните правой кнопкой мыши **Запуск от имени администратора** , чтобы разрешить средству вносить обновления в систему.

    - Для SQL Server 2017 расположение файла для RGUI — `C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\bin\x64`.

    - Для SQL Server 2016 он расположен в файле RGUI `C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\R_SERVICES\bin\x64`.

3. Получите путь к библиотеке экземпляров и добавьте ее в список путей к библиотекам. В SQL Server 2017 путь выглядит так же, как в следующем примере.

    ```R
    outputlib <- "C:/Program Files/Microsoft SQL Server/MSSQL14.MSSQLSERVER/R_SERVICES/library"
    ```

4. Укажите новое расположение на сервере, куда вы скопировали репозиторий **miniCRAN** , как `server_repo`.

    В этом примере предполагается, что репозиторий скопирован во временную папку на сервере.

    ```R
    inputlib <- "C:/temp/mylocalrepo"
    ```

5. Так как вы работаете в новой рабочей области R на сервере, необходимо также добавить список пакетов для установки.

    ```R
    mypackages <- c("zoo", "forecast")
    ```

6. Установите пакеты, указав путь к локальной копии репозитория miniCRAN.

    ```R
    install.packages(mypackages, repos = file.path("file://", normalizePath(inputlib, winslash = "/")), lib = outputlib, type = "win.binary", dependencies = TRUE);
    ```

7. Из библиотеки экземпляров можно просмотреть установленные пакеты с помощью команды, как в следующем примере:

    ```R
    installed.packages()
    ```

## <a name="see-also"></a>См. также

+ [Получение сведений о пакете](../package-management/installed-package-information.md)
+ [Учебники по R](../tutorials/sql-server-r-tutorials.md)

---
title: Функция функцию SQLAllocHandle | Документация Майкрософт
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLAllocHandle
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLAllocHandle
helpviewer_keywords:
- SQLAllocHandle function [ODBC]
ms.assetid: 6e7fe420-8cf4-4e72-8dad-212affaff317
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 2fcf08a4a55a7c65dbc94219ac908da83ea15bad
ms.sourcegitcommit: c1382268152585aa77688162d2286798fd8a06bb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/19/2019
ms.locfileid: "68344284"
---
# <a name="sqlallochandle-function"></a>Функция SQLAllocHandle
**Соответствия**  
 Представленная версия: Соответствие стандартам ODBC 3,0: ISO 92  
  
 **Сводка**  
 **Функцию SQLAllocHandle** выделяет среду, соединение, инструкцию или дескриптор.  
  
> [!NOTE]  
>  Эта функция является универсальной функцией выделения дескрипторов, которые заменяют функции ODBC 2,0 **SQLAllocConnect**, **SQLAllocEnv**и **SQLAllocStmt**. Чтобы разрешить приложениям, вызывающим **функцию SQLAllocHandle** , работать с ODBC 2. драйверы *x* , вызов **функцию SQLAllocHandle** сопоставляется в диспетчере драйверов с **SQLAllocConnect**, **SQLAllocEnv**или **SQLAllocStmt**, в зависимости от ситуации. Дополнительные сведения см. в разделе "Комментарии". Дополнительные сведения о том, что диспетчер драйверов сопоставляет эту функцию при использовании ODBC 3. Приложение *x* работает с ODBC 2. драйвер *x* см. в разделе [Сопоставление функций замены для обратной совместимости приложений](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Синтаксис  
  
```cpp  
  
SQLRETURN SQLAllocHandle(  
      SQLSMALLINT   HandleType,  
      SQLHANDLE     InputHandle,  
      SQLHANDLE *   OutputHandlePtr);  
```  
  
## <a name="arguments"></a>Аргументы  
 *Параметром handletype*  
 Входной Тип маркера, выводимого **функцию SQLAllocHandle**. Должно иметь одно из следующих значений:  
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 Обработчик SQL_HANDLE_DBC_INFO_TOKEN используется только диспетчером драйверов и драйвером. Приложения не должны использовать этот тип обработчика. Дополнительные сведения о SQL_HANDLE_DBC_INFO_TOKEN см. [в разделе Разработка осведомленности о пуле подключений в драйвере ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).  
  
 *инпусандле*  
 Входной Входной маркер, в контексте которого должен быть выделен новый маркер. Если *параметром handletype* имеет значение SQL_HANDLE_ENV, это SQL_NULL_HANDLE. Если *параметром handletype* имеет значение SQL_HANDLE_DBC, это должен быть обработчик среды, а если он — SQL_HANDLE_STMT или SQL_HANDLE_DESC, то он должен быть маркером соединения.  
  
 *аутпусандлептр*  
 Проверки Указатель на буфер, в который будет возвращен маркер для вновь выделенной структуры данных.  
  
## <a name="returns"></a>Возвращает  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_INVALID_HANDLE или SQL_ERROR.  
  
 При выделении маркера, отличного от обработчика среды, если **функцию SQLAllocHandle** возвращает SQL_ERROR, он устанавливает для *аутпусандлептр* значение SQL_NULL_HDBC, SQL_NULL_HSTMT или SQL_NULL_HDESC в зависимости от значения *параметром handletype*, если только Аргумент Output является пустым указателем. Затем приложение может получить дополнительные сведения из структуры диагностических данных, связанной с маркером в аргументе *инпусандле* .  
  
## <a name="environment-handle-allocation-errors"></a>Ошибки выделения памяти для обработчиков среды  
 Выделение среды выполняется как в диспетчере драйверов, так и в пределах каждого драйвера. Ошибка, возвращаемая функцией **функцию SQLAllocHandle** с *параметром handletype* SQL_HANDLE_ENV, зависит от уровня, в котором произошла ошибка.  
  
 Если диспетчеру драйверов не удается выделить память для  *\*аутпусандлептр* при вызове **функцию SQLAllocHandle** с *параметром handletype* SQL_HANDLE_ENV, или приложение предоставляет указатель null для *аутпусандлептр*,  **Функцию SQLAllocHandle** ВОЗВРАЩАЕТ SQL_ERROR. Диспетчер драйверов устанавливает для **аутпусандлептр* значение SQL_NULL_HENV (если только приложение не предоставил пустой указатель, возвращающий SQL_ERROR). Отсутствует Handle, с помощью которого можно связать дополнительные диагностические сведения.  
  
 Диспетчер драйверов не вызывает функцию выделения среды на уровне драйвера, пока приложение не вызовет **SQLConnect**, **SQLBrowseConnect**или **SQLDriverConnect**. Если ошибка возникает в функции **функцию SQLAllocHandle** на уровне драйвера, то функция **SQLConnect**, **SQLBrowseConnect**или **SQLDriverConnect** диспетчера драйверов возвращает значение SQL_ERROR. Структура диагностических данных содержит значение SQLSTATE IM004 (сбой драйвера **функцию SQLAllocHandle** ). Ошибка возвращается для маркера соединения.  
  
 Дополнительные сведения о потоке вызовов функций между диспетчером драйверов и драйвером см. в разделе [Функция SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="diagnostics"></a>Диагностика  
 Когда **функцию SQLAllocHandle** возвращает SQL_ERROR или SQL_SUCCESS_WITH_INFO, связанное значение SQLSTATE может быть получено путем вызова **SQLGetDiagRec** с соответствующим *параметром handletype* , а *для набора —* значением *инпусандле* . SQL_SUCCESS_WITH_INFO (но не SQL_ERROR) можно вернуть для аргумента *аутпусандле* . В следующей таблице перечислены значения SQLSTATE, обычно возвращаемые функцией **функцию SQLAllocHandle** , и объясняется каждый из них в контексте этой функции. Нотация "(DM)" предшествует описаниям SQLSTATE, возвращаемым диспетчером драйверов. Код возврата, связанный с каждым значением SQLSTATE, — это SQL_ERROR, если не указано иное.  
  
|SQLSTATE|Ошибка|Описание|  
|--------------|-----------|-----------------|  
|01000|Общее предупреждение|Информационное сообщение для конкретного драйвера. (Функция возвращает SQL_SUCCESS_WITH_INFO.)|  
|08003|Соединение не открыто|(DM) аргумент *параметром handletype* был SQL_HANDLE_STMT или SQL_HANDLE_DESC, но соединение, заданное аргументом *инпусандле* , не было открыто. Процесс подключения должен быть успешно завершен (и соединение должно быть открыто), чтобы драйвер выделил оператор или дескриптор дескриптора.|  
|HY000|Общая ошибка|Произошла ошибка, для которой нет определенного SQLSTATE и для которого не определен SQLSTATE для конкретной реализации. Сообщение об ошибке, возвращаемое функцией **SQLGetDiagRec** в буфере **MessageText* , описывает ошибку и ее причину.|  
|HY001|Ошибка выделения памяти|(DM) диспетчеру драйверов не удалось выделить память для указанного маркера.<br /><br /> Драйверу не удалось выделить память для указанного маркера.|  
|HY009|Недопустимое использование пустого указателя|(DM) аргумент *аутпусандлептр* был пустым указателем.|  
|HY010|Ошибка последовательности функций|(DM) аргумент *параметром handletype* был SQL_HANDLE_DBC, а **SQLSetEnvAttr** не был вызван для установки атрибута среды SQL_ODBC_VERSION.<br /><br /> (DM) вызывается асинхронно исполняемая функция для **инпусандле** , и она все еще выполнялась, когда функция **функцию SQLAllocHandle** была вызвана с **параметром handletype** , равным SQL_HANDLE_STMT или SQL_HANDLE_DESC.|  
|HY013|Ошибка управления памятью|Аргумент *параметром handletype* был SQL_HANDLE_DBC, SQL_HANDLE_STMT или SQL_HANDLE_DESC; не удалось обработать вызов функции, так как не удалось получить доступ к базовым объектам памяти, возможно, из-за нехватки памяти.|  
|HY014|Превышено ограничение на количество дескрипторов|Достигнуто ограничение числа дескрипторов, которые могут быть выделены для типа дескриптора, указанного аргументом *параметром handletype* .|  
|HY092|Недопустимый идентификатор атрибута или параметра|(DM) аргумент *параметром handletype* не был: SQL_HANDLE_ENV, SQL_HANDLE_DBC, SQL_HANDLE_STMT или SQL_HANDLE_DESC.|  
|HY117|Подключение приостановлено из-за неизвестного состояния транзакции. Допускаются только функции отключения и только для чтения.|(DM) Дополнительные сведения о состоянии SUSPENDED см. в разделе [функция SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Необязательная функция не реализована|Аргумент *параметром handletype* был SQL_HANDLE_DESC, а драйвер был ODBC 2. драйвер *x* .|  
|HYT01|Время ожидания подключения истекло|Время ожидания соединения истекло до ответа источника данных на запрос. Время ожидания соединения задается через **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Драйвер не поддерживает эту функцию|(DM) аргумент *параметром handletype* был SQL_HANDLE_STMT, а драйвер не является допустимым драйвером ODBC.<br /><br /> (DM) аргумент *параметром handletype* был SQL_HANDLE_DESC, а драйвер не поддерживает выделение дескриптора дескриптора.|  
  
## <a name="comments"></a>Комментарии  
 **Функцию SQLAllocHandle** используется для выделения дескрипторов для сред, соединений, инструкций и дескрипторов, как описано в следующих разделах. Общие сведения об дескрипторах см. [](../../../odbc/reference/develop-app/handles.md)в разделе Handles.  
  
 Если драйвер поддерживает несколько выделений, то в каждый момент времени может быть выделено более одной среды, соединения или маркера инструкций. В ODBC ограничение на количество дескрипторов среды, соединения, инструкций или дескрипторов, которые могут быть выделены одновременно, не определено. Драйверы могут накладывать ограничение на количество определенных типов маркеров, которые могут быть выделены одновременно. Дополнительные сведения см. в документации по драйверу.  
  
 Если приложение вызывает **функцию SQLAllocHandle** с  *\** параметром аутпусандлептр для среды, соединения, инструкции или дескриптора дескриптора, который уже существует, драйвер перезаписывает сведения, связанные с дескриптором. , если приложение не использует пулы соединений (см. раздел "выделение атрибута среды для пулов соединений" Далее в этом разделе). Диспетчер драйверов не проверяет, используется ли уже указанный в  *\*аутпусандлептр* *обработчик* , а также не проверяет предыдущее содержимое маркера перед его перезаписыванием.  
  
> [!NOTE]  
>  Неправильное программирование приложений ODBC для вызова **функцию SQLAllocHandle** в два раза с той же переменной приложения, определенной для  *\*аутпусандлептр* , без вызова **SQLFreeHandle** для освобождения обработчика перед перераспределением. . Перезапись дескрипторов ODBC таким образом может привести к несогласованному поведению или ошибкам в части драйверов ODBC.  
  
 В операционных системах, поддерживающих несколько потоков, приложения могут использовать одну среду, соединение, инструкцию или дескриптор в разных потоках. Поэтому драйверы должны поддерживать защищенный многопоточный доступ к этим сведениям. одним из способов достичь этого, например, является использование критического раздела или семафора. Дополнительные сведения о потоках [см. в](../../../odbc/reference/develop-app/multithreading.md)разделе Многопоточность.  
  
 **Функцию SQLAllocHandle** не задает атрибут среды SQL_ATTR_ODBC_VERSION при вызове для выделения маркера среды. приложение должно быть установлено атрибутом среды, или значение SQLSTATE HY010 (ошибка последовательности функций) будет возвращаться при вызове **функцию SQLAllocHandle** для выделения маркера соединения.  
  
 Для приложений, соответствующих стандартам, **функцию SQLAllocHandle** сопоставляется с **склаллочандлестд** во время компиляции. Разница между этими двумя функциями заключается в том, что **склаллочандлестд** устанавливает атрибут среды SQL_ATTR_ODBC_VERSION в SQL_OV_ODBC3 при вызове с аргументом *параметром handletype* , для которого задано значение SQL_HANDLE_ENV. Это делается потому, что приложения, соответствующие стандартам, всегда имеют формат ODBC 3. приложения *x* . Более того, для этих стандартов не требуется регистрация версии приложения. Это единственное различие между этими двумя функциями. в противном случае они идентичны. **Склаллочандлестд** сопоставляется с **функцию SQLAllocHandle** в диспетчере драйверов. Таким образом, сторонние драйверы не должны реализовывать **склаллочандлестд**.  
  
 Приложения ODBC 3,8 должны использовать:  
  
-   **Функцию SQLAllocHandle, а не склаллочандлестд** , чтобы выделить маркер среды.  
  
-   **SQLSetEnvAttr** , чтобы задать для атрибута среды SQL_ATTR_ODBC_VERSION значение SQL_OV_ODBC3_80.  
  
## <a name="allocating-an-environment-handle"></a>Выделение дескриптора среды  
 Дескриптор среды предоставляет доступ к глобальной информации, такой как допустимые дескрипторы соединений и активные дескрипторы подключений. Общие сведения о дескрипторах среды см. в разделе [дескрипторы среды](../../../odbc/reference/develop-app/environment-handles.md).  
  
 Чтобы запросить обработчик среды, приложение вызывает **функцию SQLAllocHandle** с *параметром handletype* SQL_HANDLE_ENV и *инпусандле* SQL_NULL_HANDLE. Драйвер выделяет память для сведений о среде и передает значение связанного с ним маркера обратно в  *\*аргумент аутпусандлептр* . Приложение передает  *\*значение аутпусандле* во всех последующих вызовах, требующих аргумента обработчика среды. Дополнительные сведения см. [в разделе Выделение маркера среды](../../../odbc/reference/develop-app/allocating-the-environment-handle.md).  
  
 Если в обработчике среды диспетчера драйверов уже существует обработчик среды драйвера, то при установлении соединения **функцию SQLAllocHandle** с *параметром handletype* SQL_HANDLE_ENV не вызывается в этом драйвере, а только **функцию SQLAllocHandle** с *параметром HANDLETYPE* SQL_HANDLE_DBC. Если обработчик окружения драйвера не существует в обработчике диспетчера драйверов, то в драйвере, когда первое подключение, функцию SQLAllocHandle с параметром handletype SQL_HANDLE_ENV и функцию SQLAllocHandle с параметром handletype SQL_HANDLE_DBC. обработчик среды подключен к драйверу.  
  
 Когда диспетчер драйверов обрабатывает функцию **функцию SQLAllocHandle** с *параметром handletype* SQL_HANDLE_ENV, он проверяет ключевое слово **Trace** в разделе [ODBC] сведений о системе. Если задано значение 1, диспетчер драйверов включает трассировку для текущего приложения. Если установлен флаг трассировки, трассировка запускается, когда первый обработчик среды выделяется и заканчивается при освобождении последнего обработчика среды. Дополнительные сведения см. в разделе [Настройка источников данных](../../../odbc/reference/install/configuring-data-sources.md).  
  
 После выделения обработчика среды приложение должно вызвать **SQLSetEnvAttr** в обработчике среды, чтобы задать атрибут среды SQL_ATTR_ODBC_VERSION. Если этот атрибут не задан до вызова **функцию SQLAllocHandle** для выделения маркера соединения в среде, вызов для выделения соединения ВОЗВРАТИТ значение SQLSTATE HY010 (ошибка последовательности функций). Дополнительные сведения см. [в разделе Объявление версии ODBC приложения](../../../odbc/reference/develop-app/declaring-the-application-s-odbc-version.md).  
  
## <a name="allocating-shared-environments-for-connection-pooling"></a>Выделение общих сред для пула подключений  
 Среды могут совместно использоваться несколькими компонентами в одном процессе. Общая среда может использоваться более чем одним компонентом одновременно. Если компонент использует общую среду, он может использовать соединения в составе пула, что позволяет распределять и использовать существующее соединение без повторного создания этого соединения.  
  
 Перед выделением общей среды, которую можно использовать для пула соединений, приложение должно вызвать **SQLSetEnvAttr** , чтобы задать для атрибута среды SQL_ATTR_CONNECTION_POOLING значение SQL_CP_ONE_PER_DRIVER или SQL_CP_ONE_PER_HENV. **SQLSetEnvAttr** в этом случае вызывается с параметром *енвиронменсандле* , для которого задано значение null, что делает атрибут атрибутом уровня процесса.  
  
 После включения пула соединений приложение вызывает **функцию SQLAllocHandle** с аргументом *параметром handletype* , для которого задано значение SQL_HANDLE_ENV. Среда, выделенная этим вызовом, будет неявной общей средой, так как включена поддержка пулов соединений.  
  
 При выделении общей среды используемая среда не определяется до тех пор, пока не будет вызван **функцию SQLAllocHandle** с *параметром handletype* SQL_HANDLE_DBC. На этом этапе диспетчер драйверов пытается найти существующую среду, соответствующую атрибутам среды, запрошенным приложением. Если такой среды не существует, она создается как общая среда. Диспетчер драйверов поддерживает счетчик ссылок для каждой общей среды. При первом создании среды счетчику присваивается значение 1. Если найдена соответствующая среда, в приложение возвращается маркер этой среды, а число ссылок увеличивается. Выделенный таким образом обработчик среды можно использовать в любой функции ODBC, которая принимает в качестве входного аргумента обработчик среды.  
  
## <a name="allocating-a-connection-handle"></a>Выделение дескриптора соединения  
 Дескриптор соединения предоставляет доступ к таким сведениям, как допустимые операторы и дескрипторы дескрипторов для соединения, а также указывает, открыта ли транзакция в данный момент. Общие сведения о дескрипторах подключений см. [](../../../odbc/reference/develop-app/connection-handles.md)в разделе Handles Connections.  
  
 Чтобы запросить обработчик соединения, приложение вызывает **функцию SQLAllocHandle** с *параметром handletype* SQL_HANDLE_DBC. Аргументу *инпусандле* присваивается маркер среды, возвращенный вызовом **функцию SQLAllocHandle** , который выделил этот обработчик. Драйвер выделяет память для сведений о соединении и передает значение связанного с ним маркера обратно в  *\*аутпусандлептр*. Приложение передает  *\*значение аутпусандлептр* во всех последующих вызовах, для которых требуется маркер подключения. Дополнительные сведения см. [в разделе Выделение маркера подключения](../../../odbc/reference/develop-app/allocating-a-connection-handle-odbc.md).  
  
 Диспетчер драйверов обрабатывает функцию **функцию SQLAllocHandle** и вызывает функцию **функцию SQLAllocHandle** драйвера, когда приложение вызывает **SQLConnect**, **SQLBrowseConnect**или **SQLDriverConnect**. (Дополнительные сведения см. в разделе [Функция SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).)  
  
 Если атрибут среды SQL_ATTR_ODBC_VERSION не задан до вызова **функцию SQLAllocHandle** для выделения маркера соединения в среде, вызов для выделения соединения вернет значение SQLSTATE HY010 (ошибка последовательности функций).  
  
 Когда приложение вызывает **функцию SQLAllocHandle** с аргументом *инпусандле* , установленным в SQL_HANDLE_DBC, а также задает общий обработчик среды, диспетчер драйверов пытается найти существующую общую среду, соответствующую атрибутам среды. задается приложением. Если такой среды не существует, создается одна из них со счетчиком ссылок (поддерживаемым диспетчером драйверов), равным 1. Если найдена соответствующая общая среда, этот маркер возвращается приложению, а его число ссылок увеличивается.  
  
 Фактическое соединение, которое будет использоваться, не определяется диспетчером драйверов до тех пор, пока не будет вызван **SQLConnect** или **SQLDriverConnect** . Диспетчер драйверов использует параметры соединения в вызове **SQLConnect** (или ключевые слова подключения в вызове **SQLDriverConnect**) и атрибуты соединения, заданные после выделения соединения, чтобы определить, какое подключение в пуле следует использовать. Дополнительные сведения см. в разделе [Функция SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="allocating-a-statement-handle"></a>Выделение дескриптора инструкции  
 Маркер инструкции предоставляет доступ к сведениям инструкции, таким как сообщения об ошибках, имя курсора и сведения о состоянии для обработки инструкции SQL. Общие сведения о дескрипторах инструкций см. в разделе [дескрипторы инструкций](../../../odbc/reference/develop-app/statement-handles.md).  
  
 Чтобы запросить маркер инструкции, приложение подключается к источнику данных, а затем вызывает **функцию SQLAllocHandle** перед отправкой инструкций SQL. В этом вызове параметру *параметром handletype* должно быть присвоено значение SQL_HANDLE_STMT, а *инпусандле* должен быть задан маркер соединения, возвращенный вызовом **функцию SQLAllocHandle** , который выделил этот обработчик. Драйвер выделяет память для данных инструкции, связывает обработчик инструкции с указанным соединением и передает значение связанного с ним обработчика обратно в  *\*аутпусандлептр*. Приложение передает  *\*значение аутпусандлептр* во всех последующих вызовах, требующих маркер оператора. Дополнительные сведения см. [в разделе Выделение маркера инструкции](../../../odbc/reference/develop-app/allocating-a-statement-handle-odbc.md).  
  
 При выделении дескриптора инструкции драйвер автоматически выделяет набор из четырех дескрипторов и назначает дескрипторы для них в SQL_ATTR_APP_ROW_DESC, SQL_ATTR_APP_PARAM_DESC, SQL_ATTR_IMP_ROW_DESC и SQL_ATTR_IMP_PARAM_DESC атрибуты инструкции. Они называются *неявно* выделенными дескрипторами. Сведения о том, как явно выделить дескриптор приложения, см. в следующем разделе "Выделение дескриптора дескриптора".  
  
## <a name="allocating-a-descriptor-handle"></a>Выделение дескриптора дескриптора  
 Когда приложение вызывает **функцию SQLAllocHandle** с *параметром handletype* SQL_HANDLE_DESC, драйвер выделяет дескриптор приложения. Они называются *явно* выделенными дескрипторами. Приложение направляет драйвер для использования явно выделенного дескриптора приложения вместо автоматически выделенного для данного дескриптора инструкции путем вызова функции **SQLSetStmtAttr** с параметром SQL_ATTR_APP_ROW_DESC или SQL_ATTR_APP_ Атрибут PARAM_DESC. Дескриптор реализации не может быть выделен явным образом, а в вызове функции **SQLSetStmtAttr** нельзя указывать дескриптор реализации.  
  
 Явно выделенные дескрипторы связаны с дескриптором соединения вместо дескриптора оператора (так как автоматически назначаемые дескрипторы). Дескрипторы остаются выделены только тогда, когда приложение фактически подключается к базе данных. Так как явно выделенные дескрипторы связаны с дескриптором соединения, приложение может связать явно выделенный дескриптор с более чем одной инструкцией в пределах соединения. Неявно выделенный дескриптор приложения, с другой стороны, не может быть связан с более чем одним дескриптором инструкции. (Он не может быть связан с каким-либо другим маркером инструкции, который был выделен для.) Явно выделенные дескрипторы дескриптора можно освободить явным образом либо с помощью приложения, либо путем вызова **SQLFreeHandle** с *параметром handletype* SQL_HANDLE_DESC или неявно при закрытии соединения.  
  
 При освобождении явно выделенного дескриптора, неявно выделенный дескриптор снова связывается с инструкцией. (Атрибут SQL_ATTR_APP_ROW_DESC или SQL_ATTR_APP_PARAM_DESC для этой инструкции снова задается неявно выделенным дескриптором дескриптора.) Это справедливо для всех инструкций, которые были связаны с явно выделенным дескриптором в соединении.  
  
 Дополнительные сведения о дескрипторах см. в разделе [дескрипторы](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="code-example"></a>Пример кода  
 См. [Пример программы ODBC](../../../odbc/reference/sample-odbc-program.md), [функция SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md), [Функция SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)и [функция SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md).  
  
## <a name="related-functions"></a>Связанные функции  
  
|Сведения о|См.|  
|---------------------------|---------|  
|Исполнение инструкции SQL|[Функция SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Исполнение подготовленной инструкции SQL|[Функция SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Освобождение среды, соединения, инструкции или дескриптора|[Функция SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Подготовка инструкции к выполнению|[Функция SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|Установка атрибута соединения|[Функция SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Задание поля дескриптора|[Функция SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Задание атрибута среды|[Функция SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|Задание атрибута инструкции|[Функция SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>См. также  
 [Справочник по API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Файлы заголовков ODBC](../../../odbc/reference/install/odbc-header-files.md)

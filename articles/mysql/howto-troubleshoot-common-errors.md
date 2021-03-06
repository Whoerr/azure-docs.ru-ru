---
title: Устранение распространенных ошибок — База данных Azure для MySQL
description: Сведения о том, как устранять распространенные ошибки миграции, которые происходят у начинающих пользователей Базы данных Azure для MySQL
author: savjani
ms.service: mysql
ms.author: pariks
ms.custom: mvc
ms.topic: overview
ms.date: 8/20/2020
ms.openlocfilehash: ca75416a66bcf2c90028c7f1dc11fbe23a9a9bd9
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/21/2021
ms.locfileid: "98631373"
---
# <a name="common-errors"></a>Распространенные ошибки

База данных Azure для MySQL — это полностью управляемая служба на базе версии MySQL для сообщества. Функциональные возможности MySQL в среде управляемой службы могут отличаться от возможностей MySQL в вашей собственной среде. Из этой статьи вы узнаете о некоторых распространенных ошибках, с которыми могут столкнуться пользователи при миграции в службу "База данных Azure для MySQL" или осуществлении в ней разработки.

## <a name="common-connection-errors"></a>Распространенные ошибки при подключении

#### <a name="error-1184-08s01-aborted-connection-22-to-db-db-name-user-user-host-hostip-init_connect-command-failed"></a>ERROR 1184 (08S01). Прервано подключение 22 к базе данных "db-name" пользователя "user" узла "hostIP" (сбой команды init_connect)
Описанная выше ошибка возникает после успешного входа и настройки сеанса, но перед выполнением любой команды. Приведенное выше сообщение указывает, что для параметра сервера init_connect задано неверное значение, что приводит к сбою инициализации сеанса.

Некоторые параметры сервера, такие как require_secure_transport, не поддерживаются на уровне сеанса, поэтому попытка изменить значения этих параметров с помощью init_connect может привести к ошибке 1184 при подключении к серверу MySQL, как показано ниже

mysql> show databases; ОШИБКА 2006 (HY000). Не удается обнаружить сервер MySQL. Нет соединения. Попытка повторного подключения... Идентификатор подключения:    64897. Текущая база данных: *** NONE **_ОШИБКА 1184 (08S01). Прервано подключение 22 к базе данных "db-name" пользователя "user" узла "hostIP" (сбой команды init_connect)

_ *Решение**. Следует сбросить значение init_connect на вкладке "Параметры сервера" на портале Azure и задать только поддерживаемые параметры сервера с помощью параметра init_connect. 


## <a name="errors-due-to-lack-of-super-privilege-and-dba-role"></a>Ошибки из-за отсутствия разрешения SUPER и роли администратора баз данных

Служба не поддерживает разрешение SUPER и роль администратора баз данных. В результате вы можете столкнуться с некоторыми распространенными ошибками, перечисленными ниже.

#### <a name="error-1419-you-do-not-have-the-super-privilege-and-binary-logging-is-enabled-you-might-want-to-use-the-less-safe-log_bin_trust_function_creators-variable"></a>ОШИБКА 1419. У вас нет разрешения SUPER, и включено ведение двоичного журнала (вы *можете* использовать менее безопасную переменную log_bin_trust_function_creators)

Такая ошибка может произойти при создании функции или триггера, как показано ниже, или при импорте схемы. Инструкции DDL, например CREATE FUNCTION или CREATE TRIGGER, записываются в двоичный журнал, поэтому их может выполнять вторичная реплика. У потока реплики SQL есть полные права доступа, которые можно использовать для повышения привилегий. Для защиты от этой опасности ядро MySQL помимо обычного обязательного разрешения CREATE ROUTINE требует для серверов с включенным ведением журнала в двоичном формате наличия разрешения SUPER. 

```sql
CREATE FUNCTION f1(i INT)
RETURNS INT
DETERMINISTIC
READS SQL DATA
BEGIN
  RETURN i;
END;
```

**Решение**.  Чтобы устранить эту ошибку, на портале в колонке [параметров сервера](howto-server-parameters.md) установите для log_bin_trust_function_creators значение 1. Запустите выполнение инструкций DDL или импортируйте схему, чтобы создать нужные объекты, и восстановите предыдущее значение параметра log_bin_trust_function_creators после создания.

#### <a name="error-1227-42000-at-line-101-access-denied-you-need-at-least-one-of-the-super-privileges-for-this-operation-operation-failed-with-exitcode-1"></a>ОШИБКА 1227 (42000) в строке 101. Доступ запрещен — для выполнения этой операции необходимо иметь (по крайней мере одно) разрешение SUPER; операция завершилась ошибкой с кодом выхода 1

Описанная выше ошибка может произойти при импорте файла дампа или создании процедуры, содержащей предложения [DEFINER](https://dev.mysql.com/doc/refman/5.7/en/create-procedure.html). 

**Решение**.  Чтобы устранить эту ошибку, пользователь с правами администратора может предоставить права на создание или выполнение процедур с помощью команды GRANT, как показано в следующих примерах.

```sql
GRANT CREATE ROUTINE ON mydb.* TO 'someuser'@'somehost';
GRANT EXECUTE ON PROCEDURE mydb.myproc TO 'someuser'@'somehost';
```
В качестве альтернативы можно заменить значения предложения DEFINER именем администратора, выполняющего процесс импорта, как показано ниже.

```sql
DELIMITER;;
/*!50003 CREATE*/ /*!50017 DEFINER=`root`@`127.0.0.1`*/ /*!50003
DELIMITER;;

/* Modified to */

DELIMITER ;;
/*!50003 CREATE*/ /*!50017 DEFINER=`AdminUserName`@`ServerName`*/ /*!50003
DELIMITER ;
```
#### <a name="error-1227-42000-at-line-295-access-denied-you-need-at-least-one-of-the-super-or-set_user_id-privileges-for-this-operation"></a>ОШИБКА 1227 (42000) в строке 295. Доступ запрещен — для выполнения этой операции необходимо иметь (по крайней мере одну) привилегию SUPER или SET_USER_ID.

Описанная выше ошибка может возникнуть при выполнении инструкций CREATE VIEW с DEFINER при импорте файла дампа или выполнении скрипта. В Базе данных Azure для MySQL нельзя назначить какому-либо пользователю привилегии SUPER или SET_USER_ID. 

**Решение**. 
* Для выполнения инструкции CREATE VIEW по возможности используйте пользователя с правами DEFINER. Вполне вероятно, что существует множество представлений с разными пользователями с правами DEFINER, которые имеют разные разрешения, поэтому такое решение может оказаться невозможным.  OR
* Измените файл дампа или скрипт CREATE VIEW, удалив инструкцию DEFINER= из файла дампа. 
* Либо измените файл дампа или скрипт CREATE VIEW, указав в качестве значений DEFINER пользователя с правами администратора, который импортирует или выполняет файл скрипта.

> [!Tip] 
> Для изменения файла дампа используйте sed или perl, а для изменения инструкции DEFINER= — скрипт SQL.

## <a name="common-connection-errors-for-server-admin-login"></a>Распространенные ошибки подключения с именем входа администратора сервера

При создании сервера Базы данных Azure для MySQL имя входа администратора сервера предоставляет пользователь. Это имя входа позволяет создавать базы данных, добавлять новых пользователей и предоставлять разрешения. Если удалено имя входа администратора сервера, отозваны его разрешения или изменен его пароль, в приложении при установке подключения могут отображаться ошибки. Ниже приведены некоторые распространенные ошибки.

#### <a name="error-1045-28000-access-denied-for-user-usernameip-address-using-password-yes"></a>Ошибка 1045 (28000): доступ запрещен для пользователя "имя_пользователя"@"IP-адрес" (используется ли пароль: да)

Описанная выше ошибка возникает в следующих случаях:

* имя пользователя не существует;
* имя пользователя удалено;
* пароль пользователя изменен или сброшен.

**Решение**. 
* Проверьте, существует ли указанное имя пользователя в качестве допустимого на сервере или это имя было случайно удалено. Для выполнения следующего запроса войдите учетную запись пользователя в Базе данных Azure для MySQL:
  ```sql
  select user from mysql.user;
  ```
* Если вы не можете войти в MySQL для выполнения приведенного выше запроса, рекомендуем [сбросить пароль администратора с помощью портала Azure](howto-create-manage-server-portal.md). Сброс пароля на портале Azure позволяет восстановить пользователя, сбросить пароль и восстановить разрешения администратора, чтобы вы смогли входить в систему с помощью данных администратора сервера и выполнять дальнейшие операции.

## <a name="next-steps"></a>Дальнейшие действия
Если вы не нашли ответ, который искали, воспользуйтесь следующими рекомендациями:

- Опубликуйте свой вопрос на [странице вопросов и ответов на сайте Microsoft Q&A](/answers/topics/azure-database-mysql.html) или [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-database-mysql).
- Отправьте сообщение электронной почты команде разработчиков службы "База данных Azure для MySQL" [по этому адресу (@Ask)](mailto:AskAzureDBforMySQL@service.microsoft.com). Учтите, что это не электронный адрес службы технической поддержки.
- Обратитесь в Службу поддержки Azure, [отправив запрос с портала Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Чтобы устранить проблему, связанную с учетной записью, отправьте [запрос в службу поддержки](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) на портале Azure.
- Чтобы отправить отзыв или отправить запрос на новые возможности, создайте запись через [UserVoice](https://feedback.azure.com/forums/597982-azure-database-for-mysql).

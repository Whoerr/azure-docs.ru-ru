---
title: Использование Python для создания запросов к базе данных
description: В этой статье показано, как использовать Python для создания программы, которая подключается к базе данных SQL Azure, и создавать к ней запросы с помощью инструкций Transact-SQL.
titleSuffix: Azure SQL Database & SQL Managed Instance
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: seo-python-october2019, sqldbrb=2, devx-track-python
ms.devlang: python
ms.topic: quickstart
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 12/19/2020
ms.openlocfilehash: 8fb6d319cacf85630b2c400cd18d14487725f925
ms.sourcegitcommit: e7152996ee917505c7aba707d214b2b520348302
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/20/2020
ms.locfileid: "97703985"
---
# <a name="quickstart-use-python-to-query-a-database"></a>Краткое руководство. Использование Python для создания запросов к базе данных
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi-asa.md)]

При работе с этим кратким руководством вы будете использовать Python для подключения к Базе данных SQL Azure, Управляемому экземпляру SQL Azure или базе данных Synapse SQL, а затем выполните запрос данных с помощью инструкций T-SQL.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этим кратким руководством вам понадобится:

- Учетная запись Azure с активной подпиской. [Создайте учетную запись](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) бесплатно.

- База данных, в которой будет выполняться запрос.

  [!INCLUDE[create-configure-database](../includes/create-configure-database.md)]

- [Python](https://python.org/downloads) 3 и связанное с ним программное обеспечение

  # <a name="macos"></a>[macOS](#tab/macos)

  Для установки Homebrew и Python, драйвера ODBC и SQLCMD, а также драйвера Python для SQL Server используйте шаги **1.2**, **1.3** и **2.1** в [create Python apps using SQL Server on macOS](https://www.microsoft.com/sql-server/developer-get-started/python/mac/) (Создание приложений Python с помощью SQL Server на MacOS).

  Дополнительные сведения см. в статье [Установка драйвера ODBC Microsoft на macOS](/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).

  # <a name="ubuntu"></a>[Ubuntu](#tab/ubuntu)

  Используйте `sudo apt-get install python python-pip gcc g++ build-essential`, чтобы установить Python и другие необходимые пакеты.

  Чтобы установить драйвер ODBC, SQLCMD и драйвер Python для SQL Server, см. статью [Step 1: Configure development environment for pyodbc Python development](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#linux) (Шаг 1. Настройка среды разработки для использования pyodbc в Python).

  Дополнительные сведения см. в статье [Установка драйвера ODBC Microsoft на Linux ](/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server).

  # <a name="windows"></a>[Windows](#tab/windows)

  Чтобы установить Python, драйвер ODBC, SQLCMD и драйвер Python для SQL Server, см. статью [Step 1: Configure development environment for pyodbc Python development](/sql/connect/python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development#windows) (Шаг 1. Настройка среды разработки для использования pyodbc в Python).

  Дополнительные сведения см. в статье [Драйвер ODBC Microsoft](/sql/connect/odbc/microsoft-odbc-driver-for-sql-server).

---
Для дальнейшего изучения Python и базы данных в службе "Базы данных SQL Azure" см. [Библиотеки Базы данных SQL Azure для Python](/python/api/overview/azure/sql), репозитория [репозиторий pyodbc](https://github.com/mkleehammer/pyodbc/wiki/) и [выборку pyodbc](https://github.com/mkleehammer/pyodbc/wiki/Getting-started).

## <a name="create-code-to-query-your-database"></a>Создание кода для запроса базы данных 

1. Создайте файл *sqltest.py* в текстовом редакторе.  
   
1. Добавьте следующий код. Получите сведения о подключении из раздела о предварительных требованиях и замените \<server>, \<database>, \<username> и \<password> собственными значениями.
   
   ```python
   import pyodbc
   server = '<server>.database.windows.net'
   database = '<database>'
   username = '<username>'
   password = '<password>'   
   driver= '{ODBC Driver 17 for SQL Server}'
   
   with pyodbc.connect('DRIVER='+driver+';SERVER='+server+';PORT=1433;DATABASE='+database+';UID='+username+';PWD='+ password) as conn:
       with conn.cursor() as cursor:
           cursor.execute("SELECT TOP 3 name, collation_name FROM sys.databases")
           row = cursor.fetchone()
           while row:
               print (str(row[0]) + " " + str(row[1]))
               row = cursor.fetchone()
   ```
   

## <a name="run-the-code"></a>Выполнение кода

1. В командной строке выполните следующую команду:

   ```cmd
   python sqltest.py
   ```

1. Убедитесь, что возвращены базы данных и их параметры сортировки, и закройте командное окно.

## <a name="next-steps"></a>Дальнейшие действия

- [Руководство по разработке первой базы данных в службе "База данных SQL Azure"](design-first-database-tutorial.md)
- [Сведения о драйверах Microsoft Python для SQL Server](/sql/connect/python/python-driver-for-sql-server/)
- [Центр по разработке на Python](https://azure.microsoft.com/develop/python/?v=17.23h)

---
title: Обработку строк ошибок с сопоставлением потоков данных в фабрике данных Azure
description: Узнайте, как выполнять обработку ошибок усечения SQL в фабрике данных Azure с помощью сопоставления потоков данных.
author: kromerm
ms.service: data-factory
ms.topic: conceptual
ms.date: 11/22/2020
ms.author: makromer
ms.openlocfilehash: a7a03ff1a58f50f16ebefce48b9e2772a16a011a
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2021
ms.locfileid: "100386345"
---
# <a name="handle-sql-truncation-error-rows-in-data-factory-mapping-data-flows"></a>Обработку строк ошибок усечения SQL в потоках данных сопоставления фабрики данных

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Распространенный сценарий в фабрике данных при использовании сопоставления потоков данных заключается в записи преобразованных данных в базу данных в базе данных SQL Azure. В этом сценарии распространенным условием ошибки, которое необходимо предотвратить, является усечение столбца.

Существует два основных способа корректной обработке ошибок при записи данных в приемник базы данных в потоках данных ADF:

* При обработке данных базы данных установите для [обработки строк ошибок](./connector-azure-sql-database.md#error-row-handling) приемника значение "продолжить при ошибке". Это автоматический метод Catch-All, который не требует пользовательской логики в потоке данных.
* Кроме того, выполните приведенные ниже действия, чтобы вести журнал столбцов, которые не помещаются в целевой столбец строки, что позволяет потоку данных продолжить работу.

> [!NOTE]
> При включении автоматической обработки ошибок в строках, в отличие от приведенного ниже метода написания собственной логики обработки ошибок, это приведет к незначительному снижению производительности, а дополнительное действие, предпринимаемое ADF, — выполнить 2-фазную операцию для перехвата ошибок.

## <a name="scenario"></a>Сценарий

1. У нас есть таблица целевой базы данных, которая содержит столбец с именем ```nvarchar(5)``` Name.

2. Внутри нашего потока данных мы хотим сопоставлять названия фильмов из нашего приемника с этим целевым столбцом "Name".

    ![Поток данных фильмов 1](media/data-flow/error4.png)
    
3. Проблема заключается в том, что заголовок фильма не умещается в столбце приемника, в котором может содержаться только 5 символов. При выполнении этого потока данных появится сообщение об ошибке следующего вида: ```"Job failed due to reason: DF-SYS-01 at Sink 'WriteToDatabase': java.sql.BatchUpdateException: String or binary data would be truncated. java.sql.BatchUpdateException: String or binary data would be truncated."```

В этом видео рассматривается пример логики обработки строк ошибок в потоке данных.
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4uOHj]

## <a name="how-to-design-around-this-condition"></a>Как создать обход этого условия

1. В этом сценарии максимальная длина столбца "Name" составляет пять символов. Итак, добавим преобразование «Условное разбиение», которое позволит нам записывать строки с «заголовками», длина которых превышает пять символов, одновременно позволяя остальным строкам, которые могут поместиться в это пространство, записываться в базу данных.

    ![условное разбиение](media/data-flow/error1.png)

2. Это преобразование "Условное разбиение" определяет максимальную длину "Title", которая должна быть пятью. Любая строка, которая меньше или равна 5, перейдет в ```GoodRows``` поток. Любая строка, превышающая пять, перейдет в ```BadRows``` поток.

3. Теперь нужно регистрировать строки, на которые произошел сбой. Добавьте преобразование приемника в ```BadRows``` поток для ведения журнала. Здесь мы будем «автоmap» все поля, чтобы мы зарегистрировали полную запись транзакции. Это файловый выход CSV-файла, разделенный текстом, в один файл в хранилище больших двоичных объектов. Мы назовем файл журнала «badrows.csv».

    ![Неправильные строки](media/data-flow/error3.png)
    
4. Завершенный поток данных показан ниже. Теперь можно разделить строки ошибок, чтобы избежать ошибок усечения SQL и поместить эти записи в файл журнала. В то же время успешные строки могут продолжать записывать в нашу целевую базу данных.

    ![завершение потока данных](media/data-flow/error2.png)

5. Если в преобразовании приемника выбран параметр Обработка строк с ошибками и задано значение "выходные строки ошибок", ADF автоматически создаст выходные данные CSV-файла для данных строк вместе с сообщениями об ошибках, сообщаемыми драйвером. Вам не нужно вручную добавлять эту логику в поток данных с помощью этого альтернативного параметра. Этот вариант приведет к незначительному снижению производительности, так что ADF может реализовать 2-фазную методологию для перехвата ошибок и их записи в журнал.

    ![завершение потока данных с строками ошибок](media/data-flow/error-row-3.png)

## <a name="next-steps"></a>Следующие шаги

* Создайте оставшуюся часть логики потока данных с помощью [преобразования потоков данных](concepts-data-flow-overview.md)сопоставления.
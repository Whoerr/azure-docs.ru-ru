---
title: Начало работы с возможностями расширенной разработки в облаке с помощью Azure Percept
description: Начало работы с возможностями расширенной разработки в облаке с помощью записных книжек Jupyter Notebook и Машинного обучения Azure
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: tutorial
ms.date: 02/10/2021
ms.custom: template-tutorial
ms.openlocfilehash: 408040ea68273a29ed9be87b60bd0cc6da736a05
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101658901"
---
# <a name="getting-started-with-advanced-development-in-the-cloud-via-jupyter-notebooks-and-azure-machine-learning"></a>Начало работы с возможностями расширенной разработки в облаке с помощью записных книжек Jupyter Notebook и Машинного обучения Azure

Эта статья описывает процесс настройки рабочей области машинного обучения Azure, отправку предварительно настроенного примера записной книжки Jupyter Notebook в рабочую область, создание вычислительного экземпляра и выполнение ячеек записной книжки в рабочей области.

[Записная книжка](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) выполняет передачу обучения с помощью предварительно обученной модели TensorFlow (MobileNetSSDV2Lite) в AzureML на Python с использованием пользовательского набора данных для обнаружения тарелок.

В записной книжке показано, как начать с набора данных [COCO](https://cocodataset.org/#home), отфильтровать его до нужного класса (тарелки — bowls), а затем преобразовать его в соответствующий формат. Кроме того, можно использовать средство маркировки с открытым кодом [VoTT 2](https://github.com/microsoft/VoTT), чтобы создавать и маркировать ограничивающие прямоугольники в формате PASCAL VOC.

После повторного обучения модели на основе пользовательского набора данных ее можно развернуть в Azure Percept DK с помощью метода обновления двойника модуля.

Затем вы можете проверить вывод модели, просмотрев динамический поток RTSP из Azure Percept Vision SoM. Повторное обучение и развертывание модели выполняются в записной книжке в облаке.

## <a name="prerequisites"></a>Предварительные требования

- [Подписка Машинного обучения Azure](https://azure.microsoft.com/free/services/machine-learning/)
- [Azure Percept DK с подключенным Azure Percept Vision SoM](./overview-azure-percept-dk.md)
- [Процедура освоения Azure Percept DK](./quickstart-percept-dk-set-up.md) выполнена

## <a name="download-azure-percept-github-repository"></a>Скачивание из репозитория GitHub для Azure Percept

1. Перейдите в [репозиторий GitHub для Azure Percept](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview).

1. Клонируйте репозиторий или скачайте ZIP-файл. В верхней части экрана щелкните **Код** -> **Скачать ZIP-файл**.

    :::image type="content" source="./media/advanced-development-cloud/download-zip.png" alt-text="Экран скачивания GitHub":::

## <a name="set-up-azure-machine-learning-portal-and-notebook"></a>Настройка записной книжки и портала Машинного обучения Azure

1. Перейдите на [Портал Машинного обучения Azure](https://ml.azure.com).

1. Выберите каталог, подписку Azure и рабочую область машинного обучения в раскрывающихся меню, а затем щелкните **Начало работы**.

    :::image type="content" source="./media/advanced-development-cloud/machine-learning-portal-get-started.png" alt-text="Экран начала работы с Машинным обучением Azure":::

    Если у вас еще нет рабочей области машинного обучения Azure, щелкните **Создать рабочую область**. На новой вкладке браузера сделайте следующее.

    1. Выберите подписку Azure.
    1. Выберите нужную группу ресурсов.
    1. Укажите имя рабочей области.
    1. Выберите нужный регион.
    1. Выберите выпуск рабочей области.
    1. Щелкните **Проверка и создание**.
    1. На следующей странице просмотрите выбранные параметры и нажмите кнопку **Создать**.

        :::image type="content" source="./media/advanced-development-cloud/workspace-review-and-create.png" alt-text="Экран создания рабочей области в Машинном обучении Azure":::

    Подождите несколько минут для создания рабочей области. После завершения создания рабочей области вы увидите зеленые флажки рядом с ресурсами и сообщение **Развертывание выполнено** в верхней части страницы с обзором Служб машинного обучения.

    :::image type="content" source="./media/advanced-development-cloud/workspace-creation-complete-inline.png" alt-text="Подтверждение создания рабочей области" lightbox= "./media/advanced-development-cloud/workspace-creation-complete.png":::

    После завершения создания рабочей области вернитесь на вкладку портала машинного обучения и щелкните **Начало работы**.

1. На домашней странице рабочей области машинного обучения щелкните **Записные книжки** на левой панели.

    :::image type="content" source="./media/advanced-development-cloud/notebook.png" alt-text="Домашняя страница Машинного обучения Azure":::

1. На вкладке **Мои файлы** щелкните вертикальную стрелку, чтобы отправить файл IPYNB.

    :::image type="content" source="./media/advanced-development-cloud/upload-files.png" alt-text="Домашняя страница с выделенным значком отправки файла":::

1. Найдите и выберите [файл Model.ipynb Transferlearningusing_SSDLiteV2](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) в своей локальной копии репозитория GitHub для Azure Percept. Нажмите кнопку **Открыть**. В окне **Отправка файлов** установите флажок **I trust contents from this file** (Доверять содержимому из этого файла) и нажмите кнопку **Отправить**.

    :::image type="content" source="./media/advanced-development-cloud/select-file.png" alt-text="Экран выбора файла":::

1. В верхней правой строке меню проверьте состояние **Вычисления**. Если вычисления не найдены, щелкните значок **+** , чтобы создать вычисление.

    :::image type="content" source="./media/advanced-development-cloud/no-computes-found-inline.png" alt-text="Состояние вычислений с выделенным значком +" lightbox= "./media/advanced-development-cloud/no-computes-found.png":::

1. В окне **Новый вычислительный экземпляр** введите **Имя вычисления**, выберите **Тип виртуальной машины** и затем **Размер виртуальной машины**. Нажмите кнопку **Создать**.

    :::image type="content" source="./media/advanced-development-cloud/new-compute-instance.png" alt-text="Экран создания вычислительного экземпляра":::

    После нажатия кнопки **Создать** для состояния **Вычисления** будет отображаться синий круг и **\<your compute name> — создание**

    :::image type="content" source="./media/advanced-development-cloud/compute-creating.png" alt-text="Состояние вычислений, когда создание вычисления еще выполняется":::

    Для состояния **Вычисления** отображается зеленый круг и **\<your compute name> — выполнение** после завершения создания вычисления.

    :::image type="content" source="./media/advanced-development-cloud/compute-running.png" alt-text="Состояние вычислений, показывающее, что создание вычисления завершено":::

1. После запуска вычисления выберите ядро **Python 3.6 — AzureML** в раскрывающемся меню рядом со значком **+** .

## <a name="working-with-the-notebook"></a>Работа с записной книжкой

Теперь вы можете запустить записную книжку, чтобы обучить пользовательское средство обнаружения тарелок и развернуть его в наборе разработки. Обязательно выполните каждую ячейку записной книжки отдельно, так как некоторые ячейки перед выполнением сценария должны иметь входные параметры. Ячейки, требующие входных параметров, можно изменить непосредственно в записной книжке. Чтобы запустить ячейку, щелкните значок воспроизведения в левой ее части:

:::image type="content" source="./media/advanced-development-cloud/run-cell-inline.png" alt-text="Записная книжка с выделенным значком ячейки" lightbox= "./media/advanced-development-cloud/run-cell.png":::

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные примеры записных книжек для Службы машинного обучения Azure см. в этом [репозитории](https://github.com/Azure/MachineLearningNotebooks/tree/2aa7c53b0ce84e67565d77e484987714fdaed36e/how-to-use-azureml).

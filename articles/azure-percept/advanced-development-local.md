---
title: Начало работы с возможностями расширенной разработки в локальной среде с помощью Azure Percept
description: Начало работы с возможностями разработки машинного обучения в локальной среде с использованием записных книжек Jupyter и VS Code в Службе машинного обучения Azure
author: elqu20
ms.author: v-elqu
ms.service: azure-percept
ms.topic: tutorial
ms.date: 02/10/2021
ms.custom: template-tutorial
ms.openlocfilehash: 88e71ac21177a13d30176212e97442c63272eca6
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2021
ms.locfileid: "101658971"
---
# <a name="getting-started-with-machine-learning-development-using-vs-code--jupyter-notebooks-on-azureml"></a>Начало работы с возможностями разработки машинного обучения с использованием записных книжек Jupyter и VS Code в Службе машинного обучения Azure

Эта статья описывает процесс настройки рабочей области машинного обучения Azure, создания вычислительного экземпляра, настройки среды разработки Visual Studio Code на локальном компьютере и выполнения ячеек предварительно настроенного примера записной книжки Jupyter в VS Code.

[Записная книжка](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) выполняет передачу обучения с помощью предварительно обученной модели TensorFlow (MobileNetSSDV2Lite) в AzureML на Python с использованием пользовательского набора данных для обнаружения тарелок.

В записной книжке показано, как начать с [набора данных COCO](https://cocodataset.org/#home), отфильтровать его до нужного класса (тарелки — bowls), а затем преобразовать его в соответствующий формат. Кроме того, можно использовать средство маркировки с открытым кодом [VoTT 2](https://github.com/microsoft/VoTT), чтобы создавать и маркировать ограничивающие прямоугольники в формате PASCAL VOC.

После повторного обучения модели на основе пользовательского набора данных ее можно развернуть в Azure Percept DK с помощью метода обновления двойника модуля. Затем вы можете проверить вывод модели, просмотрев динамический поток RTSP из Azure Percept Vision SoM. Повторное обучение и развертывание модели выполняются в записной книжке через удаленный вычислительный экземпляр.

## <a name="prerequisites"></a>Предварительные требования

- [Подписка Машинного обучения Azure](https://azure.microsoft.com/free/services/machine-learning/)
- [Azure Percept DK с подключенным Azure Percept Vision SoM](./overview-azure-percept-dk.md)
- [Процедура освоения Azure Percept DK](./quickstart-percept-dk-set-up.md) выполнена

## <a name="download-azure-percept-github-repository"></a>Скачивание из репозитория GitHub для Azure Percept

1. Перейдите в [репозиторий GitHub для Azure Percept DK](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview).
1. Клонируйте репозиторий или скачайте ZIP-файл. В верхней части экрана щелкните **Код** -> **Скачать ZIP-файл**.

    :::image type="content" source="./media/advanced-development-local/download-zip.png" alt-text="Экран скачивания GitHub":::

## <a name="create-an-azure-machine-learning-workspace-and-remote-compute-instance"></a>Создание вычислительного экземпляра Машинного обучения Azure и удаленного вычислительного экземпляра

1. Перейдите на [Портал Машинного обучения Azure](https://ml.azure.com).
1. Выберите каталог, подписку Azure и рабочую область машинного обучения в раскрывающихся меню, а затем щелкните **Начало работы**.

    :::image type="content" source="./media/advanced-development-local/machine-learning-portal-get-started.png" alt-text="Экран начала работы с Машинным обучением Azure":::

    Если у вас еще нет рабочей области машинного обучения Azure, щелкните **Создать рабочую область**. На новой вкладке браузера сделайте следующее.

    1. Выберите подписку Azure.
    1. Выберите нужную группу ресурсов.
    1. Укажите имя рабочей области.
    1. Выберите нужный регион.
    1. Выберите выпуск рабочей области.
    1. Щелкните **Проверка и создание**.
    1. На следующей странице просмотрите выбранные параметры и нажмите кнопку **Создать**.

        :::image type="content" source="./media/advanced-development-local/workspace-review-and-create.png" alt-text="Экран создания рабочей области в Машинном обучении Azure":::

    Подождите несколько минут для создания рабочей области. После завершения создания рабочей области вы увидите зеленые флажки рядом с ресурсами и сообщение **Развертывание выполнено** в верхней части страницы с обзором Служб машинного обучения.

    :::image type="content" source="./media/advanced-development-local/workspace-creation-complete-inline.png" alt-text="Подтверждение создания рабочей области" lightbox= "./media/advanced-development-local/workspace-creation-complete.png":::

    После завершения создания рабочей области вернитесь на вкладку портала машинного обучения и щелкните **Начало работы**.

1. На домашней странице рабочей области машинного обучения щелкните **Вычисления** на левой панели.

1. Если существующий вычислительный экземпляр отсутствует, создайте вычисление ЦП или GPU, щелкнув **Создать**. В окне **Новый вычислительный экземпляр** введите **Имя вычисления**, выберите **Тип виртуальной машины** и затем **Размер виртуальной машины**. Нажмите кнопку **Создать**.

    :::image type="content" source="./media/advanced-development-local/new-compute-instance.png" alt-text="Экран создания вычислительного экземпляра":::

    После нажатия кнопки **Создать** создание вычислительного экземпляра занимает несколько минут. Для состояния **Вычисления** отображается зеленый круг и **\<your compute name> — выполнение** после завершения создания вычисления. Этот вычислительный экземпляр запускает сервер Jupyter и будет использоваться для работы с этим руководством.

## <a name="visual-studio-code-development-environment-setup"></a>Настройка среды разработки Visual Studio Code

1. Установите необходимые средства.

    1. Вариант 1.

        Используйте [установщик пакетов средств разработки](./dev-tools-installer.md) для установки следующих пакетов.

        - Visual Studio Code
        - Python 3.5, 3.6 или 3.7
        - Miniconda3
        - Intel OpenVino Toolkit 2020.2

        Примечание. Intel OpenVino Toolkit указан в установщике пакетов средств разработки в качестве необязательного пакета, но он необходим для работы с Azure Percept Vision SoM из пакета разработки Azure Percept DK.

    1. Вариант 2.

        Если вы предпочитаете не использовать установщик пакетов средств разработки, установите следующие пакеты вручную, перейдя по указанным ниже ссылкам и следуя указаниям по скачиванию и установке.

        - [Visual Studio Code](https://code.visualstudio.com/)
        - [Python 3.5, 3.6 или 3.7](https://www.python.org/)
        - [Miniconda3](https://docs.conda.io/en/latest/miniconda.html)
        - [Intel OpenVino Toolkit 2020.2](https://docs.openvinotoolkit.org/)

    Примечание. Если вы уже установили полный дистрибутив Anaconda, устанавливать Miniconda не нужно. Кроме того, если вы предпочитаете не использовать Anaconda или Miniconda, можно создать виртуальную среду Python и установить пакеты, необходимые для разработки модели ИИ, с помощью pip.

1. Запустите Visual Studio Code.
1. Настройка среды для обработки и анализа данных

    - Вариант 1. Подключитесь к существующему или новому удаленному вычислительному экземпляру машинного обучения, где уже есть курированные пакеты машинного обучения. **Этот вариант будет использоваться в настоящем руководстве**.

    - Вариант 2. Настройте среду Conda на локальном компьютере.
        1. Откройте командную строку Anaconda или Miniconda и выполните следующую команду, чтобы создать окружение **myenv** с указанными пакетами.

            `conda create -n myenv python=3.7 pandas jupyter seaborn scikit-learn keras tensorflow=1.15`

            Дополнительные сведения о создании сред Anaconda и управлении ими см. в [документации по Anaconda](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).

1. Создание рабочей области VS Code

    1. Создайте в удобном месте папку, которая будет служить рабочей областью Visual Studio Code. Назовите ее **myworkspace**.
    1. Откройте **myworkspace** в Visual Studio Code, выбрав **Файл** > **Открыть папку** > **Выбор папки**.
    1. В проводнике найдите и выберите [файл Model.ipynbb Transferlearningusing_SSDLiteV2](https://github.com/microsoft/Project-Santa-Cruz-Private-Preview/blob/main/Sample-Scripts-and-Notebooks/Official/Machine%20Learning%20Notebooks/Transferlearningusing_SSDLiteV2%20Model.ipynb) в своей локальной копии репозитория GitHub для Azure Percept DK. Скопируйте этот файл записной книжки в папку myworkspace.

## <a name="azure-integration-with-visual-studio-code"></a>Интеграция Azure с Visual Studio Code

1. Если у вас ее еще нет, зарегистрируйтесь для получения [бесплатной или платной версии Службы машинного обучения Azure](https://aka.ms/AMLFree).

1. Установите следующие расширения Azure для VS Code.
    - [Расширение Машинного обучения Azure](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).
    - Расширение Python Insiders. В VS Code выберите **Вид** -> **Палитра команд**. В палитре команд введите и выберите **Python: Switch to Insiders Daily Channel** (Python: переключиться на ежедневный канал для участников программы предварительной оценки).
    - [Расширение учетной записи Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account). Перезагрузите окно в VS Code при появлении соответствующего запроса.
    - [Расширение "Набор средств Центра Интернета вещей Azure"](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit).
    - [Расширение Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

1. Войдите в свою учетную запись Azure в Visual Studio Code, чтобы подготавливать ресурсы и выполнять рабочие нагрузки в Azure.
    1. В Visual Studio Code откройте палитру команд, выбрав **Вид** > **Палитра команд** в строке меню.
    1. Введите **Azure: Sign In** (Azure: вход) в палитре команд, чтобы начать процесс входа.

        :::image type="content" source="./media/advanced-development-local/transfer-learning-azure-sign-in-inline.png" alt-text="Экран входа в Azure" lightbox= "./media/advanced-development-local/transfer-learning-azure-sign-in.png":::

    1. Активируйте расширение Python и учетную запись Azure, щелкнув значок Azure в левой части VS Code.

        :::image type="content" source="./media/advanced-development-local/azure-icon.png" alt-text="Значок Azure в VS Code":::

    1. Откройте записную книжку Model_VSCode.ipynb Transferlearningusing_SSDLiteV2 из папки **myworkspace**.
    1. Откройте палитру команд. Введите и выберите **Python: specify local or remote Jupyter server for connections** (Python: укажите локальный или удаленный сервер Jupyter для подключений).
    1. Вы увидите список вариантов подключения. Выберите **Вычислительные экземпляры Машинного обучения Azure**.

        :::image type="content" source="./media/advanced-development-local/azure-machine-learning-compute-instances.png" alt-text="Параметры вычислительного экземпляра Машинного обучения Azure в VS Code":::

    1. Следуйте пошаговым инструкциям, чтобы выбрать подписку, рабочую область и удаленный вычислительный экземпляр. Выберите рабочую область и удаленный вычислительный экземпляр, созданный ранее в этом руководстве. Вы также можете создать новый вычислительный экземпляр.
    1. После выбора вычислительного экземпляра вам будет предложено перезагрузить окно VS Code. После перезагрузки выберите ядро **Python 3.6 — AzureML**. Теперь вы можете запустить любую ячейку в записной книжке. При выполнении ячейки записной книжки будет создан экземпляр подключения между записной книжкой и вычислительным экземпляром. Панель инструментов записной книжки будет обновлена, чтобы отразить используемый вычислительный экземпляр.

        :::image type="content" source="./media/advanced-development-local/kernel-inline.png" alt-text="Выбор ядра в VS Code" lightbox= "./media/advanced-development-local/kernel.png":::

## <a name="working-with-the-notebook"></a>Работа с записной книжкой

Теперь вы можете запустить записную книжку, чтобы обучить пользовательское средство обнаружения тарелок в VS Code и развернуть его в наборе разработки. Обязательно выполните каждую ячейку записной книжки отдельно, так как некоторые ячейки перед выполнением сценария должны иметь входные параметры. Ячейки, требующие входных параметров, можно изменить непосредственно в записной книжке. Чтобы запустить ячейку, щелкните значок воспроизведения в левой ее части:

:::image type="content" source="./media/advanced-development-local/run-cell-1.png" alt-text="Записная книжка с выделенным значком ячейки":::

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные примеры записных книжек для Службы машинного обучения Azure см. в этом [репозитории](https://github.com/Azure/MachineLearningNotebooks/tree/2aa7c53b0ce84e67565d77e484987714fdaed36e/how-to-use-azureml).

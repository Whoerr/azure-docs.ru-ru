---
ms.openlocfilehash: 3c0d6dbba6ecda7ef87e4978d522da27cb6f9833
ms.sourcegitcommit: 31cfd3782a448068c0ff1105abe06035ee7b672a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/10/2021
ms.locfileid: "98061229"
---
При настройке ресурсов Azure на виртуальную машину Linux в Azure, используемую в качестве устройства IoT Edge, копируется короткое видео дорожного потока автомобилей. В этом кратком руководстве для имитации потока в реальном времени используется видеофайл.

Откройте приложение, например [проигрыватель мультимедиа VLC](https://www.videolan.org/vlc/). Нажмите сочетание клавиш CTRL+N, а затем вставьте ссылку на [видео](https://lvamedia.blob.core.windows.net/public/camera-300s.mkv), чтобы начать воспроизведение. Вы увидите видеозапись множества транспортных средств, движущихся в потоке.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4LTY4]

В этом кратком руководстве вы будете использовать Аналитику видеотрансляций в IoT Edge для обнаружения таких объектов, как транспортные средства и лица. Вы будете публиковать связанные события вывода в центре IoT Edge.

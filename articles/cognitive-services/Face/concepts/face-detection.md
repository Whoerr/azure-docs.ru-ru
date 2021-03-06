---
title: Основные понятия об обнаружении и атрибутах лиц
titleSuffix: Azure Cognitive Services
description: Обнаружение лиц — это действие, которое позволяет найти людей в изображении и при необходимости возвратить различные виды данных, связанных с лицом.
services: cognitive-services
author: PatrickFarley
manager: nitime
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: pafarley
ms.openlocfilehash: 68d1e9744d937cf80327c3f41cc69f4af97d3400
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/20/2021
ms.locfileid: "98600186"
---
# <a name="face-detection-and-attributes"></a>Обнаружение и атрибуты лиц

В этой статье объясняются понятия, связанные с определением лиц и данными атрибутов лиц. Обнаружение лиц — это действие, которое позволяет найти людей в изображении и при необходимости возвратить различные виды данных, связанных с лицом.

Для обнаружения лиц в образе используется операция [распознавания лиц](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) . Как минимум, каждая обнаруженная грань соответствует полю Фацеректангле в ответе. Этот набор координат в пикселях для левого, верхнего, ширины и высоты отмечает найденный значок. Используя эти координаты, можно получить расположение лица и его размер. В ответе API лица отображаются в порядке их размера от большего к меньшему.

## <a name="face-id"></a>Идентификатор лица

ИДЕНТИФИКАТОР лица — это уникальная строка идентификатора для каждой обнаруженной грани в изображении. ИДЕНТИФИКАТОР лица можно запросить в вызове API распознавания [лиц](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) .

## <a name="face-landmarks"></a>Ориентиры лица

Ориентиры — это набор удобных для поиска точек на лицевой стороне, например пупилс или Совет нос. По умолчанию используются 27 предопределенных точек ориентиров. На следующем рисунке показаны все 27 точек:

![Схема лиц со всеми 27 ориентирами с меткой](../Images/landmarks.1.jpg)

Координаты точек возвращаются в виде единиц точек.

## <a name="attributes"></a>Атрибуты

Атрибуты — это набор функций, которые при необходимости могут быть обнаружены API распознавания [лиц](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) . Могут быть обнаружены следующие атрибуты:

* **Возраст**. Предполагаемый возраст определенного лица в годах.
* **Размытие**. Размытость лица в изображении. Этот атрибут возвращает значение от 0 до одной и неформальной оценки низкого, среднего или высокого уровня.
* **Распознавания эмоций**. Список эмоции с достоверностью обнаружения для данного лица. Оценки достоверности нормализованы, а оценки для всех эмоции добавляются к одному. Эмоции возвращает счастье, грусть, Neutral, гнев, зрение, отвращение, удивление и счастье.
* **Экспозиция**. Раскрытие поверхности изображения. Этот атрибут возвращает значение от нуля до одной и неформальной оценки недоэкспозиции, Гудекспосуре или перекрытия.
* **Волосы**. Оценка присутствия лица и длина данного лица.
* **Пол**. Предполагаемый род данного лица. Возможные значения: «пол», «женщина» и «беспол».
* **Очков**. Имеет ли заданная грань очков. Возможные значения: Реадингглассес, своему солнцезащитных очков и Swimming Гогглес.
* **Волосы**. Тип волосы для грани. Этот атрибут показывает, видны ли волосы, обнаружено ли балднесс и какие именно волосы будут обнаружены.
* **Главная**. Ориентация лица в трехмерном пространстве. Этот атрибут описывается наклоном, рулоном и значения нутации углом в градусах. Диапазон значений — от-90 градусов до 90 градусов, от-90 градусов до 90 градусов и от-90 градусов до 90 градусов соответственно. Для сопоставления углов см. следующую диаграмму:

    ![Заголовок с осями тон, рулон и значения нутации с меткой](../Images/headpose.1.jpg)
* **Описывающего**. Имеет ли лицо описывающего. Этот атрибут возвращает логическое значение для Эйемакеуп и Липмакеуп.
* **Шум**. Визуальный шум обнаружен в изображении лицевой стороны. Этот атрибут возвращает значение от 0 до одной и неформальной оценки низкого, среднего или высокого уровня.
* **Перекрытия**. Существуют ли объекты, блокирующие части грани. Этот атрибут возвращает логическое значение для Эйеокклудед, Форехеадокклудед и Маусокклудед.
* **Смайлик**. Выражение смайлика данной грани. Это значение находится в диапазоне от нуля в отсутствие смайлика, а другое — для четкой смайлика.

> [!IMPORTANT]
> Атрибуты лица прогнозируется с помощью статистических алгоритмов. Они могут не всегда быть точными. Будьте внимательны при принятии решений на основе данных атрибутов.

## <a name="input-data"></a>Входные данные

Используйте следующие советы, чтобы убедиться, что входные образы дают наиболее точные результаты обнаружения:

* Поддерживаемые форматы входных изображений: JPEG, PNG, GIF для первого кадра и BMP.
* Размер файла изображения должен быть не более 6 МБ.
* Минимальный размер лица, который может быть обнаружен, составляет 36 x 36 пикселей в изображении размером не более 1920 x 1080 пикселей. Изображения с размером более 1920 x 1080 пикселей имеют пропорционально больший размер в меньшей мере. Уменьшение размера шрифта может привести к тому, что некоторые лица не будут обнаружены, даже если они больше минимального размера, который можно определить.
* Максимальный размер, который может быть обнаружен, составляет 4096 x 4096 пикселей.
* Лица, не входящие в диапазон размера 36 x 36 до 4096 x 4096 пикселей, не будут обнаружены.
* Некоторые лица могут не быть обнаружены из-за технических проблем. Крайние углы (головной элемент) или лицо перекрытия (объекты, такие как своему солнцезащитных очков или руки, которые блокируют часть грани) могут повлиять на обнаружение. Лучшие результаты выдаются внешними и близкими к лицевым сторонам.

Если вы обнаруживаете лица из видеоканала, вы можете повысить производительность, изменив определенные параметры видеокамеры.

* **Сглаживание**: многие видеокамеры применяют гладкий режим. Этот параметр следует отключить, если это можно сделать, поскольку он создает размытие между кадрами и сокращает ясность.
* **Скорость затвора**: более высокая скорость затвора сокращает количество перемещений между кадрами и делает каждый кадр более четким. Мы рекомендуем использовать выдержку с частотой 1/60 секунды или выше.
* **Угол затвора**: некоторые камеры указывают угол затвора вместо скорости затвора. По возможности следует использовать нижний угол затвора. Это приведет к снятию кадров видео.

    >[!NOTE]
    > Камера с нижним углом затвора получит меньше освещения в каждом кадре, поэтому изображение будет темнее. Необходимо определить подходящий уровень для использования.

## <a name="next-steps"></a>Следующие шаги

Теперь, когда вы знакомы с концепциями обнаружения лиц, Узнайте, как написать сценарий, который обнаруживает лица в определенном изображении.

* [Определение лиц на изображении](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)

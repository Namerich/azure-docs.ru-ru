---
title: Изменения в Пользовательском визуальном распознавании
titleSuffix: Azure Cognitive Services
description: Эта статья содержит новости о Пользовательском визуальном распознавании.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: overview
ms.date: 11/23/2020
ms.author: pafarley
ms.openlocfilehash: 57f3cf0cb15243d054da0111366f3a1dc0fb5349
ms.sourcegitcommit: 1bf144dc5d7c496c4abeb95fc2f473cfa0bbed43
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/24/2020
ms.locfileid: "95739763"
---
# <a name="whats-new-in-custom-vision"></a>Изменения в Пользовательском визуальном распознавании

Узнайте о новых возможностях службы. Эти элементы могут включать заметки о выпуске, видео, записи блогов и другие типы информации. Создайте закладку для этой страницы, чтобы получать последние сведения об обновлениях службы.


## <a name="october-2020"></a>Октябрь 2020 г. 

### <a name="custom-base-model"></a>Пользовательская базовая модель

- Некоторые приложения используют большой объем общих обучающих данных, но модели в них настраиваются раздельно. Это повышает производительность при работе с изображениями из разных источников, различия между которыми незначительны. В этом случае первую модель можно обучать обычным образом по большому объему обучающих данных. После этого вызовите **TrainProject** из API общедоступной предварительной версии 3.4, указав в тексте запроса объект _CustomBaseModelInfo_, чтобы применить первую обученную модель в качестве базовой для последующих проектов. Если исходный проект и основанный на нем целевой проект используют изображения со сходными характеристиками, можно рассчитывать на повышенную производительность. 

### <a name="new-domain-information"></a>Новые сведения о домене

- Сведения о домене, получаемые через **GetDomains** в API Пользовательского визуального распознавания общедоступной предварительной версии 3.4, теперь включают поддерживаемые экспортируемые платформы, краткое описание архитектуры модели и размер модели для компактных доменов.

### <a name="training-divergence-feedback"></a>Отзывы о расхождениях при обучении

- API Пользовательского визуального распознавания общедоступной предварительной версии 3.4 теперь возвращает **TrainingErrorDetails** при вызове **GetIteration**. Для неудачных итераций это позволяет определить, была ли ошибка вызвана расхождением при обучении, которое можно устранить путем повышения объема и качества обучающих данных.

## <a name="july-2020"></a>Июль 2020 г.

### <a name="azure-role-based-access-control"></a>Управление доступом на основе ролей в Azure

* Пользовательское визуальное распознавание поддерживает управление доступом на основе ролей Azure (Azure RBAC), систему авторизации для управления отдельным доступом к ресурсам Azure. Сведения об управлении доступом к проектам Пользовательского визуального распознавания см. в статье об [управлении доступом на основе ролей в Azure](./role-based-access-control.md).

### <a name="subset-training"></a>Обучение подмножества

* При обучении проекта обнаружения объектов по желанию вы можете выполнить обучение с использованием только некоторых из примененных тегов. Это может потребоваться, если вы еще не применили достаточное количество определенных тегов, но у вас достаточно других. Дополнительные сведения см. в кратком руководстве по [работе с клиентской библиотекой для C# или Python](./quickstarts/object-detection.md).

### <a name="azure-storage-notifications"></a>Уведомления службы хранилища Azure

* Проект "Пользовательское визуальное распознавание" можно интегрировать с очередью Хранилища BLOB-объектов Azure, чтобы получать push-уведомления об обучении или экспорте проекта и о резервных копиях опубликованных моделей. Эта функция позволяет избежать постоянного опроса службы на предмет результатов, когда выполняются длительные операции. Вместо этого уведомления очереди хранилища можно интегрировать в рабочий процесс. Дополнительные сведения см. в статье [об интеграции с хранилищем](./storage-integration.md).

### <a name="copy-and-move-projects"></a>Копирование и перемещение проектов

* Теперь можно копировать проекты из одной учетной записи Пользовательского визуального распознавания в другие. Чтобы повысить безопасность данных, возможно, потребуется переместить проект из среды разработки в рабочую среду или создать резервную копию проекта в учетной записи в другом регионе Azure. Дополнительные сведения см. в статье [о копировании и перемещении проектов](./copy-move-projects.md).

## <a name="september-2019"></a>Сентябрь 2019 г.

### <a name="suggested-tags"></a>Предложенные теги

* Средство Smart Labeler на веб-сайте [Пользовательское визуальное распознавание](https://www.customvision.ai/) создает предложения по тегам для обучающих изображений. Это позволит вам быстрее присвоить теги большому числу изображений, по которым будет обучаться модель Пользовательского визуального распознавания. Инструкции по использованию этой возможности см. в статье [о предложенных тегах](./suggested-tags.md).

## <a name="cognitive-service-updates"></a>Обновления Cognitive Service

[Объявление об обновлениях Azure для Cognitive Services](https://azure.microsoft.com/updates/?product=cognitive-services)
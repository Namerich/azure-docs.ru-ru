---
title: 'ML Studio (классическая модель): создание конечных точек веб-службы в Azure'
description: Создайте конечные точки веб-службы в Машинное обучение Azure Studio (классическая модель). Каждая конечная точка в веб-службе адресуется, регулируется и управляется независимо.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: how-to
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 02/15/2019
ms.openlocfilehash: 1032a90a35e60643e2ce937ed457a1fe3493d4d7
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/04/2020
ms.locfileid: "93322883"
---
# <a name="create-endpoints-for-deployed-azure-machine-learning-studio-classic-web-services"></a>Создание конечных точек для развернутых веб-служб Машинное обучение Azure Studio (классическая модель)

**ПРИМЕНИМО К:**  ![Применимо к.](../../../includes/media/aml-applies-to-skus/yes.png)Студия машинного обучения (классическая)   ![Неприменимо к. ](../../../includes/media/aml-applies-to-skus/no.png)[Машинное обучение Azure](../overview-what-is-machine-learning-studio.md#ml-studio-classic-vs-azure-machine-learning-studio)


> [!NOTE]
> В этом разделе описываются методы, применимые к **классической** веб-службе машинное обучение.

После развертывания веб-службы для нее создается конечная точка по умолчанию. Эту конечную точку можно вызвать, используя ключ API. На портале веб-служб вы можете добавить больше конечных точек с их собственными ключами.
Каждая конечная точка в веб-службе адресуется, регулируется и управляется независимо. Каждая конечная точка представляет собой уникальный URL-адрес с ключом авторизации, который вы можете передавать своим клиентам.

## <a name="add-endpoints-to-a-web-service"></a>Добавление конечных точек в веб-службу

Вы можете добавить конечную точку в управление веб-службой с помощью портала веб-служб Машинного обучения Azure. После создания конечную точку можно использовать в синхронных API, API пакетов и на листах Excel.

> [!NOTE]
> Если в веб-службу добавлены дополнительные конечные точки, вы не можете удалить конечную точку.

1. В Машинное обучение Studio (классическая модель) в колонке навигации слева щелкните веб-службы.
2. В нижней части панели мониторинга веб-службы щелкните **Управление конечными точками**. Портал веб-служб Машинного обучения Azure откроется на странице конечных точек для веб-службы.
3. Нажмите кнопку **Создать**.
4. Введите имя и описание новой конечной точки. Имена конечных точек должны и состоять из строчных букв или цифр и не могут содержать более 24 символов. Выберите уровень ведения журнала и укажите, включать ли образцы данных. Дополнительные сведения о ведении журнала см. в статье [Включение ведения журнала для веб-служб машинное обучение](web-services-logging.md).

## <a name="scale-a-web-service-by-adding-additional-endpoints"></a><a id="scaling"></a> Масштабирование веб-службы путем добавления дополнительных конечных точек

По умолчанию каждая опубликованная веб-служба настраивается для поддержки 20 параллельных запросов, а максимальное число параллельных запросов может доходить до 200. Машинное обучение Azure Studio (классическая модель) автоматически оптимизирует этот параметр, чтобы обеспечить максимальную производительность веб-службы, и значение на портале игнорируется.

Если для API планируется более высокая нагрузка, чем максимальное поддерживаемое значение в 200 одновременных вызовов, следует создать для такой веб-службы несколько конечных точек. Это позволит случайным образом распределять нагрузку между ними.

Масштабирование веб-службы — достаточно распространенная задача. Например, масштабирование будет полезным для поддержки более 200 одновременных запросов, для повышения доступности путем использования нескольких конечных точек или для разделения конечных точек веб-службы. Вы можете увеличить масштаб, добавив дополнительные конечные точки для той же веб-службы с помощью портала [веб-службы машинное обучение Azure](https://services.azureml.net/) .

Помните, что высокие значения для количества одновременных событий могут неблагоприятно влиять на работу API, если для него не создается достаточно высокой нагрузки. При создании достаточно низкой нагрузки на API, настроенный на высокую нагрузку, периодически случайно могут возникать периоды ожидания или резкое повышение значений задержки.

Синхронные API обычно используются в ситуациях, когда требуется низкий уровень задержки. Под задержкой здесь подразумевается время, необходимое для того, чтобы API завершил один запрос, и в ней не учитывается время сетевой задержки. Предположим, что имеется API с задержкой в 50 мс. Чтобы полностью задействовать доступный объем с использованием высокого уровня ускорения и максимальным количеством одновременных вызовов = 20, необходимо вызывать этот API 20 * 1000 / 50 = 400 раз в секунду. Аналогично, при максимальном количестве одновременных вызовов в 200 и задержке в 50 мс этот API можно вызывать 4000 раз в секунду.

## <a name="next-steps"></a>Дальнейшие действия

[Как использовать веб-службу машинное обучение Azure](consume-web-services.md).
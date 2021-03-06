---
title: Правила именования сущностей фабрики данных Azure
description: Описывает правила именования для сущностей фабрик данных.
services: data-factory
documentationcenter: ''
author: dcstwh
ms.author: weetok
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/15/2020
ms.openlocfilehash: 21adf26c2dbaca4507a4c925e3dae3b99c9d53ba
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/02/2020
ms.locfileid: "96497527"
---
# <a name="azure-data-factory---naming-rules"></a>Фабрика данных Azure — правила именования

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

В следующей таблице приведены правила именования для артефактов фабрики данных.

| Название | Уникальность имени | Проверки |
|:--- |:--- |:--- |
| Фабрика данных | Уникально в рамках Microsoft Azure. Регистр в именах не учитывается, т. е. `MyDF` и `mydf` ссылаются на одну и ту же фабрику данных. |<ul><li>Каждая фабрика данных привязывается ровно к одной подписке Azure.</li><li>Имена объектов должны начинаться с буквы или цифры и могут содержать только буквы, цифры и дефисы (-).</li><li>Перед каждым знаком тире (-) и после него должна стоять буква или цифра. Последовательные тире в именах контейнеров использовать нельзя.</li><li>Имя может содержать от 3 до 63 символов.</li></ul> |
| Связанные службы/наборы данных/конвейеры/потоки данных | Уникально в рамках фабрики данных. Регистр в именах не учитывается. |<ul><li>Имена объектов должны начинаться с буквы.</li><li>Следующие символы не допускаются: ".", "+", "?", "/", "<", ">", "*", "%", "&", ":", " \\ "</li><li>Дефисы ("-") не допускаются в именах связанных служб, потоков данных и DataSets.</li></ul>  |
| Integration Runtime |Уникально в рамках фабрики данных. Регистр в именах не учитывается. |<ul><li>Имя среды выполнения интеграции может содержать только буквы, цифры и дефис (-).</li><li>Первый и последний символы должны быть буквами или цифрами. Перед каждым знаком тире (-) и после него должна стоять буква или цифра.</li><li>В имени среды выполнения интеграции не допускаются последовательные дефисы. </li></ul> |
| Преобразования потока данных | Уникально в потоке данных. Имена не учитывают регистр | <ul><li>Имена преобразований потока данных могут содержать только буквы и цифры</li><li>Первый символ должен быть буквой. </li></ul> |
| Группа ресурсов |Уникально в рамках Microsoft Azure. Регистр в именах не учитывается. | Дополнительные сведения см. в разделе [Правила именования и ограничения](/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging#resource-naming). |

## <a name="next-steps"></a>Дальнейшие действия
Узнайте, как создавать фабрики данных, выполнив пошаговые инструкции из [краткого руководства](quickstart-create-data-factory-powershell.md). 

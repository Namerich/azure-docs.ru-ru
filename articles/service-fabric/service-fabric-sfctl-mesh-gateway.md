---
title: Azure Service Fabric CLI — шлюз сетки sfctl
description: Сведения о sfctl, интерфейсе командной строки Azure Service Fabric. Содержит список команд для получения и удаления Service Fabric ресурсов шлюза сетки.
author: jeffj6123
ms.topic: reference
ms.date: 1/16/2020
ms.author: jejarry
ms.openlocfilehash: 9b6766137dd88a5a780dcca7b6eab7c6c3f9bbf4
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "86260392"
---
# <a name="sfctl-mesh-gateway"></a>sfctl mesh gateway
Получение и удаление ресурсов mesh gateway.

## <a name="commands"></a>Команды

|Команда|Описание|
| --- | --- |
| "Удалить" | Удаляет ресурс шлюза. |
| list | Отображает список всех ресурсов шлюза. |
| показать | Возвращает ресурс шлюза вместе с заданным именем. |

## <a name="sfctl-mesh-gateway-delete"></a>sfctl mesh gateway delete
Удаляет ресурс шлюза.

Удаляет ресурс шлюза, идентифицированный по имени.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --name -n [обязательный параметр] | Имя ресурса шлюза. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Повышение уровня детализации журнала для включения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат вывода.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации журнала. Чтобы включить полные журналы отладки, используйте параметр --debug. |

## <a name="sfctl-mesh-gateway-list"></a>sfctl mesh gateway list
Отображает список всех ресурсов шлюза.

Возвращает сведения обо всех ресурсах шлюза в указанной группе ресурсов. Эти сведения содержат описание и другие свойства шлюза.

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Повышение уровня детализации журнала для включения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат вывода.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации журнала. Чтобы включить полные журналы отладки, используйте параметр --debug. |

## <a name="sfctl-mesh-gateway-show"></a>sfctl mesh gateway show
Возвращает ресурс шлюза вместе с заданным именем.

Возвращает сведения о ресурсе шлюза с заданным именем. Эти сведения содержат описание и другие свойства шлюза.

### <a name="arguments"></a>Аргументы

|Аргумент|Описание|
| --- | --- |
| --name -n [обязательный параметр] | Имя ресурса шлюза. |

### <a name="global-arguments"></a>Глобальные аргументы

|Аргумент|Описание|
| --- | --- |
| --debug | Повышение уровня детализации журнала для включения всех журналов отладки. |
| --help -h | Отображение этого справочного сообщения и выход. |
| --output -o | Формат вывода.  Допустимые значения\: json, jsonc, table, tsv.  Значение по умолчанию\: json. |
| --query | Строка запроса JMESPath. Дополнительные сведения и примеры см. на веб-сайте http\://jmespath.org/. |
| --verbose | Повышение уровня детализации журнала. Чтобы включить полные журналы отладки, используйте параметр --debug. |


## <a name="next-steps"></a>Дальнейшие действия
- [Настройте](service-fabric-cli.md) Service Fabric CLI.
- Узнайте, как использовать интерфейс командной строки Service Fabric, с помощью [примеров сценариев](./scripts/sfctl-upgrade-application.md).

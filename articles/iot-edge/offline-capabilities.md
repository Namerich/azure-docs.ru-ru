---
title: Автономная работа устройств — Azure IoT Edge | Документация Майкрософт
description: Узнайте, как устройства и модули IoT Edge могут работать в течение долгого периода времени без подключения к Интернету и как с помощью IoT Edge можно обеспечить автономную работу обычных устройств Интернета вещей.
author: kgremban
ms.author: kgremban
ms.date: 11/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: bf8b8554aa2ea1d6d06f58f726ca65f77499ec5f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2020
ms.locfileid: "91440039"
---
# <a name="understand-extended-offline-capabilities-for-iot-edge-devices-modules-and-child-devices"></a>Сведения о расширенных возможностях автономной работы устройств, модулей и дочерних устройств IoT Edge

Azure IoT Edge поддерживает расширенную автономную работу устройств IoT Edge и обеспечивает автономную работу для дочерних устройств не из IoT Edge. Пока у устройства IoT Edge есть возможность подключиться к Центру Интернета вещей, оно и его дочерние устройства могут продолжать работу с нестабильным подключением к Интернету или даже без него.

## <a name="how-it-works"></a>Принцип работы

Когда устройство IoT Edge переходит в автономный режим, центр IoT Edge выполняет три роли. Во-первых, он хранит все сообщения, которые необходимо отправить, до тех пор, пока соединение с устройством не будет восстановлено. Во-вторых, он выступает от имени центра Интернета вещей и проверяет подлинность модулей и дочерних устройств, чтобы они продолжали работу. В-третьих, он обеспечивает связь между дочерними устройствами, которая обычно проходит через центр Интернета вещей.

В следующем примере показано, как IoT Edge работает в автономном режиме:

1. **Настройка устройств**

   Функция автономной работы включена на устройствах IoT Edge автоматически. Чтобы дать эту возможность другим устройствам Интернета вещей, необходимо объявить отношение "родитель-дочерний элемент" между устройствами в центре Интернета вещей. Затем следует настроить дочерние устройства, чтобы они доверяли назначенному им родительскому устройству, и маршрутизировать данные, которыми обменивается устройство с облаком, используя родительское устройство в качестве шлюза.

2. **Синхронизация с центром Интернета вещей**

   Хотя бы один раз после установки среды выполнения IoT Edge необходимо подключить устройство IoT Edge к сети для синхронизации с центром Интернета вещей. Во время синхронизации устройство IoT Edge получает сведения о назначенных ему дочерних устройствах. Устройство IoT Edge безопасно обновляет свой локальный кэш, чтобы можно было продолжать работу автономно, и извлекает параметры для локального хранения сообщений телеметрии.

3. **Переход в автономный режим**

   При отключении от центра Интернета вещей устройств IoT Edge его развернутые модули и любые дочерние устройства Интернета вещей могут работать бесконечно. Модули и дочерние устройства могут запускаться и перезапускаться с прохождением аутентификации в центре IoT Edge в автономном режиме. Восходящий поток телеметрии в центр Интернета вещей хранится локально. Обмен данными между модулями или дочерними устройствами Интернета вещей поддерживается через прямые методы или сообщения.

4. **Повторное подключение и синхронизация с Центром Интернета вещей**

   После восстановления связи с центром Интернета вещей устройство IoT Edge будет снова синхронизировано. Сообщения, которые хранятся локально, немедленно доставляются в Центр Интернета вещей, однако они зависят от скорости подключения, задержки Центра Интернета вещей и иных сопутствующих факторов. Локально хранимые сообщения доставляются в том же порядке, в котором они были сохранены.

   Все различия между требуемыми и сообщаемыми свойствами модулей и устройств будут согласованы. Устройство IoT Edge передает все изменения в свои назначенные дочерние устройства.

## <a name="restrictions-and-limits"></a>Ограничения

Расширенные возможности автономной работы, описанные в этой статье, доступны в [IoT Edge 1.0.7 или более поздней версии](https://github.com/Azure/azure-iotedge/releases). Более ранние версии имеют подмножество автономных функций. Существующие устройства IoT Edge без расширенных возможностей автономной работы невозможно обновить, изменив версию среды выполнения. Необходимо изменить их конфигурацию с использованием нового удостоверения устройства IoT Edge, чтобы использовать эти функции.

В качестве дочерних устройств можно добавить только устройства не из IoT Edge.

Устройства IoT Edge и их дочерние устройства могут неограниченное время работать в автономном режиме после первой однократной синхронизации. Однако хранение сообщений зависит от срока жизни (TTL) и доступного места на диске для хранения сообщений.

## <a name="set-up-parent-and-child-devices"></a>Настройка родительских и дочерних устройств

Чтобы использовать расширенные возможности автономной работы, характерные для устройств IoT Edge, на дочерних устройствах Интернета вещей, необходимо выполнить два действия. Во-первых, сначала объявить связи типа "родители — потомки" на портале Azure. Во-вторых, настроить отношение доверия между родительским устройством и всеми дочерними, а затем настроить обмен данными между устройством и облаком, используя родительское устройство в качестве шлюза.

### <a name="assign-child-devices"></a>Назначение дочерних устройств

Дочерние устройства могут быть любыми устройствами не из IoT Edge, зарегистрированными в одном Центре Интернета вещей. Родительские устройства могут иметь несколько дочерних устройств, но для дочернего устройства может существовать только одно родительское. Существует три варианта установки дочерних устройств на пограничном устройстве: через портал Azure, с помощью Azure CLI или с помощью пакета SDK для службы Центра Интернета вещей.

В следующих разделах приведены примеры объявления отношений "родители — потомки" в Центре Интернета вещей для существующих устройств IoT. Дополнительные сведения о создании новых удостоверений для дочерних устройств см. в статье [Проверка подлинности подчиненного устройства в Центре Интернета вещей Azure](how-to-authenticate-downstream-device.md).

#### <a name="option-1-iot-hub-portal"></a>Вариант 1. Портал Центра Интернета вещей

Вы можете объявить отношение "родители — потомки" при создании нового устройства. Кроме того, для существующих устройств можно объявить отношение на странице сведений о родительском или дочернем устройстве Интернета вещей.

   ![Управление дочерними устройствами на странице сведений об устройстве IoT Edge](./media/offline-capabilities/manage-child-devices.png)

#### <a name="option-2-use-the-az-command-line-tool"></a>Вариант 2. Использование инструмента командной строки `az`

Используя [интерфейс командной строки Azure](/cli/azure/) с [расширением IoT](https://github.com/azure/azure-iot-cli-extension) (версия 0.7.0 или более поздняя), можно управлять отношениями "родитель — потомок" с помощью подкоманд [device-identity](/cli/azure/ext/azure-iot/iot/hub/device-identity). В следующем примере запрос используется для назначения всех устройств не из IoT Edge в центре как дочерних устройств для устройства IoT Edge.

```azurecli
# Set IoT Edge parent device
egde_device="edge-device1"

# Get All IoT Devices
device_list=$(az iot hub query \
        --hub-name replace-with-hub-name \
        --subscription replace-with-sub-name \
        --resource-group replace-with-rg-name \
        -q "SELECT * FROM devices WHERE capabilities.iotEdge = false" \
        --query 'join(`, `, [].deviceId)' -o tsv)

# Add all IoT devices to IoT Edge (as child)
az iot hub device-identity add-children \
  --device-id $egde_device \
  --child-list $device_list \
  --hub-name replace-with-hub-name \
  --resource-group replace-with-rg-name \
  --subscription replace-with-sub-name
```

Можно изменить [запрос](../iot-hub/iot-hub-devguide-query-language.md), чтобы выбрать другое подмножество устройств. Выполнение команды может занять несколько секунд, если задан большой набор устройств.

#### <a name="option-3-use-iot-hub-service-sdk"></a>Способ 3. Использование пакета SDK для службы Центра Интернета вещей

Наконец, можно управлять отношениями между "родителями" и "потомками" программно с помощью пакета SDK для C#, Java или службы Центра Интернета вещей Node.js. Ниже приведен [пример назначения дочернего устройства](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/e2e/test/iothub/service/RegistryManagerE2ETests.cs) с помощью пакета SDK C#.

### <a name="set-up-the-parent-device-as-a-gateway"></a>Настройка родительского устройства в качестве шлюза

Отношение "родители — потомки" можно рассматривать как прозрачный шлюз, где дочернее устройство использует собственное удостоверение в центре Интернета вещей, но обмен данными осуществляет в облаке через свое родительское устройство. Для безопасного взаимодействия дочернее устройство должно иметь возможность убедиться в том, что родительское устройство поступает из надежного источника. В противном случае сторонние устройства могут настроить вредоносные устройства, которые имитируют родительские устройства и перехватывают данные.

Один из способов создания такого отношения доверия подробно описан в следующих статьях.

* [Настройка устройства IoT Edge для использования в качестве прозрачного шлюза](how-to-create-transparent-gateway.md)
* [Connect a downstream (child) device to an Azure IoT Edge gateway](how-to-connect-downstream-device.md) (Подключение подчиненного (дочернего) устройства к шлюзу Azure IoT Edge)

## <a name="specify-dns-servers"></a>Указание DNS-серверов

Чтобы повысить надежность, настоятельно рекомендуется указать адреса DNS-серверов, используемые в вашей среде. Инструкции по настройке DNS-сервера для IoT Edge см. в описании решения проблемы [Модуль агента Edge постоянно выводит сообщение "Пустой файл конфигурации", и на устройстве не запускаются модули](troubleshoot-common-errors.md#edge-agent-module-reports-empty-config-file-and-no-modules-start-on-the-device) в статье раздела, посвященного устранению неполадок.

## <a name="optional-offline-settings"></a>Дополнительные параметры автономной работы

Если устройства переведены в автономный режим, родительское устройство IoT Edge хранит все сообщения, отправляемые с устройства в облако, вплоть до восстановления подключения. Модуль центра IoT Edge управляет хранением и пересылкой сообщений в автономном режиме. Для устройств, которые могут находиться в автономном режиме в течение продолжительного периода времени, необходимо оптимизировать производительность, настроив два параметра центра IoT Edge.

Во-первых, увеличьте время жизни данных, чтобы центр IoT Edge сохранял сообщения в течение достаточно долгого периода вплоть до восстановления подключения устройства. Затем добавьте дополнительное место на диске для хранения сообщений.

### <a name="time-to-live"></a>Срок жизни

Срок жизни — это период времени (в секундах), в течение которого сообщение ожидает доставки, прежде чем срок его действия истечет. По умолчанию это 7200 секунд (два часа). Максимальное значение ограничено только максимальным значением целочисленной переменной (приблизительно 2 000 000 000).

Этот параметр является требуемым свойством центра IoT Edge, которое хранится в двойнике модуля. Можно указать эти значения на портале Azure или непосредственно в манифесте развертывания.

```json
"$edgeHub": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {},
        "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
        }
    }
}
```

### <a name="host-storage-for-system-modules"></a>Хранилище узлов для системных модулей

Сообщения и сведения о состоянии модуля по умолчанию хранятся в локальной файловой системе контейнера IoT Edge. Для повышения надежности, особенно при работе в автономном режиме, можно также выделить хранилище на узле устройства IoT Edge. Дополнительные сведения см. в разделе [Предоставление модулям доступа к локальному хранилищу устройства](how-to-access-host-storage-from-module.md).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о настройке прозрачного шлюза для подключения родительских и дочерних устройств

* [Настройка устройства IoT Edge для использования в качестве прозрачного шлюза](how-to-create-transparent-gateway.md)
* [Аутентификация подчиненного устройства в Центре Интернета вещей](how-to-authenticate-downstream-device.md)
* [Connect a downstream device to an Azure IoT Edge gateway](how-to-connect-downstream-device.md) (Подключение подчиненного устройства к шлюзу Azure IoT Edge)

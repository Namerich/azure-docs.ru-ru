---
title: Отправка файлов с устройств в службу хранилища Azure | Документация Майкрософт
description: Настройка отправки файлов с устройств в облако. После настройки отправки файлов реализуйте отправку файлов на устройствах.
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 08/06/2020
ms.topic: how-to
ms.service: iot-central
ms.openlocfilehash: d6fbf84ec3822195f62970dbf08115059ffb7e4a
ms.sourcegitcommit: 7dacbf3b9ae0652931762bd5c8192a1a3989e701
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2020
ms.locfileid: "92122608"
---
# <a name="upload-files-from-your-devices-to-the-cloud"></a>Передача файлов с устройств в облако

*Этот раздел относится к администраторам и разработчикам устройств.*

IoT Central позволяет отправлять мультимедиа и другие файлы с подключенных устройств в облачное хранилище. Вы настраиваете возможность отправки файлов в приложении IoT Central, а затем реализуете отправку файлов в коде устройства.

## <a name="prerequisites"></a>Предварительные условия

Для настройки отправки файлов требуются права администратора в приложении IoT Central.

Для хранения отправленных файлов требуется учетная запись хранения Azure и контейнер. Если у вас нет существующей учетной записи хранения и контейнера, создайте [новую учетную запись хранения в портал Azure](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM).

## <a name="configure-device-file-uploads"></a>Настройка отправки файлов устройств

Настройка отправки файлов устройств:

1. Перейдите к разделу **Администрирование** в приложении.

1. Выберите **Отправка файла устройства**.

1. Выберите учетную запись хранения и контейнер для использования. Если учетная запись хранения находится в другой подписке Azure из приложения, введите строку подключения к учетной записи хранения.

1. При необходимости настройте время ожидания отправки, определяющее время, в течение которого запрос на отправку остается действительным для. Допустимые значения: от 1 до 24 часов.

1. Щелкните **Сохранить**. Когда отображается состояние " **настроено**", вы можете отправлять файлы с устройств.

:::image type="content" source="media/howto-configure-file-uploads/file-upload-configuration.png" alt-text="Настройка отправки файлов в приложении":::

## <a name="disable-device-file-uploads"></a>Отключить передачу файлов устройства

Если вы хотите отключить передачу файлов устройства в приложение IoT Central:

1. Перейдите к разделу **Администрирование** в приложении.

1. Выберите **Отправка файла устройства**.

1. Выберите **Удалить**.

## <a name="upload-a-file-from-a-device"></a>Отправка файла с устройства

IoT Central использует функцию отправки файлов в центре Интернета вещей, чтобы разрешить устройствам отправлять файлы. Пример кода, демонстрирующий передачу файлов с устройства, см. в разделе [пример устройства загрузки файлов IOT Central](/samples/iot-for-all/iotc-file-upload-device/iotc-file-upload-device/).

## <a name="next-steps"></a>Дальнейшие шаги

Теперь, когда вы узнали, как настроить и реализовать передачу файлов устройств в IoT Central, предлагаемый следующий шаг — узнать больше о передаче файлов устройства:

- [Передача файлов с устройства в облако с помощью Центра Интернета вещей (.NET)](../../iot-hub/iot-hub-csharp-csharp-file-upload.md)
- [Передача файлов с устройства в облако с помощью Центра Интернета вещей (Java)](../../iot-hub/iot-hub-java-java-file-upload.md)
- [Передача файлов с устройства в облако с помощью Центра Интернета вещей (Node.js)](../../iot-hub/iot-hub-node-node-file-upload.md)
- [Передача файлов с устройства в облако с помощью центра Интернета вещей (Python)](../../iot-hub/iot-hub-python-python-file-upload.md)